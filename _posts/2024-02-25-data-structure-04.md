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
last_modified_at: 2024-03-10
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

#### 3. 배열로 구현한 스택

배열로 구현한 스택은 앞서 공부한 배열 리스트에서와 마찬가지로 물리적으로 연결된 C의 배열을 이용하기 때문에 스택을 생성할 때 스택의 크기를 반드시 지정해 주어야 한다는 특성이 있다.  

![image](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/ce0b0e79-9c45-4219-ac37-347aca45747f)  

배열로 구현한 스택은 스택의 크기(최대 노드 개수를 내부 변수 maxElementCount)를 저장하고 있다. 이러한 스택의 크기는 곧 스택이 가진 배열의 크기를 의미한다.  

스택에 저장된 현재 노드 개수를 내부 변수 currentElementCount를 이용해 스택에서의 Top의 위치를 알 수 있다.(currentElementCount - 1)  

##### 3-1. 스택의 생성

메모리를 할당하고 초기화 해주는 함수  

```c
if (size > 0) {
  pReturn = (ArrayStack *)malloc(sizeof(ArrayStack))  //메모리 할당
  if(pReturn != NULL) {                               //메모리 할당 점검
    memset(pReturn, 0, sizeof(ArrayStack))            //메모리 초기화
    pReturn -> maxElementCount = size;
  }
  ...
  //실제 배열 생성
  pReturn -> pElement = (ArrayStackNode *)malloc(sizeof(ArrayStackNode));
  if(pReturn -> pElement != NULL) {
    memset(pReturn -> pElement, 0, sizeof(ArrayStackNode)*size);
  }
}
```

배열 리스트를 생성하는 함수 createArrayStack()은   
1. 배열의 크기 size가 0보다 큰지 점검하고  
2. 구조체 ArrayStack에 대한 메모리를 할당한다.  
3. 최대 원소 개수, 현재 원소 개수 및 배열 포인터를 초기화 시킨다.  
4. 원소를 저장하는 배열에 대한 메모리를 할당하고 원소 데이터를 초기화 시킨다.  


> memset() 연산
```c
#include <string.h>
```
라이브러리 헤더 파일 string.h를 포함해야 한다.  
input 파라미터 s가 가리키는 메모리 영역의 처음 n 바이트를 상수 c로 채우는 기능을 수행한다.  
```c
void *memset(void *s, int c, size_t n);
```
보통 메모리를 초기화 할 때 대부분 그 값을 0으로 초기화 하는데 위 구문에서는 memset()로 노드 배열(pReturn -> pElement)을 초기화했다.

```c
sizeof(ArrayStackNode) * maxElementCount
```
이러한 방식은 앞으로 ArrayStackNode의 구조가 변경되어도 위의 초기화 루틴은 변경이 필요 없다는 장점이 있다.  
구조체 소스 변화에 독립된 구현을 위해 memset()이 사용되었으며, 유지/보수 비용 감소 효과가 있다.  
  
##### 3-2. Push 연산

![image](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/e9326a2b-5702-4179-b235-6f716897ca1a)


[//]: # (#### 4. 연결 리스트로 구현한 스택)

[//]: # (#### 5. 스택 응용 - 1)

[//]: # (#### 6. 스택 응용 - 2)

[//]: # (#### 7. 스택 응용 - 3)