---
layout: post
title: ANYMATE APP (2)
date: 2021-08-23 11:55:10
---

ANYMATE의 핵심 기능인 Collaborative Project and Drawing을 하기엔 웹 소켓이 필요하다 했다.  

하지만 ASP.NET Framework를 사용한다고 마음먹은 지금, Web Socket을 그대로 이용하는 것보단 SignalR을 사용하는게 더 낫다고 볼 수 있다. 왜냐하면 SignalR은 Web Socket을 포함한 추상화를 제공하는 ASP.NET이 추천하는 RPC 방식이기 때문이다.  

분명히, 대학교에서 Socket Programming을 배울땐 Low level로 하나 하나 설정해 chat server/client 같은 것을 생성한 적이 있다.  
그리고 분명 Socket Programming은 굉장히 강력하고 목적에 따라서는 일반적으로 굉장히 좋은 방법인 것은 확실하다.  

하지만 웹 브라우저와 서버와의 통신에 사용되는 Web Socket을 이용할 때는, 브라우저의  종류라던가, 환경이라던가, 모바일에서 쓰이는가라던가 등등 Web Socket 자체가 비교적 최근 기술이기에 아예 호환조차 안되는 경우가 있다.  

이렇기에 마치 css에는 modernizer이 있듯, RPC에는 SignalR이 있는 것이다.  

------------------------------

SignalR은 앞서 말햇듯, Web Socket을 포함한 RPC 방식의 Abstraction이라고 보면 편하다.  

SignalR은 RPC 4가지 기능들을 한 데 모아서 필요한 목적에 따라서 그 4가지 중의 한가지 방식을 ASP.NET이 결정해서 써준다고 보면 된다.  

그리고 그 4가지 방식은 다음과 같다.  

- Web Sockets
- Server Sent Events - SSE (Event Source)
- Forever Frame
- Long Poll (AJAX에 Poll(unblocking)을 끼얹은 형태)

Web Socket이 내가 구현하려는 웹 앱의 목적 (클라이언트와 서버간의 push, request가 매우 잦은 경우) 과 매우 잘 맞기 때문에, Long Poll 부터 시작해서 Web Sockets 까지 간략하게 설명하도록 하겠다.  
Long Poll을 설명하기에 앞서는 AJAX를 설명하고 가면 매우 편하다.  

---------------------------------------------------

### HTTP 개념 복습 (대충)

기본적으로 웹 클라이언트 - 웹 서버는 HTTP 프로토콜이라는 통신을 사용한다.  
주소가 http로 시작하는 이유가 그 이유인데, 이 HTTP의 특징은 stateless 하다는 것이다.  
Stateless 하다는 것은 클라이언트가 서버에게 뭔가를 요청하면, 요청 할 때마다 서버는 그 클라이언트가 몇초 전에 요청을 보냈었나, 클라이언트가 무슨 요청을 보냈었나, 이런 거를 아예 기억 못하는 기억 상실증에 걸린 것 마냥 행동한다는 뜻이다.  

자, 어쨋든, 이런 식으로 우리의 브라우저(클라이언트)는 서버와 기본적으로 http를 사용하기에 우리 눈에 보이는 몇가지 특징 들이 있다.  
제일 눈에 잘 보이며 HTTP의 특징을 잘 보여주는 기능은 새로 고침이라는 기능이라고 할 수 있다.  
너무나 자연스럽게 적응이 되어서 별로 이상한 점도 모르겠는 새로고침이라는 기능은 HTTP를 써서 서버로부터 웹 페이지를 받아오는 클라이언트의 특징 때문에 존재한다고 생각하면 편하다.  

예를 들어서, 우리가 어떤 블로그를 보고 있다고 하자.  
만약 그 블로그에 글을 쓰는 주인이 어떠한 새로운 글을 올렸다면? 그리고 이 블로그에서는 HTTP의 기능만 사용한다면?  

기존의 HTTP를 쓴 우리의 브라우저는 블로그를 보려고 URL에 블로그 주소를 쳤을 때, 서버에게서 블로그의 웹 페이지를 받아와서 브라우저 상에 렌더링을 한 경우이기에, 블로그 주인장이 서버에 어떤 변화를 주었다고 해도, 다른 connection이 있지 않는 이상, 클라이언트의 브라우저에는 아무 일도 일어나지 않는다.  
하지만 새로고침 버튼을 누르는 순간, 다시 서버에게서 페이지를 받아오기에, 변화가 적용이 된 페이지를 다시 받아 오게 된다.  

