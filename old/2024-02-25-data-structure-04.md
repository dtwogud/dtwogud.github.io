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



[//]: # (#### 2. 스택 추상 자료형)

[//]: # (#### 3. 배열로 구현한 스택)

[//]: # (#### 4. 연결 리스트로 구현한 스택)

[//]: # (#### 5. 스택 응용 - 1)

[//]: # (#### 6. 스택 응용 - 2)

[//]: # (#### 7. 스택 응용 - 3)