---
layout: post
title: ANYMATE APP (1)
date: 2021-08-22 03:55:10
---

Project TEAMUP을 끝내기도 전에 만들고 싶은 앱이 생겼다.  

이름은 ANYMATE.  
최근에 자주 보이는 세션을 만들고 URL으로 접속하는 방식으로 구현해서, 세션을 만든 사람은 프로젝트 리더, 그 외에 들어오는 사람들은 프로젝트 맴버로 설정을 하는 식으로 일단 협업의 환경을 만드는 것이 기본적으로 제공되야 할 수 있는 것으로, 최근에 유튜버들이 하는 Gartic phone이나 aggie.io와 비슷하다고 보면 될 것 같다.  

이걸 구현하는 방법은 찾아내질 못했는데, 아마 웹 소켓을 사용할 거라고 생각한다.  

이런 기능을 가진 내가 봐왔던 웹사이트들은 다 항상 비슷한 특징이 있었는데, 로그인 기능이 기본적으로 제공되어 있다는 점.. (세션 내에서 유저를 각각 다르게 구분 지을 수 있게..) 그 외에 굳이 로그인을 안해도 될 경우에는 임시 이름이랑 태그를 지정해주는 경우가 태반이였다.  

즉, 클라이언트가 웹사이트 URL을 통해 들어갈 때, Get request의 parameter 부분을 가지고 세션을 구분짓고, 그 특정 세션에 임시이름 + 태그 등을 이용해서 유니크한 유저를 넣어서 (wss를 통해 서버와 연결) 그 때부터 채팅을 하든 데이터를 나누든 하는 것 같다.  

서버 입장에서는 그러면 get request를 받으면 request parameter 체크하고, 유저의 임시이름 + 태그가 방에 존재하나 안하나 체크? 해야 할 것 같고.. 없으면 wss 연결, 있으면 메세지를 보내야 할 것이다.  

그런데 아무래도 URL 세션이다보니까 방 자체의 보안성은 없을 듯 싶다.  
전문성으로 나아간다면 방을 잠구는 기능 등도 구현해야겠지 나중에.  

글이 좀 중구난방이고 두서가 없지만 일기라고 생각하며 적겠다.  

프레임워크는 node.js에다가 express 써서 소켓까지 한번에 구현하고 캔버스 기능들도 만드는게 솔직히 제일 좋아보인다.  

실제로 aggie.io builtwith으로 보니깐 node.js + express더라.  

그런데 나는 좀 요새 C#이 끌린다. 아무래도 비교적 최근에 JSP할 때 Java 했던 것도 있고 비슷하다보니깐.. 그리고 요 몇달간 웹 개발에는 ASP.NET CORE 쪽이 JSP를 재치고 인기가 1등이다.. OTL  
Blazor 기능도 솔직히 매력적이라서 다 떄려치우고 C# 올인하면 더 유능할 것 같다.  

그래서 ASP 입문차 이 웹사이트는 일단 처음에 ASP.NET FRAMEWORK로 webform으로 대충 만들어보고, mvc 그 후에 core로 넘어가서 직접 다른 클라우드 서버 환경이라던가 (linux 쓰는 AWS)에서 조금 더 싼 가격에 직접 돌려볼 수 있겠다.  

일단 webform 입문으로 첫 화면으로 navigation과 설명 정도는 만들어 놓은 다음에 바로 SPA로 넘어갈 듯 싶다.  

그렇게 크게 보면 세 가지 기능을 구현하고 싶다.  

----------------------------------

하나는 일단 프로젝트를 만드는건데, 이 부분이 쉽게 말하면 세션을 만드는 부분이 되겠다.  
웹사이트에서 버튼을 눌러서 서버에서 주는 세션 생성도 되겠지만, 반대로 URL에 그냥 랜덤한 문자열을 집어넣어서 세션을 찾아들어가거나 아니면 없을 경우 만드는 식도 괜찮은 것 같다.  

