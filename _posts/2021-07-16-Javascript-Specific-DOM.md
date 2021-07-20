---
layout: post
title: Javascript Specific DOM
date: 2021-07-16 11:38:00
---

앞에서 다룬 기본적인 내용들은 다 자바의 그것들과 비슷했다.  
하지만 앞으로 다룰 내용들은 자바스크립트의 특수성이라 할 수 있는 웹 개발 프론트엔드에 관한 내용이다.  

------------------------------

### 타이머

사실 혹자는 왜 바로 DOM manipulation을 다루지 않느냐고 물을 수 있다.  
이는 타이머, 이벤트 개념이 동적인 엘러먼트를 담당하는 하나의 축이기 때문이다.  
시간축이 없이 어찌 변화를 논할 수가 있는가?

자바스크립트에서는 몇초의 시간이 지나면 기능을 수행하는 SetTimeout과 몇초가 지날 때마다 같은 행동을 반복하는 SetInterval 두가지가 있다. 
```
setTimeout(<function>, <time in miliseconds>);

ex) 3초뒤 alert를 띄운다.
var timer = setTimeout(function() {
	console.log('콘솔 예시');
	alert('alert 예시');
}, 3000); 

// to remove timeout timer
clearTimeout(timer);

setInterval(<function>, <interval in miliseconds>);


ex) 매 3초마다 alert를 띄운다
var timer = setInterval(function() {
	console.log('콘솔 예시');
	alert('alert 예시');
}, 3000);

//to remove interval timer
clearInterval(timer);
```

--------------------------------

### 이벤트

이벤트란 다른 시스템 구조에서의 signal과 동일하다. 

OS 프로그래밍에서의 시그널은 시그널 핸들러가 보내진 시그널 종류에 따라 return memory address를 저장하고, stack의 다른 메모리 주소로 보내져 주어진 기능을 수행한 뒤, 원래의 user-space로 돌아오는 signal trampoline을 수행한다.  

자바 스크립트에서의 이벤트는 이벤트 리스너가 보내진 이벤트 종류에 따라 주어진 기능을 수행하는 것을 뜻한다. 사실 이런 이벤트 자체는 AHK같은 언어에서도 자주 볼 수 있다. key/mouse listener/hotkey, etc.

```
AutoHotKey나 Python의 이벤트 리스너처럼 JS의 이벤트리스너도 element와 같이 쓰인다. 이 말이 무슨 말이고 하니..

<element>.addEventListener(<event-type>, <function>);
의 식으로 쓰인다.

ex) 
var element = document.getElementsById('submit');
element.addEventListener('click', function() {

	// element has a style property which is just like css, the following syntax will be explained in a minute. Just keep reading

	element.style.color = 'red';
});
```
----------------

이벤트를 다뤘으니 이제는 HTML DOM을 JS에서 access하는 법을 알면 된다.
이 것만 알면 JQUERY로 넘어가도 나쁘지않다.  

### DOM

DOM element 중 몇몇 중요한 요소들은 document의 properties로 가져올 수 있다.
```
var element = document.body;
var element = document.cookie;
var element = document.images;
etc..
```

JS Selector로 DOM element를 select 할 수 있다.

셀렉터로는 
tag, id, class, css selector 등이 있다.

```
var element = document.getElementById(id);
var element = document.getElementsByClassName(class);
var element = document.getElementsByTagName(tag);
var element = document.querySelector(css-selector);

ex)
css-selector : .list .btn

var element = document.querySelector('.list .bin');
```

### DOM manipulation

위에서 가져온 element에게는 여러가지 properties 들이있다.  
이 중에는 css의 속성을 바꾸는 style property도 존재한다.  

```
document.getElementById('temp').style.color='red';
```
