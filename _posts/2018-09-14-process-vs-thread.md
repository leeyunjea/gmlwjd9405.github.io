---
layout: post
title: '[OS] 프로세스와 스레드의 차이'
subtitle: '프로세스와 스레드의 개념과 차이를 이해할 수 있다.'
date: 2018-09-14
author: heejeong Kwon
cover: '/images/os-process-and-thread/process-and-thread-main.png'
tags: os process thread
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - 프로세스와 스레드의 개념을 설명할 수 있다.
> - 프로세스와 스레드의 차이(Process vs Thread)를 이해할 수 있다.
> - 멀티 프로세스 대신 멀티 스레드를 사용하는 이유를 알아본다.

## 프로세스와 스레드의 차이(Process vs Thread)
### 프로그램(Program) 이란
* 사전적 의미
  * <span style="background-color: #e1e1e1">"어떤 작업을 위해 실행할 수 있는 파일"</span>

### 프로세스(Process) 란
* 사전적 의미
  * <span style="background-color: #e1e1e1">"컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램"</span>
  * 메모리에 올라와 **실행되고 있는 프로그램의 인스턴스(독립적인 개체)**
  * 운영체제로부터 시스템 자원을 할당받는 작업의 단위
  * 즉, 동적인 개념으로는 실행된 프로그램을 의미한다.
* <mark>참고</mark>  할당받는 시스템 자원의 예
  * CPU 시간
  * 운영되기 위해 필요한 주소 공간
  * Code, Data, Stack, Heap의 구조로 되어 있는 독립된 메모리 영역
* 특징
  * ![](/images/os-process-and-thread/process.png){: width="450" height="250"}
  <!-- * <img src="./images/os-process-and-thread/process.png" width="70%" height="70%"> -->
  * 프로세스는 각각 독립된 메모리 영역(Code, Data, Stack, Heap의 구조)을 할당받는다.
  * 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가지고 있다.
  * 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.
  * 한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간의 통신(IPC, inter-process communication)을 사용해야 한다. (Ex. 파이프, 파일, 소켓 등을 이용한 통신 방법 이용)

### 스레드(Thread) 란
* 사전적 의미
  * <span style="background-color: #e1e1e1">"프로세스 내에서 실행되는 여러 흐름의 단위"</span>
  * **프로세스의 특정한 수행 경로**
  * 프로세스가 할당받은 자원을 이용하는 실행의 단위
* 특징
  * ![](/images/os-process-and-thread/thread.png){: width="450" height="250"}
  * 스레드는 프로세스 내에서 각각 Stack만 따로 할당받고 Code, Data, Heap 영역은 공유한다.
  * 스레드는 한 프로세스 내에서 동작되는 여러 실행의 흐름으로, 프로세스 내의 주소 공간이나 자원들(힙 공간 등)을 같은 프로세스 내에 스레드끼리 공유하면서 실행된다.
  * 같은 프로세스 안에 있는 여러 스레드들은 같은 힙 공간을 공유한다. 반면에 프로세스는 다른 프로세스의 메모리에 직접 접근할 수 없다.
  * 각각의 스레드는 별도의 레지스터와 스택을 갖고 있지만, 힙 메모리는 서로 읽고 쓸 수 있다.
  * 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있다.


## 멀티 프로세스와 멀티 스레드의 차이
### 멀티 프로세스
* 장점
  * 여러 개의 자식 프로세스 중 하나에 문제가 발생하면 그 자식 프로세스만 죽는 것 이상으로 다른 영향이 확산되지 않는다.