그런데 이 부분에서 고민해야 할 게, 세션이 없어져도 되냐는건데.. 예를 들어서 aggie.io는 세션 이름 들어갈 자리에 단어같은거 치면 다른 사람들 세션이 남아있는걸 자주 볼 수 있다.  

이 프로젝트가 사실상 애니메이터들의 전문성을 존중해야 할 텐데, 기능은 그렇다 치더라도 만들던게 날라가는 휘발성 자료이면 솔직히 아무래도 backlash가 엄청 날 것 같긴 하다.  

그러면 각 유저들의 프로젝트 내역들을 평생 저장하고, 그 프로젝트의 결과물을 또 저장하고, 하면 애니메이션의 특성상 용량이 어마어마해 질 것이다.  

하여튼 프로젝트를 만들면 처음 만드는 세션일 경우, 그 사람이 프로젝트 리더가 될 것이다. 그리고 그 이후 사람들은 다 프로젝트 맴버가 되면.. 이래서 휘발성일 수 밖에 없나.. 아 머리아프다.  

아니면 그냥 처음 시작 때 이름 정하는거랑 끝날 때 키 프레임 옮기고 넣고 하는거는 다 같이 정할 수 있게 해도 괜찮을 것 같다.  

방장 시스템도 가능하고 방을 닫는 시스템도 가능하다면 가능하다.  

내가 원하는 인터페이스는 2D로 화살표로 가고 싶은 키 프레임으로 가서 in-between을 추가하는거, 그리고 그걸 드래그해서 옮기는거 정도가 되는데, 여기까지가 프로젝트를 manage 하는 부분인 것 같다.  
그리고 추가하면 좋은게 맴버들이 다루고 있는 캔버스의 preview를 가끔 업데이트해줘서 들어가서 확인 할 수 있는정도?

--------------------------------------

다른 멤버의 preview를 통해 들어가면, 거기서 같이 그리거나 할 수 있으면 좋을 것 같다. 당연히 이상한 사람이 있을  수 있으므로, 이 때도 관리하는 시스템이 필요할 듯 싶다.  

일단은 기능이 중요하니까 이 부분은 나중에 좀 더 생각해보자..  

Html5 Canvas를 이용해서 같이 그리는 방식이 될 거 같은데, 기본적으로 웹 소캣으로 연결된 유저들이 어떤 좌표에 어떤 색깔/어떤 도구 를 사용하냐를 볼 것 같다.  

쉽게 말하면 wss로 클라이언트가 붓질을 할 때, 캔버스 상의 위치와 색깔을 받아서 서버로 data로 넘기면, 서버는 다른 클라이언트들에게 broadcast를 하고, 데이터를 받는 순간  클라이언트는 캔버스의 draw + update 함수를 실행 시켜서 같은 위치에 그림을 출력하는 걸 기능하면 될 것 같다.  

위의 data만 쓰면 처음 들어갈 때 클라이언트의 canvas는 무조건 빈 상태니까, 처음 들어갈 때는 png파일이나 그 전 stroke 데이터를 받아와서 모든 작업들을 미리 준비해 놔야 할 것 같다.  

------------------------------------------------

그렇게 그려진 그림들은 preview에서 하나하나 화살표를 통해서 볼 수 있어져야 될 거고, fps를 정한다음에 합쳐서 export 한 후, anymate 웹사이트의 post 부분에 퍼블리쉬 할 지 정하면 좋을 것 같다.  
유저에게서 직접적으로 파일을 넘겨받는거는 shell hacking을 염두하고 조심하면서 해야할 것이고.. 단지 위에 캔버스로 같이 그리는 부분에 애니메이션에 좋은 기능들을 어떻게 넣을지가 관건인 듯 싶다.  


이렇게 대충 브레인스토밍은 해봤지만, 개념적으로는 조금 그림이 잡혀도 직접 하려니까 막막한 감이 없지않아 있다.  
일단 하나하나씩 차근차근 해본다는 생각으로 다가가봐야 할 것 같다.  