당연하게도 이 방식은 매우 불편하다.  
우리는 클라이언트가 서버에 변화가 생겼을 때 바로 업데이트가 되기를 원한다.  
그리고 이 방식으로 하나 제안되었던게 바로 이제 다룰 AJAX이다.  

------------------------------------

### AJAX : Asynchronous Javascript XML

AJAX는 이름만 풀이해보자면 비동기적 자바스크립트 그리고 XML이다.  
AJAX가 비동기적이라는것은 AJAX가 실행되는 동안에 원래 클라이언트 브라우저가 멈추지 않는다는 것이다.  
즉, AJAX의 기능이 실행되는 동안에 원래 클라이언트 브라우저에서 행할 수 있는 form이라던가 submit, redirect 같은 기능들이 다 실행 가능하다는 것이다.  

Javascript XML이 붙은 이유는 Javascript를 통해서 XML이라는 데이터 transition에 특화된 언어를 이용해 데이터를 주고받는다는 것을 뜻한다.  

즉, 기존의 HTTP에서 페이지 정보를 받고 stateless, connectionless로 서버와 클라이언트가 단절된 환경에 존재하게 만드는 시스템에 또 다른 데이터를 주고 받을 수 있는 방법을 제공해줌으로써, 기존 HTTP의 정적인 환경에서 벗어나 조금 더 동적인 환경을 제공해준다고 보면 된다.  

이 AJAX를 쓰면 앞서 다뤘던 예시인 블로그에서 블로그 주인장이 포스팅을 했을 때, 블로그를 보던 클라이언트의 브라우저에서 포스팅에 대한 데이터를 받아서 새로고침 없이도 웹 페이지를 업데이트 한다던가 같은 기능을 추가할 수 있다.  

하지만 "블로그 주인장이 포스팅을 했을 때" 라고 표현했음에도 불구하고, 사실 이 데이터 통신의 시작은 항상 클라이언트가 해야 한다는 특징이 있다.  
다시 말해서, 서버에서 알려줘야 할 클라이언트들을 기억해 뒀다가, 새로운 변화가 생겼을 때 서버쪽에서 먼저 나눠서 알려주는 것 (push)이 아니고, 클라이언트에서 시간이 어느정도 지나거나, 어떤 버튼이 눌리는 이벤트가 생겼을시에, 서버에게 혹시 새로운거 뭐 없니? 하고 요청을 하고, 서버쪽에서 있다, 없다에 맞춰서 XML/JSON 데이터를 날려주는게 AJAX라고 보면 된다.  

알아보기 쉽게, 데이터 통신은 다음과 같이 진행된다.  

```
클라이언트:  (3초마다) "새로운 데이터 있니?"
서버: . . . timeout
클라이언트:  (3초마다) "새로운 데이터 있니?"
서버: {"answer": "no"}
클라이언트:  (3초마다) "새로운 데이터 있니?"
서버: {"answer": "no"}
클라이언트:  (3초마다) "새로운 데이터 있니?"
서버: {"result": {"id": "1", "loc": "VA"}}
```

보자마자  문제점이 보이겠지만, 클라이언트 쪽에서 몇초마다 물어줘야 한다는 구조상의 비효율성, 서버쪽으로 데이터를 요청할 때마다 발생하는 오버헤드, timeout이 일어날 수 있는 낮은 의존도, 새로운 데이터가 없을 때 나타나는 비효율성 등이 있겠다.  

이와 같은 단점들을 좀 보완한게 Long Polling 이다. 

----------------------------------------

### Long Polling

Long polling은 AJAX와 매우 비슷하게 클라이언트 쪽에서 요청을 함으로써 시작한다고 보면 된다.  
하지만 AJAX와는 다른점은 서버가 데이터가 없을 때는 데이터가 생길 때까지 답을 안한다는 점이 있겠다 (polling).  
클라이언트는 이 맨 처음 요청을 보낸 이후는 서버가 답을 할 때 까지 요청을 보내지 않는다.  

이렇게 하면 몇초마다 물어줘야하는 비효율성이 없어지고, 서버쪽에 데이터를 요청할 때의 오버헤드가 확연히 줄며, timeout이 일어날 가능성이 줄고, 새로운 데이터가 없을 때는 서버쪽에서 polling으로 기다려주기에 비효율적인 부분이 사라진다.  

----------------------------------------

### Forever Frame 

AJAX와 Long Polling이 클라이언트 쪽에서 요청을 하고 답을 받아오는 형태였다면, 앞으로 다룰 녀석들은 서버쪽에서 푸쉬를 해서 클라이언트로 정보를 한방향으로 넘겨주는 녀석들이다.  

