---
layout: post
title: URL Path Variable and Query Parameter
date: 2021-08-08 21:12:00
---

### URL의 구조

우리가 동적인 웹페이지를 이용하다 보면 때때로 URL이 기존의 도메인 주소와 더불어 어지럽게 쓰여진 것을 확인 할 수 있다.  
이를 이해하기 위해서는 동적인 웹사이트의 URL의 도메인을 제외한 각 부분이 어떤 종류일 수 있는지를 아는게 중요하다.  
크게 보면 3가지 종류라고 볼 수 있다.  

- Route Parameters (Path Variables)
- Query Parameters
- Fragments (anchors)

### 서버에서의 Path, 라우팅

우리가 웹 사이트를 이용할 때 라우팅을 해주는 코드가 없이는 우리는 서버의 루트(root) 디렉토리에서 상대적인 위치의 파일을 브라우저를 통해 요청하게 된다.

```
예시) github.com/images/car-image-1.png
위 예시를 브라우저의 URL에 넣는다고 가정했을때,  github.com 부분은 도메인이니 제외하며, 나머지 부분은 그 특정 컴퓨터의 루트 디렉토리에서 images라는 디렉토리로 들어간 후, car-image-1.png라는 파일을 보여달라는 요청과 같다.
```

하지만 이는 이 웹 사이트를 만드는 사람의 입장에서는 조금 불편하다고 볼 수 있다.  
웹사이트의 모든 웹페이지 하나하나가 웹 서버의 폴더와 파일에 1대1로 정확히 일치해야 함으로 이는 상당히 불편할 수 밖에.  

라우팅을 해주는 코드가 있을 시에는 굳이 웹 서버의 정확한 PATH를 따르지 않더라도, 즉 어느 위치에 파일이 있을지라도, 라우팅을 통해 좀 더 통합적인 URL을 구성할 수 있다.

```
예시) github.com/images/car-image-1.png 를 
github.com/images/1 이라는 식으로 라우팅 해줄 수 있다.  
이럴 경우 유저가 github.com/images/1이라는 url을 브라우저에서 요청할 지라도, images/car-image-1.png를 얻어 올 수 있는것이다.

저 새로운 주소는 기존의 path와는 다른 routing 용 path이며, 1은 마치 parameter로써 라우팅 코드로 넘겨져서, 결과적으로 유저에게 적합한 페이지를 보내줄 수 있는 것이다.

이는 서버의 라우팅을 담당하는 코드에서 다음과 같이 parameter을 받아 올 수 있다.

Route Path = "images/:id"
```

이처럼 URL을 라우팅 해주는 부분의 코드에 이용되는 parameter을 routh parameter이라고 하며, 이는 당연하게도 resource를 식별하는 것에 사용된다 (여러가지 파일이 주어졌을때, 어느 파일이 적합한지).

### 동적인 웹페이지에서의 정보 이동

동적인(dynamic) 웹 페이지에서 유저는 서버와 당연하게도 상호작용을 할 수 있으며, 이 상호 작용은 몇가지 방법으로 몇가지 매체를 통해 진행된다.
  
유저가 요청을 보내고 응답을 받는 방법의 종류에는 Get과 Post 등이 있는데, 유저의 요청과 함께 보내질 정보/값들 또한 URL을 통해 보내질 수 있다.

이 URL을 통해 보내지는 정보는 Query Parameter이라고 불리며, Key-value pair로 이루어져야 한다.  
서버쪽에서 value를 통해 resource를 필터링하는데 쓰인다.  
이 query parameters들은 endpoint의 ?뒤에 나열되며, &를 통해 여러개의 parameter을 보낼 수 있다.

```
예시) 
github.com/users?gender=male&age=30
위와 같은 url에서 도메인을 제외하고 users라는 누가 보기에도 여러개의 결과값이 나올 것 같은 endpoint에서 query parameter을 통해 resource의 필터링을 진행하는 것을 볼 수 있다.

이는 식별과는 다른 개념인게, 식별은 아예 resource 자체를 다른 것을 찾는 것이고, 위의 상황은 resource 안에서의 결과값들을 필터링하는 것이다.
```

###  웹사이트의 요소를 특정

서버의 입장에서 유저가 자신의 웹 페이지를 들어왔다고 했을 때, 브라우저가 띄어둔 웹 페이지의 특정 요소를 이용해야 할 필요성을 느낄 때가 간혹 있다.  

이럴 때 URL에 fragment라는 것을 추가하면 이를 쉽게 첨가할 수 있다.  

fragment는 #element의 형식으로 오며 DOM element와 같은 것의 이름을 보내는 경우가 많다.  
이렇게 특정된 요소를 이용해서 웹 페이지의 스크롤을 특정 위치로 보내는 등의 작업을 할 수 있다.

```
예시) mdn.com/tutorials#content
위의 url에서 tutorials라는 endpoint에 content라는 특정 요소를 이용, 스크롤을 content가 있는 부분까지 내리는 등의 작업을 수행할 수 있다.
```
