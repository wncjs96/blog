---
layout: post
title: Javascript Array
date: 2021-07-15 19:40:00
---

이번 포스트는 쉬어가는 정리 글이다.  
Javascript의 배열(Array)은 Python의 배열(List)과 그렇게 크게 다른점은 없다.  
그저 다른 배열 관련 함수를 정리하듯 정리하는 것이다.


### Array 

------------------

```
var arr = [];
var arr = ['apple', 'banana', 'cherry'];
var arr = new Array();

var len = arr.length;
var str = arr.join('<delimiter>'); -> a-b-c
var arr = arr.reverse();
// sort with comparator
var arr = arr.sort(function(a,b) { return b - a }); 
var arr = arr.concat(arr2);
var arr = arr.slice(n,m); -> [n,m)번째까지 자르기, 원본 유지
arr.splice(n,m); n에서 m개 빼내기, 원본 손상

slice와 splice의 특이한 점은 python과 비슷하게 negative indexing을 쓸 수 있다는 점.
i.e) -1은 length-1 와 같은 index.
또한, length 보다 높은 인덱스는 index out of bounds 에러를 던지는 것이 아니라, 제일 큰 index를 쓴다.


arr.push(x, y, z, ..); <-> arr.pop(); 
arr.unshift(x,y,z,..); <-> arr.shift();

n번째 자리에 item 을 넣기위해서 Python에서는 insert()라는 메소드를 쓴다.
Python : arr.insert(n, item);

자바스크립트에서는 splice를 쓰면 된다.
arr.splice(n,0,item);

```

### String

--------------------------

문자열은 문자의 배열이니 이 범주에 넣겠다.
자바스크립트의 문자열(String)메소드들은 자바의 문자열(String)메소드들과 거의 같다.  
이 말이 무슨 말이고 하니..  

자바에서는 문자열을 효과적으로 수정하기 위해 StringBuilder라는 클래스를 사용한다.   
StringBuffer for thread-safety;  
String s = a + b; 라는 짧은 assignment statement조차도 사실은 StringBuilder로 convert 되어 컴파일되는 경우도 있다. 디벨로퍼가 따로 정하지 않아도 말이다.  
```
String s = a + b;
Stringbuilder s = new StringBuilder(a);
s.append(b);
s.insert(a.length()-1, b);
s = s.reverse();

```

자바스크립트는 이와 다르게 String 한 가지의 오브젝트를 사용한다. 그렇기에 StringBuilder에서 제공하는 여러가지 유용한 built in method와 benefit은 없다고 보면 된다. 그렇다 해도 reverse 정도가 유용하지만 말이다. 메모리 부분에서의 garbage collection 전 heap 이 꽉 차버리는 부분도 포함한다. 즉, 자바의 String과 비슷하다는 말이다.

이렇기에 String 자체를 바로 reverse 하는 static method 또한 존재하지 않는다.
```
// string을 array로 바꿔준다 -> 자바의 .toCharArray(), split()과 동일
var array = str.split("");
// Python의 구조를 가진 어레이를 reverse 해주는 method 사용.
var reverseArr = array.reverse();
// Python의 구조를 가진 어레이를 합쳐주는 join 사용.
var joinArray = reverseArr.join("");

```
이 과정은 자바의 그것과 거의 동일하다.

```
또 다른 예시로는
var ind = str.indexOf('x'); //자바와 동일
var str = str.substring(n,m); //자바와 동일
var up = str.toUpperCase()/toLowerCase(); // 자바와 동일
var text = "        x ".trim();      // 자바와 동일
var chr = str.charAt(0); // 자바와 동일
마지막으로 조금 다른 부분인 length가 있다.
자바스크립트의 length는 property로서 존재한다.
반대로 자바의 length는 method로서 존재한다.
var len = str.length vs int len = str.length();
하지만 이것조차도 파이썬의 len()메소드와는 확연히 다르다.
```

반대로 Strongly typed인 자바와는 다르게 자바스크립트는 딱히 casting을 해야 할 상황이나 data type conversion같은 일은 일어나지 않는다.

자바에서는 Integer을 String으로 바꿀때 String.ValueOf(num), String을 Integer로 바꿀때 Integer,ValueOf(str) 등의 class들의 static, class methods를 사용하여 바꿔주는 일이 허다하다.
