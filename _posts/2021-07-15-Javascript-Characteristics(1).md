---
layout: post
title: Javascript Characteristics(1)
date: 2021-07-15 19:29:00
---

����Ʈ�� Jquery ���� ���� ����Ʈ �ϱ����� JS�� Ư¡�� ���� �÷��� �Ѵٴ� ������ �����.  

-----------------

### �ڹٽ�ũ��Ʈ?

������ �ڹٽ�ũ��Ʈ��..   

 HTML,CSS�� ���� DOM ��ü�� ������Ű�� FRONT-END�� Node.js���� ���� RDBMS�� ������ ����� ���� �����͸� �ְ� �޾ƿ��� BACK-END �� �� ��ȭ�� �� �ִ� ���� ����̴�.  

------------------- 

###  ���� Ư���� ���� ������ Ÿ��

����, Strongly typed�� �ƴ� ���μ�, var�̳� let���� ����(property)�� ���Ĺ����ٴ� ���� �� �����ϴٰ� �� �� �ִ�.  

�Լ�(method)�� function �̶�� prefix�� �̿�, strongly typed�� �ƴ����� return type���� ���� ��Ÿ���� �ʴ´�.  
�Լ� ��ü�� parameter�� ���� ���� ������, call back function�̶�� �Լ� ��ü�� �����μ� ������ �� �ִ� ������ �ִ�. �� �� ������ ���� ������ ������ ������ parameter �� �������� �Լ� �Ǵ� call back �Լ��� �̸����� ()�� ���� �ʴ´�.  
```
Javascript������ callback
<script>
//�Լ� �����
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
�ڹٿ����� �̿� ���� ����� �����ϱ� ���ؼ��� abstraction�� �̿��ؾ��Ѵ�.  
���� ���ؼ�, function�� �ϳ��� parameter�� ������ ���ؼ��� �� function�� ������ ��������� abstract �ϰ� ��Ÿ���ִ� Interface�� �ϳ� ������ �ϸ�, �׷��� ������� interface�� �ȿ� �޼ҵ�μ� call back function�� ����� ����� ����� ���� ���̴�.  
�ٸ� �޼ҵ忡 �������̽��� �־� �ְ� �ʹٸ�, ���� �������̽��� instance�� �ϳ� �����, �� instance�� parameter�� �־� ����Ѵ�.  
�ݴ�� �ٸ� �޼ҵ忡���� argument�� �� ����� �޾� ���� ���ؼ� �������̽��� �̸��� Ÿ������ �����Ͽ� ������ �޾ƿ���, �� ������ method�� �̿��ϴ� ����� �ִ�.  
```
Java������ callback(?)

// �������̽� ���� abstraction
interface Example {
	boolean isOdd(num) {
		return num %2 == 0  ? false : true;
	};
}
// �������̽� instance
Example e = new Example();

// passing instance as a parameter
public static... main (String args[]).. method(e, 10);

// method that takes the interface instance as an argument.
public boolean method (Example eF, int num) {
	return eF.isOdd(num);
}
```

�ڹٽ�ũ��Ʈ������ Object�� ����(property)�� �Լ�(method)�� ��� ���� �ϳ��� ��ü�̴�.
property�� �κ��� �ٸ� ���� �ϸ� k-v pair, �� �ٸ� ������ map�� �����ϴ�.
dot notation, key �κп� quote�� �ʿ����. �� bracket notation, key �κп� quote �� �ʿ����� ���� v�� ��� �� �� �ִ�. quote�� double-quote�� single-quote�� �Ѵ� ���� �� �ִ�. SQL�� ���ڿ��� ���� single-quote �� ����, � ������ double-quote�� ���ٴ� ���� �����ϸ� ��.. ���������.  
�Լ� �κ��� �ٸ� ������Ʈ, �� Ŭ������ �޼ҵ�� �����ϴ�. Ŭ������ �����ϱ⿡ python �� self, java�� this �� ������ this �� �����Ѵ�.
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
