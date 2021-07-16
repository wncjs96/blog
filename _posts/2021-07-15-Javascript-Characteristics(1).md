---
layout: post
title: Javascript Characteristics(1)
date: 2021-07-15 19:29:00
---

리액트나 Jquery 관련 글을 포스트 하기전에 JS의 특징에 대해 올려야 한다는 생각이 들었다.  

-----------------

### 자바스크립트?

나에게 자바스크립트란..   

 HTML,CSS와 같은 DOM 자체를 수정시키는 FRONT-END와 Node.js등을 통해 RDBMS와 직접적 통신을 통해 데이터를 넣고 받아오는 BACK-END 둘 다 소화할 수 있는 만능 언어이다.  

------------------- 

###  조금 특별한 기초 데이터 타입

또한, Strongly typed가 아닌 언어로서, var이나 let으로 변수(property)를 퉁쳐버린다는 것이 참 편이하다고 볼 수 있다.  

함수(method)는 function 이라는 prefix를 이용, strongly typed가 아닌지라 return type같은 것을 나타내지 않는다.  
함수 자체를 parameter로 보낼 수도 있으며, call back function이라는 함수 자체를 변수로서 저장할 수 있는 구조도 있다. 이 두 가지는 같은 구조를 가지기 때문에 parameter 로 보내지는 함수 또는 call back 함수의 이름에는 ()가 들어가지 않는다.  
```
Javascript에서의 callback
<script>
//함수 만들기
function isOdd(num){
	if (nun % 2 == 0) return false;
	return true; 
}

function someother(isOddF, num) {
	return isOddF(num);
}
someother(isOdd, 1);

</script>
```
자바에서는 이와 같은 기능을 구현하기 위해서는 abstraction을 이용해야한다.  
쉽게 말해서, function을 하나의 parameter로 보내기 위해서는 그 function의 구조를 어느정도는 abstract 하게 나타내주는 Interface를 하나 만들어야 하며, 그렇게 만들어진 interface의 안에 메소드로서 call back function과 비슷한 기능을 만들어 놓는 것이다.  
다른 메소드에 인터페이스를 넣어 주고 싶다면, 따로 인터페이스의 instance를 하나 만들고, 그 instance를 parameter로 넣어 줘야한다.  
반대로 다른 메소드에서는 argument로 그 기능을 받아 오기 위해서 인터페이스의 이름을 타입으로 지정하여 변수를 받아오고, 그 변수의 method를 이용하는 방식이 있다.  
```
Java에서의 callback(?)

// 인터페이스 구조 abstraction
interface Example {
	boolean isOdd(num) {
		return num %2 == 0  ? false : true;
	};
}
// 인터페이스 instance
Example e = new Example();

// passing instance as a parameter
public static... main (String args[]).. method(e, 10);

// method that takes the interface instance as an argument.
public boolean method (Example eF, int num) {
	return eF.isOdd(num);
}
```

자바스크립트에서의 Object는 변수(property)와 함수(method)를 모아 만든 하나의 객체이다.
property의 부분은 다른 말로 하면 k-v pair, 즉 다른 언어들의 map과 동일하다.
dot notation, key 부분에 quote가 필요없음. 과 bracket notation, key 부분에 quote 가 필요함을 통해 v을 얻어 낼 수 있다. quote는 double-quote와 single-quote가 둘다 쓰일 수 있다. SQL은 문자열을 위해 single-quote 만 쓰고, 어떤 언어들은 double-quote를 쓴다는 점을 생각하면 참.. 답답해진다.  
함수 부분은 다른 오브젝트, 즉 클래스의 메소드와 동일하다. 클래스와 동일하기에 python 의 self, java의 this 와 동일한 this 가 존재한다.
```	
	var obj = new Object();
	var obj ={};
	
	obj.age = 10;
	obj.["name"] = "name";	
	
	var obj={
		// property
		age: 10, name = "name"

		// method
		getAge:function() {
			return this.age;
		}

		// abstraction, callback function
		getAge:getAge
	};
	// callback function
	function  getAge() {
		return this.age;
	}
```