Forever Frame은 HTML의 iframe을 이용한다.  
iframe은 페이지 내에서 또 다른 서버와 connection을 만드는것, 다시 말해서 페이지 내에서 작동하는 또 다른 페이지라고 생각하면 편하다.  
이런 특징때문에 실생활에서 제일 접하기 쉬운 iframe은 배너로 뜨는 광고를 보면 편하다.  

배너 광고를 보면 불법 사이트 같은 경우 배너를 클릭 하지 않더라도 마치 배너광고를 클릭한 것 마냥 작동하는 경우가 있는데, 이게 자바스크립트 클릭 같은 것을 이벤트 핸들링해놔서 그런 것이 있고, 같은 브라우저의 보는 화면 내에 두 개의 페이지가 켜져있는 상태인지라, 유저가 타이핑하는 내용같은 것도 intercept할 수 있는, 보완상에서 보면 무섭게 작동할 수 있는 녀석이다.  
하지만 iframe의 부모 사이트의 쿠키나 세션같은 정보를 탈취할 수는 없으니, 유저가 조심하면 그 때부터는 그렇게 위협적인 것도 없는것이 사실이다.  
(허나 자바스크립트가 실행되는 특성상, 여느 웹사이트와 마찬가지로, 유저 인풋과 상관없이 나쁜 코드가 실행될 수 있다는 근본적인 문제는 존재한다. )

어쨋든, iframe은 이런 식으로 다른 웹 페이지의 내부에서 다른 서버와 connection을 만들 수 있는, 즉 다른 서버로의 URL을 가지고 있는 HTML 요소이다.  

그리고 이렇기에, iframe을 통해 연결된 서버는 클라이언트에게 announce()같은 단방향 function을 실행시켜 데이터같은걸 전해줄 수 있게 되는 것이다.  
그리고 당연하게도, 이 방법에 클라이언트의 요청은 필요가 없다.  

그리고 이제 iframe을 통해 완벽히 데이터를 받을 수 있다는 걸 이해한 지금 시점에, forever frame의 이름을 풀이하자면, iframe이 클라이언트상에 계속 존재하며 서버가 데이터를 자기가 원할 때 보내면 언제든지 이용된다는 뜻이다.  

이 Forever Frame의 단점은 바로 iframe으로써 클라이언트의 코드에 "존재"한다는 것이다.  
그렇다. 존재가 바로 단점이다. 이렇게 평생 존재하는 iframe은 데이터를 보내고나서 메모리를 많이 차지하게 되는데, 이것에 대한 low-level 해법은 데이터를 보낸 후 차지되는 메모리를 free해주는 것이다.  
그리고 또 당연하게도, iframe은 html의 요소이므로, iframe을 제공하지 않는 애플리케이션같은 경우는 forever frame을 이용하지 못한다.  
그리고 서버가 클라이언트에게 단방향으로 보내주는 부분은 내가 만들려고 하는 채팅 서버/클라이언트 라던가 이런 부분에 쓰이기 매우 불편하다.  

------------------------------------------------

### Server Sent Events - SSE (Event Source)

SSE 혹은 Event Source 또한 Forever Frame 처럼 서버에서 클라이언트로 단방향 통신을 만들어낸다.  

이게 작동하는 원리는 서버가 클라이언트에게 보내는 Content Type을 바꾸는 것에 있는데, 원래 html같은 정보를 보낼때는 text/html, 자바스크립트 같은 경우는 text/javascript라는 content type을 명시하는데, 이 event source는 content type을 text/event-stream이라는 것으로 명시함으로써 클라이언트가 그 컨텐트 타입을 받자마자 서버-클라이언트 persistent connection을 만들고, listening을 하기 시작한다.  
그러면 이 데이터 스트림은 그 단방향 컨넥션을 통해 클라이언트에게 전달되게 된다.  

------------------------------------------------

### Web Sockets 

웹 소켓은 소켓 프로그래밍에 기초한 비교적 최근의 기술이고 내가 구현하려는 채팅 서버/클라이언트, 클라이언트에서 보내는 정보를 다른 클라이언트에게 그대로 혹은 가공 후 전해주는 것에 유용한 bidirectional, full-duplex communication이다.  

