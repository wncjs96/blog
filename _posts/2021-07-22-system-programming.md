---
layout: post
title: System Programming
date: 2021-07-22 03:06:00
---

오늘은 문득 멍때리다가 떠오른김에 시스템 프로그래밍의 기초부터 차근차근 정리를 해보는 시간을 갖도록 하겠다.

### 시스템 프로그래밍?

우리가 컴퓨터를 통해 유저 애플리케이션을 실행하는 구조는 크게 세 가지로 나눌 수 있다.

- 하드웨어
- Kernal Space(Operating Systems)
- User Space(User Applications)

혹자는 소프트웨어 프로그래밍을 지금 시대의 마법이라고 생각한다.  
여러가지 기능들을 abstraction을 통해 합쳐 새로운 기능을 만들어내는 그 부분은 정말 마법같이 느껴질 때가 있기도 하다.  
최근에 떠오른 VR 해드셋같은 또 하나의 peripheral device도 다른 유저 애플리케이션과 합쳐져서 정말 색다른 경험을 느끼게 해준다.  
하지만 이는 프로그래밍 하나로 만들어진 마법이 아니다.  
"VR 해드셋"이라는 하드웨어를 "Operating System"이 드라이버 매니저/컨트롤러의 기능을 이용해서, "User application"에서 쓸 수 있게 해줬기에 가능한 것이다.  
잘 모르고 봤을 때 마법같이 보이던 프로그램들은 전부 사실 당연하지만 하드웨어의 기능에 의지하고 있는 것이다.  

이를 embedded system이라고 한다 (다른 IoT, 항공기 기계 등등에서도 사용되는 그 embedded system 맞음).

OS가 보는 하드웨어들 중 우리가 막연히 알고있는 것들은 다 제각기 다른 구조, architecture을 가지고 있다.  

우리가 컴퓨터 조립을 하거나 할 때 CPU의 타입같은걸 따지는 이유가 그 이유이다.  

OS는 이 제각기 다른 architecture를 가지고 있는 CPU의 명령어들을 이용, 시스템 기능들을 구현하고, 컴퓨터 유저가 쓸 수 있게 제공한다.  
그래야 컴퓨터 유저가 그 기능들을 이용해서 hardware의 기능을 쓸 수 있기 때문이다.  

즉, I/O device management 외에도 다양한 일을 한다.
- File system
- Inter-process communication
- Process Management (scheduling)

위와 같은 kernel이 하는 일을 포함, system call interface (syscalls)를 제공하고, libraries 또한 제공한다.

이렇게 우리 유저들은 소프트웨어를 이용할 시에 어찌보면 API와 비슷한 느낌의 시스템 콜 인터페이스를 이용, 함수를 call함으로써 OS가 제공하는 기능들을 사용하는것이다. 

시스템 프로그래밍은 위의 OS가 제공하는 기능을 잘 이용해서 User application을 짜는것을 뜻한다.  
OS 자체를 만드는것이 아니다!!

