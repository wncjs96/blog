---
layout: post
title: Algorithm Study
date: 2021-07-09 17:09:00
---

## 알고리즘에서 고려해야 할 점
---------------

어떤 알고리즘을 쓸 지 정하는 것보다 더 중요한 것은 없다.

어떠한 문제점이 주어졌을때, 모든 Case에서 돌아갈 수 있는 해법을 내는 방법은 주어질 수 있는 모든 case의 해법을 줄 수 있는 알고리즘을 쓰는 것이다.

그리고 그 알고리즘은 컴퓨터가 해결해낼수 있는 정도의 효율성을 가지고 있어야 할 것이다.

Time Complexity 와 Space Complexity가 컴퓨터가 이에 제일 연관성이 높다고 볼 수 있다.

Edge cases들을 제외하고 인풋의 특징을 잘 나타내는 것 중 하나는 Constraint 가 있다.

다음은 Input n에 관하여 컴퓨터가 취급할 수 있는 Time Complexity를 정리해 둔 것이다.

----------------------------
보통 컴퓨터가 1초에 처리할 수 있는 양은 인풋이 10^6 ~ 10^7 보다 작을 때이다.  
그렇기에 1초에 어떤 처리를 할 수 있는 효율 좋은 알고리즘을 만들려 한다면, 위의 문장을 다르게 보면 된다.  
만약 인풋이 10^3 정도로 작다면, 10^6를 10^3으로 꽉 채울때까지의 비효율적인 알고리즘은 써도 된다는것이다.  
이는 10^6 >= (10^3)^2를 이용해 O(n^2) 까지의 알고리즘은 써도 된다는 뜻으로 이해된다.   
즉, 10^6 = 컴퓨터의 1초라는 것만 기억하면 된다.  
그렇게 말해도 이해가 안될 경우는 밑의 표를 외우자.  

- n <= 25 일 시 O(2^n)
- n <= 100 일 시 O(n^4)
- n <= 500 일 시 O(n^3)
- n <= 10^3 일 시 O(n^2)
- n <= 10^5 일 시 O(n log(n))
- n <= 10^6 일 시 O(n)
- n > 10^9 일 시 O(1) 혹은 O(log(n))

이래도 이해가 안된다면, 예시를 하나 띄우겠다.

만약 어떠한 프로그램이 인풋으로서 2<=n<=10^5 를 받는다면, 어떠한 알고리즘이 적당한가?

일단 소거법으로 n^2 이상은 다 못한다고 봐야한다. 

즉 이것은 n log n 수준의 기본적인 라이브러리로 제공되는 sort (보통 merge sort), 아니면 아예 linear time으로 인덱스가 하나인 상태로 한번만 돌던가, 두개로 양쪽 끝에서 오던가 하는식으로 생각 할 수 있다.

잠깐! n log n 에 대한 팁  
Binary Search 와 같이 어떤 리스트를 반으로 나눠서 진행하는 형태는 log(n)의 시간을 쓴다. 이는 반으로 나눠서 진행을 할 시 매 순간 1/2를 보지 않기 때문이다.
위의 Divide and Conquer 방식에서 merge 까지 할 경우 log(n)은 n번을 반복해 총 n log(n)의 시간을 쓰게 된다.  
즉 쉽게 말하면, log(n)의 방식을 쓰는 알고리즘이 전체 리스트를 수정하는게 보이면 그것은 n log n의 시간을 쓴다고 쉽게 파악 할 수 있다.

이렇게 생각하면 알고리즘의 답을 반은 알고가는 셈인것이다.

------------------
다음은 내가 찾은 time complexity 예시들이다.

이 포스트에 있는것들만 외워도 반은 간다.

- O(n!) [Factorial time]: Permutations of 1 ... n
- O(2n) [Exponential time]: Exhaust all subsets of an array of size n
- O(n3) [Cubic time]: Exhaust all triangles with side length less than n
- O(n2) [Quadratic time]: Slow comparison-based sorting (eg. Bubble Sort, Insertion Sort, Selection Sort)
- O(n log n) [Linearithmic time]: Fast comparison-based sorting (eg. Merge Sort)
- O(n) [Linear time]: Linear Search (Finding maximum/minimum element in a 1D array), Counting Sort
- O(log n) [Logarithmic time]: Binary Search, finding GCD (Greatest Common Divisor) using Euclidean Algorithm
- O(1) [Constant time]: Calculation (eg. Solving linear equations in one unknown)
