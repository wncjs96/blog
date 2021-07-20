---
layout: post
title: Notion of Password Hashing
date: 2021-07-20 04:47:00
---

패스워드를 웹사이트에 넣어 회원가입을 한다는 시나리오에서 어떻게 안전하게 패스워드를 저장할 수 있을까?  

### 클라이언트 사이드

만약 패스워드를 웹 브라우저 한 부분에 넣어 서버에 보낸다고 했을때, 해커가 MITM 공격으로 가로챈다고 해도, 이미 HTTP request는 SSL/TLS로 인해 암호화가 완료된 상태이다.  
이 부분은 우리가 원리적으로 더 보안을 강하게 하고 싶어도 딱히 할 방법이 없다.  

클라이언트 사이드에서 자바스크립트같은 언어로 암호화를 진행한다 해도, 그 symmetric public key encryption과 같은 복잡한 암호체계가 아니고 단순히 암호화를 시키는 거라면, 클라이언트 사이드의 코드를 가지고 있는 유저쪽에서 쉽게 풀 수 있기 때문이다.

즉, 클라이언트 쪽의 패스워드, 그리고 그 패스워드의 암호화는 굳이 신경쓰지 않는다.

### 서버 사이드

반대로, 만약 해커가 서버 쪽의 컴퓨터를 해킹했다고 쳤을 때, 어떻게 유저들의 암호들을 알 수 없게 하느냐가 매우 중요하다.

여기서 요점은 암호 자체를 해커들이 알 수 있느냐 없느냐다.  
다시 말하자면, 해커가 이미 서버사이드를 장악했다고 했을 시에, 유저의 아이디를 통해 우리의 서비스를 이용하는 것은 막을 수 없다.  
우리가 암호화를 통해 달성할 수 있는 것은 유저의 암호자체를 해커가 알 수 없게 하여 그 암호를 이용해서 다른 서비스라던가 다른 해킹으로 이어지는 걸 막는 것 뿐이다.  

그러면 우리에게는 유저에게서 받아온 패스워드를 토대로 다음 시간에 로그인을 한다고 쳤을 때, 그 두 패스워드가 동일한지 알아볼 수는 있지만, 서버 쪽에서 원래 암호는 알 수 없는.. 그런 구조가 필요하다.

그 one way encryption을 hashing 이라고 한다.

hashing을 가장 쉽게 이해하는 방법은 modular과 같은 기존의 값의 어느 부분을 아예 제거해버리는 수학적 함수이다.

아무 숫자에 %3을 해준다 했을 시에 예를 들어 10%3 이나 1%3 같은 1이라는 결과를 반환하므로 우리는 그 아무 숫자가 10인지 1인지 13인지 알 방도가 없다.

그렇기에 modular 자체도 hash method 라고 볼 수 있다.

어쨋든, 패스워드를 hashing의 함수의 넣어주어 서버 쪽에서는 알아볼 수 없는 형태로 비밀번호를 저장하면 안전하다는 것이다. 그리고 그것이 바로 패스워드 저장의 제일 기초 개념이다.

-----------------------

### hash 자체의 문제점

hash function 하나로는 꽤 문제점이 많다.

- Same password, different users

우연찮게 유저 A와 유저 B가 같은 패스워드를 쓴다고 치자. 그럼 그 둘의 패스워드를 hash한 값도 같아져 버린다.  
사실 이 정보는 중요하게 느껴지지 않을지 몰라도 엄청나게 큰 것이다.  
그 정보만으로도 조합해야할 패스워드의 수가 적어지기 때문이다.  
이를 어쩌나.. 

- Dictionary attack

해커들은 유저들이 할 만한 패스워드를 엄청나게 많이 적어두어 하나하나 매우 빠른 속도로 hash시켜서, 저장되어있는 값과 비교할 수 있다.  

그렇다면, 같은 값이 나오면 바로 패스워드가 들켜버리는데, 그럼 이를 어찌할까?