먼저 클라이언트는 서버에게 웹 소켓을 사용하자고 요청한다.  
서버가 요청을 안전하게 받고 accept를 하게 되면, 클라이언트와 서버간에는 persistent connection이 형성되는데, 좋은 점은 이 persistenet connection은 한 서버에서 여럿 클라이언트와 함께 동시에 형성 할 수 있다는 점이다.  
한 세션에 여러 클라이언트가 있을 시에 매우 좋은 조건이다.  
그리고 이 connection은 서버나 클라이언트 둘 중 한명이라도 끊을 시 사라지는데, 클라이언트가 브라우저를 닫거나 해도 알아서 이 컨넥션을 끊어준다.  
즉, 예전에 온라인, 오프라인 state를 만들어야 했을 시, 클라이언트의 요청이 안 온지 몇분이 지났나 체크할 필요 없이, 웹 소켓을 통해 연결을 했다면, 온라인 오프라인 state는 클라이언트가 그 컨넥션에 존재하나의 유무를 통해 쉽게 정할 수 있다는 뜻이다.  

그리고 위의 과정들은 일련의 프로토콜이고, ws protocol을 찾아보면 더 자세히 알아볼 수 있다.  

당연하게도, signalR을 쓰는 이유가 그러하겠지만, 비교적 최신 기술이니 아주 옛날 브라우저들은 ws protocol을 지원하지 않는 경우가 있다.  

------------------------------------------------------------

### SignalR

SignalR은 위의 설명 모두를 포함해 유저가 알게 모르게 바꿔가며 상황에 맞는 방법을 사용해준다.  

닷넷 프레임워크에서는 이 signalR을 쓰기 위해 두 가지 클래스를 제공해주는데, Hub과 Persistent Connection이 두개가 있다.  

Hub은 조금 더 Higher level이고 쓰기 편하다. 마이크로소프트에서 추천하는 방식도 Hub을 사용하는 것이다.  

그리고 우리가 쓸 signalR 오브젝트는 이 Hub을 inherit 함으로써 사용할 수 있는데, C#의 문법으로는 class MyClass : Hub 과 같은 식으로 쓰면 바로 Hub의 특성을 가진 자식 클래스를 사용할 수 있다.  

SignalR을 사용하기 위해선 설치를 해야하는데, 비쥬얼 스튜디오를 킨 후에, 솔루션의 프로젝트 내부 참조(references) 부분에 SignalR을 추가해줘야 한다.  
이를 위해선, view > other windows > package manager console 을 눌러서 터미널에 Install-Package Microsoft.Aspnet.Signalr 을 쳐서 설치를 진행해도 되고,  
마치 nodejs의 npm처럼 닷넷에서 제공하는 Nugget Package manager을 이용해도 된다. 이를 위해서는 프로젝트의 오른쪽 클릭 후에, Nuget Package Manage를 눌러서 explore 부분에 Microsoft.aspnet.signalR을 쳐도 된다.  

패키지는 닷넷 프레임워크를 쓰는지, 닷넷 코어를 쓰는지를 잘 구분하고, 될 수 있으면 마이크로소프트에서 제공하는 코드를 사용하는게 좋다.  

설치가 끝나면 References 부분에 Microsoft.AspNet.SignalR ... 이 추가된 것을 확인할 수 있다.  

이 SignalR을 사용하기 위해선 첫번째로, 이 웹 애플리케이션이 signalR을 사용할 것이라는걸 명시해줘야한다.  

이를 위해선 app의 startup에 실행되는 cs파일에 hub를 register 하는 작업이 필요한데, 이를 위해서 webform의 global.asax(옛날 방식의 startup 파일) 혹은 Startup.cs 파일(owin middleware을 쓰는 방식)을 고쳐줘야 한다.  

```
Global.asax.cs

protected void Application_Start()
{

   RouteTable.Routes.MapHubs();
}

startup.cs

public class Startup
  {
    public void Configuration(IAppBuilder app)
    {            
        app.MapSignalR();
    }
  }
``` 

Nuget Package Manager을 통해 깔리는 패키지 중 Owin도 있으니, Startup.cs파일을 새로 만들고, 기존의 Global.asax에 존재하는 내용을 옮겨 startup.cs파일만 남기는 방식도 있다.  

어쨋든, 이렇게 hub이 register 되면, signalR을 script로 추가해 줄 수가 있게 되는데, 이는 master페이지의 asp:scriptReference, 혹은 App_Start에 존재하는 BundleConfig.cs에 추가해주거나, 만약 웹폼이 아닌 mvc같은 경우 view > shared 에 존재하는 layout의 하단에 razor태그로 scripts.render을 통해 src를 추가하는 곳에 가서 스크립트를 추가해주면 된다.  

나같은 경우는 웹폼을 쓰기에,
```
                <asp:ScriptReference Path="~/Scripts/jquery.signalR-2.4.2.min.js" />
                <asp:ScriptReference Path="~/signalr/js" />
```
위의 내용을 scriptManger에 추가해줬다.  

