---
layout: post
title: Javascript Array
date: 2021-07-15 19:40:00
---

이번 포스트는 쉬어가는 정리 글이다.  
Javascript의 배열은 그렇게 크게 다른점은 없다.  
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
자바스크립트의 문자열메소드들은 자바와 거의 같다.