- I don't guess password, I guess hash codes(Brute Force)

해커들이 패스워드를 예상하고, 그 패스워드를 hash해서 값을 비교하는 방법도 있겠지만, 반대로 서버 쪽 장악은 포기했다 치고 그냥 아무 웹사이트나 가서 유저 아이디 하나 정하고 배 벅벅 긁으면서 가능한 모든 경우의 수를 대입해보는 경우도 있다.  

참고로 해시라는 method는 아무 길이의 패스워드를 넣는다 해도 정해진 길이의 값만 반환한다.. 그럼 이를 어쩔까?

- GPU processing speed

최근 GPU들이 cryptocurrency mining등을 위해 성능이 좋아지면서 엄청난 속도로 복호화를 진행 할 수 있다.
만약 유저가 패스워드를 평범한 길이로 평범한 단어를 넣어서 조합했다면 이는 생각보다 빠르게 해킹당한다.
하.. 이를 어쩌나?

-----------------

### 해결 방안

첫번째 문제점을 고치는 법은 주어진 패스워드가 유저마다 다르게 암호화되어야 한다는 것이다.  
이는 생각보다 쉽게 해결할 수 있는데, 바로 랜덤한 값들을 패스워드 앞이나 뒤에 붙여주는 것이다.  
이를 "salt"라 한다.  
그 앞이나 뒤에 붙여주는 salt 값은 같은 유저테이블에 넣어둔다 해도 상관없다.  

이미 해커가 서버사이드를 장악했기 때문에 어딘가에 숨기는가는 중요하지않다. 그저 해커가 봤을 때 두 유저가 똑같은 해시를 가지고 있지만 않으면 된다.  

같은 논리로 두번째 문제점도 salt를 붙여줌으로서 dictionary attack 자체에 hash하는 값이 다르게 나올 확률을 높여 줄 수가 있다.

그리고 salt를 여러개 저장하고 hash를 여러번 적용하는것도 하나의 방법이다.

세번째 문제점은 매우 간단하다. Hash가 긴 값을 반환하는 것을 사용하면 된다.

예를 들어, 지금도 쓰이고 있는 SHA 해시를 봤을때, 
SHA1이라는 해시 함수는 128비트를 반환한다.  
그리고 SHA512라는 해시 함수는 512비트를 반환한다.  

비트당 2의 지수만큼 경우의 수가 증가하므로, 패스워드같이 중요한 정보는 적당히 긴 걸 쓰면 좋다. 

네번째 문제점은 세번째 문제점에도 해당되는 내용이지만 시대(computational power)에 맞춰 우리가 계속 생각해보며 바꿔나가야 할 부분이다.  

이 부분을 해결하는 원리적인 개념은 Hash 함수의 길이는 늘리는 것도 중요하지만, hash 자체를 생성하는 시간을 늦추면 되는 것이다.  

만약 슈퍼컴퓨터를 사용해 엄청난 수의 경우의 수를 빠르게 비교 할 수 있는 능력이 있다고 해도 한번한번의 경우를 위해 자원을 만드는 시간이 오래걸린다면, 전체 수행의 시간이 늘어지기에, 이 속도를 늦추는 느린 hash를 이용하는 것이 trade off로서 굉장이 좋다는 것이다.  

유저가 회원가입할때, 로그인할때 0.1초의 시간을 더 들여 한번의 hash를 더 해야 하는 반면, 해커는 엄청난 수를 0.1초를 더 들여야 하는 것이다.

-------------------

자, 이로서 비밀번호 해시의 개념, 정리가 끝났다.

세줄 요약하겠다.

- hash(password + salt), apply more hash methods if wanted
- use slower hash methods like PBKDF2withHmacs for strong GPU
- our premise is when the server is already compromised, so we don't care whether the hacker has access to the user authentication or not, we care about the actual password being exposed to the hackers.