* 단점
  * Context Switching에서의 오버헤드
    <!-- * ????? -->
    * CPU에서 여러 프로세스를 돌아가면서 작업을 처리하는 데 이 과정을 Context Switching라 한다.
    * 프로세스는 각각 독립된 메모리 영역을 할당받았기 때문에 **Context Switching** 과정에서 캐쉬 메모리 초기화 등 무거운 작업이 진행되고, 오버헤드가 발생하게 된다.
  * **Context Switching란?**: 동작 중인 프로세스가 대기를 하면서 해당 프로세스의 상태(Context)를 보관하고, 대기하고 있던 다음 순서의 프로세스가 동작하면서 이전에 보관했던 프로세스의 상태를 복구하게 된다.
  <!-- * **Context Switching** 는 프로세스가 가지고 있는 스레드를 처리하는 과정이다. -->

### 멀티 스레드
* 장점
  * 시스템 자원 소모 감소 (자원의 효율성 증대)
  * 시스템 처리량 증가 (처리 비용 감소)
  * 간단한 통신 방법으로 인한 프로그램 응답 시간 단축
* 단점
  * 주의 깊은 설계가 필요하다.
  * 디버깅이 까다롭다.
  * 단일 프로세스 시스템의 경우 효과를 기대하기 어렵다.
  * 다른 프로세스에서 스레드를 제어할 수 없다. (즉, 프로세스 밖에서 스레드 각각을 제어할 수 없다.)
  * 멀티 스레드의 경우 자원 공유의 문제가 발생한다. (동기화 문제)
  * 하나의 스레드에 문제가 발생하면 전체 프로세스가 영향을 받는다.


### 멀티 프로세스 대신 멀티 스레드를 사용하는 이유?
여러 프로세스(멀티 프로세스)로 할 수 있는 작업들을 하나의 프로세스에서 여러 스레드로 나눠가면서 하는 이유?
* 쉽게 설명하면, 프로그램을 여러 개 키는 것보다 하나의 프로그램 안에서 여러 작업을 해결하는 것이다.

* ![](/images/os-process-and-thread/multi-thread.png){: width="600" height="170"}

1. 자원의 효율성 증대
* 멀티 프로세스로 실행되는 작업을 멀티 스레드로 실행할 경우, **프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어들어** 자원을 효율적으로 관리할 수 있다.
  * -> 프로세스 간의 Context Switching시 단순히 CPU 레지스터 교체 뿐만 아니라 RAM과 CPU 사이의 캐쉬 메모리에 대한 데이터까지 초기화되므로 오버헤드가 크기 때문
* 스레드는 프로세스 내의 메모리를 공유하기 때문에 독립적인 프로세스와 달리 스레드 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어들게 된다.
2. 처리 비용 감소 및 응답 시간 단축
* 또한 프로세스 간의 통신(IPC)보다 스레드 간의 통신의 비용이 적으므로 작업들 간의 통신의 부담이 줄어든다.
  * -> 스레드는 Stack 영역을 제외한 모든 메모리를 공유하기 때문
* 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 빠르다.
  * -> Context Switching시 스레드는 Stack 영역만 처리하기 때문

* <mark>주의할 점!</mark>
  * **동기화 문제**
  * 스레드 간의 자원 공유는 전역 변수(데이터 세그먼트)를 이용하므로 함께 상용할 때 충돌이 발생할 수 있다.



<!-- # 관련된 Post
* Eclipse에서 Spring MVC 프로젝트 생성하기에 대해 알고 싶으시면 [Eclipse에서 Spring MVC 프로젝트 생성하기](https://gmlwjd9405.github.io/2018/05/07/spring-project-eclipse-setting.html)를 참고하시기 바랍니다. -->



# References
> - [https://brunch.co.kr/@kd4/3](https://brunch.co.kr/@kd4/3)
> - [https://magi82.github.io/process-thread/](https://magi82.github.io/process-thread/)
> - [https://jaybdev.net/2017/06/05/Java-3/](https://jaybdev.net/2017/06/05/Java-3/)
> - [http://includestdio.tistory.com/6](http://includestdio.tistory.com/6)
> - [https://lalwr.blogspot.com/2016/02/process-thread.html](https://lalwr.blogspot.com/2016/02/process-thread.html)
> - [http://you9010.tistory.com/136](http://you9010.tistory.com/136)