---
title : "[Reading] 열혈강의 자료구조 - CHAPTER 04"
excerpt: "C로 만드는 자료구조와 적용 알고리즘 해설서"

categories:
  - reading
tags:
  - [C, Data Structure, Algorithm, Reading]

toc: true
toc_sticky: true

date: 2024-02-25
last_modified_at: 2024-02-25
---
<br>

## 열혈강의 자료구조
<a href="https://freelec.co.kr/lecture/%EC%97%B4%ED%98%88%EA%B0%95%EC%9D%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/" target="_blank">
  <img src="https://image.aladin.co.kr/product/616/52/cover500/8989345022_1.jpg">
</a>

> 스택

### CHAPTER 03 - 스택

스택이라는 선형 자료구조를 살펴보자.  
스택의 고유한 특성을 이야기할 때 가장 대표되는 특성으로 LIFO(Last-In-First-Out)를 들 수 있다.  
사장 나중에 들어간 자료가 가장 먼저 나온다는 후입선출의 개념으로 선형 자료주로를 가지기에 저장된 자료들 사이의 선후관계가 모두 1:1이라는 뜻이다.  
(즉 한 물건 위에는 오직 하나의 물건만 쌓을 수 있다.)  

<br/>
선입후출, FILO(First-In-Last-Out)의 특성과도 동일한 의미의 제약사항을 가지기에 스택에서의 자료의 추가 혹은 반환은 스택의 끝에서만 가능하다. 
스택의 끝이란 스택의 제일 위(Top), 즉 가장 최근에 추가된 자료가 있는 곳을 말한다.


#### 1. 스택의 개념

스택은 자료를 한 방향으로만 쌓는 자료 구조이다. 앞서 공부했던 리스트는 처음과 중간 제일 마지막 위치에 자료를 삽입할 수 있는 반면 스택에서는 오직 스택의 제일 위에서만 추가할 수 있다.  
새로운 자료를 스택에 추가하는 것을 푸시(Push)라고 하며, 스택에서 자료를 꺼내는 것을 팝(Pop)이라고 한다.  

<img width="525" alt="스크린샷 2024-02-25 오후 5 45 42" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/05e58c1d-d4fc-408b-85f8-a255543a3898">

##### 1-1. 푸시(Push) 연산

<img width="293" alt="스크린샷 2024-03-04 오전 6 17 29" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/ff3d9da2-53e3-42a4-891d-bcebdaed1269">  

- Overflow 현상 : 스택의 크기를 초과하여 새로운 자료를 추가할 때 자료가 추가될 수 없는 현상.  

스택에 새로운 자료를 추가하는 연산을 말하며 스택의 제일 위에서만 발생한다.  

가장 최근에 스택에 삽입된 자료를 스택의 탑(top)이라고 한다.  

##### 1-2. 팝(Pop) 연산

<img width="295" alt="스크린샷 2024-03-04 오전 6 20 19" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/7dc02d0b-ef6b-4bb3-adfb-d32dc47b8f67">  

- Underflow 현상 : 공백 상태의 스택에 팝 연산이 호출되는 현상  

스택에 가장 최근에 삽입된 자료를 반환하는 팝 연산은 스택에서 해당 자료를 먼저 제거하고, 제거한 자료를 전달하는 점에 주의 하자.  

##### 1-3. 피크(Peek) 연산

<img width="314" alt="스크린샷 2024-03-04 오전 6 23 01" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/a5b2a68a-e7e4-4adf-9944-ef86885ee924">  

"반환"이 중점인 연산으로 팝 연산과 비슷한 연산이지만 스택의 최상위 자료를 반환할 뿐, 기존 스택에서 자료를 제거하지 않는다.  

#### 2. 스택 추상 자료형

스택을 실제 사용하려면 필요한 기본적인 기능들을 알아보자.

- 스택의 크기: n (포인터(연결 리스트)를 사용한다면 필요 없다.)
- Push: 원소 추가 가능 여부 판단
- Pop: 공백 스택인지 여부 판단
- Peek
- 스택 삭제: 메모리 해제

[//]: # (#### 3. 배열로 구현한 스택)

[//]: # (#### 4. 연결 리스트로 구현한 스택)

[//]: # (#### 5. 스택 응용 - 1)

[//]: # (#### 6. 스택 응용 - 2)

[//]: # (#### 7. 스택 응용 - 3)