이 다음은, Hub를 inherit하는 클래스를 만들어주는 것인데, 프로젝트에 오른쪽클릭 > class 추가를 해준다.  

추가된 클래스에 inherit를 추가해주기 위해서 C#에서는 : parentClass를 해주면 되는데, 이 때, namespace가 다르므로,  Microsoft.AspNet.SignalR.Hub를 써주거나, 아니면 using 구문에 Microsoft.AspNet.SignalR을 추가해서 바로 : Hub를 해줘도 된다.  
이런 Dependency 같은 문제인 경우는 Hub에 커서를 두고 Ctrl+. operator을 써서 자동으로 해결해도 된다.  
이렇게 기존의 기능은 유지한채로 읽기 편하거나 간편화 시켜서 코드를 바꿔주는 것을 Refactoring이라고 부른다.  

-------------------------------------

이렇게 셋업이 끝난 후에는, 앞서 말했던 데이터를 보내고 받는 방식을 구현해 내면 된다.  
이는 웹 소켓과 같은 방식으로 진행되는데, 개념상으로 알아야 할 것은 다음과 같다.  

- 클라이언트 쪽 코드는 Javascript (Jquery)를 써서 작성한다.  
- 서버 쪽 코드는 C#을 써서 작성한다.  
- 클라이언트 쪽 코드는 IIFE (Immediately Invoked Function Expression) 을 사용하여 작성해, scope의 accessibility를 관리해주는 것이 좋다.  
- 서버쪽 코드는 Hub를 inherit 하는 클래스 안에 메소드를 생성해 진행되는데, 이 때, 서버 쪽 코드는 C#이므로 메소드의 이름은 PascalCase를 따른다. 이 메소드는 그대로 클라이언트에서 이용되는데, 이 때, 클라이언트는 자바스크립트이므로 camelCase를 따른다.  
- 시작은 클라이언트 코드에서, connection object를 만드는 것으로 시작하는데, generatedProxy를 쓰거나, 아니면 그냥 만들어도 된다.  
- 그렇게 만들어진 object 는 server, client와 같은 property가 있는데, object의 server의 C#에 쓴 메소드는 자바스크립트에서 camelCase로 이용할 수 있고, 이렇게 C#의 서버사이드로 넘어간 데이터는 javascript의 object의 client property의 메소드를 만들어주고, 이걸 C# 서버쪽에서 PascalCase로 이용함으로써, 다른 클라이언트들에게 보내줄 수 있다.  
- object 의 client property를 통해 만드는 메소드는 callback function을 통해 전달받은 data를 클라이언트 브라우저 상에서 이용할 수 있게 된다.  

이 개념을 이용한 예제 코드는 다음과 같이 쓸 수 있게 된다.  

```
(.net framework webform)
.master의 스크립트를 추가해주기 위해서, script manager을 바꾸거나, 아니면 끝 부분에 content placeholder을 배치하는 방법이 있다.  
Project.Master 
<asp:ContentPlaceHolder ID="ScriptHolder" runat="server">
</asp:ContentPlaceHolder>

index.aspx
<asp:Content ID="ScriptContent" ContentPlaceHolderID="ScriptHolder" runat="server">
    <script src="./Scripts/jquery.signalR-2.4.2.min.js" type="text/javascript"></script>
    <script src="./signalr/js" type="text/javascript"></script>
    <script src="./Scripts/SignalR.js" type="text/javascript"></script>
</asp:Content>

index.cshtml (core)
layout의 @RenderSection("Scripts", required: false)을 교체해주기 위해, 다음과 같이 작성한다.  
@section scripts {
	<script src="~/Scripts/jquery.signalR-2.2.0.min.js"></script>
	<script src="~/signalr/js"></script>
	<script src="~/Scripts/SignalR.js"></script>
}

js client side code
(function () { 
	// connection object made with generated proxy
	var myHub = $.connection.myHub;
	$.connection.hub.start()
	.done(function() {
		// data goes to server side
		myHub.server.announce("Connection Done");
	})
	.fail(function() {
		alert("Error connecting to signal");
	})

	// data taken by clients
	myHub.client.announce = function (message) {
		$("#chat-room").append(message);
	};
})();

C# server side code

using Microsoft.AspNet.SignalR;

namespace PackageName
{
	public class SessionHub : Hub
	{
		public void Announce(String data) 
		{
			// data goes to clients 
			Clients.All.Announce(data);
		}
	}	
}
```

이렇게 클라이언트 전체에 보내는 방식도 있겠지만, 당연하게도, 특정 유저, 그룹에 보내는 방식도 존재한다.  

----------------------------------