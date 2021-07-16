---
layout: post
title: Javascript Array
date: 2021-07-15 19:29:00
---

�̹� ����Ʈ�� ����� ���� ���̴�.  
Javascript�� �迭�� �׷��� ũ�� �ٸ����� ����.  
���� �ٸ� �迭 ���� �Լ��� �����ϵ� �����ϴ� ���̴�.


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
var arr = arr.slice(n,m); -> [n,m)��°���� �ڸ���, ���� ����
arr.splice(n,m); n���� m�� ������, ���� �ջ�

slice�� splice�� Ư���� ���� python�� ����ϰ� negative indexing�� �� �� �ִٴ� ��.
i.e) -1�� length-1 �� ���� index.
����, length ���� ���� �ε����� index out of bounds ������ ������ ���� �ƴ϶�, ���� ū index�� ����.


arr.push(x, y, z, ..); <-> arr.pop(); 
arr.unshift(x,y,z,..); <-> arr.shift();

n��° �ڸ��� item �� �ֱ����ؼ� Python������ insert()��� �޼ҵ带 ����.
Python : arr.insert(n, item);

�ڹٽ�ũ��Ʈ������ splice�� ���� �ȴ�.
arr.splice(n,0,item);

```

### String

--------------------------

���ڿ��� ������ �迭�̴� �� ���ֿ� �ְڴ�.
�ڹٽ�ũ��Ʈ�� ���ڿ��޼ҵ���� �ڹٿ� ���� ����.

