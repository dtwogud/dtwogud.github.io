---
title : "[Reading] 열혈강의 자료구조 - CHAPTER 03"
excerpt: "C로 만드는 자료구조와 적용 알고리즘 해설서"

categories:
  - reading
tags:
  - [C, Data Structure, Algorithm, Reading]

toc: true
toc_sticky: true

date: 2024-01-11
last_modified_at: 2024-01-11
---
<br>

## 열혈강의 자료구조
<a href="https://freelec.co.kr/lecture/%EC%97%B4%ED%98%88%EA%B0%95%EC%9D%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/" target="_blank">
  <img src="https://image.aladin.co.kr/product/616/52/cover500/8989345022_1.jpg">
</a>

> 자료구조를 본격적으로 배우는 본론의 시작 '리스트'

### CHAPTER 03 - 리스트

구조가 단순해 가장 널리 사용되는 기초적인 자료구조 중 하나로 스택(Stack), 큐(Queue), 트리(Tree)와 그래프(Graph)등 다른 여러 자료구조의 구현에도 응용되어 사용된다.
<br/>
자료를 순서대로 저장하는 자료구조를 말하며 여러 개의 자료가 일직선으로 서로 연결된(Sequential) '선형 구조'를 의미한다.
<br/>
- 배열 리스트: C언어에서 제공하는 배열을 이용해 구현된 리스트, 자료가 일직선으로 "연결"
<br/>
- 배열: 같은 자료형의 데이터가 메모리 상 연속적으로 저장
  <img width="587" alt="스크린샷 2024-01-14 오후 6 10 14" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/df05daef-d408-413a-8419-23004686fe0c"

#### 1. 리스트의 개념


1. 문자 리스트 / 문자열 리스트

2. 행렬 (Matrix)

3. 다항식 (Polynomial Equation)
   <img width="423" alt="스크린샷 2024-01-14 오후 6 22 40" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/051f1d66-197f-482a-85c8-02ed2c76727f">

#### 2. 리스트 추상 자료형
리스트 사용에 필요한 기본적인 연산을 정리한 리스트의 추상 자료형으로 리스트를 실제 사용하려면 기본적으로 다음 표에 있는 기능들이 필요하다.
(N/A는 해당 값이 존재하지 않거나 필요하지 않다는 의미)

<img width="709" alt="스크린샷 2024-01-14 오후 6 18 32" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/2bf539ab-33d3-4976-ba62-fbdc06e97cbe">

- 리스트를 사용하기 위해 리스트 생성 및 생성된 리스트를 삭제할 수 있어야 한다.
- 연산에서 입력 파라미터 n은 리스트에 저장 가능한 최대 원소 개수 의미
- 리스트에 원소 추가 및 삭제 기능(추가 시 생성 위치 p 지정)

#### 3. 배열 리스트
배열은 메모리상에 순차적으로 저장된다. 즉, 배열을 구성하는 원소들이 순서대로 연속해 메모리에 저장된다.
따라서 배열 리스트에서의 논리적 순서와 물리적 순서가 같다는 특성이 있다.

1. 배열 리스트의 원소 추가

   <img width="197" alt="스크린샷 2024-01-14 오후 6 35 24" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/874e6e5c-7077-491d-80c8-3f0c9b7d2e97">

배열 리스트 중간에 새로운 원소를 추가하려면 새로운 원소가 중간에 삽입될 수 있도록 기존 원소들을 이동시켜 주어야 한다.

2. 배열 리스트의 원소 삭제

   <img width="183" alt="스크린샷 2024-01-14 오후 6 41 42" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/1af22f76-4b4a-498b-861e-d80fda7bcae4">

중간에 있던 원소를 제거하려면 중간에 발생하는 공백을 메우기 위해 원소의 이동이 필요하다.

"배열 리스트의 경우 중간에 원소가 추가, 제거 됨에 따라 관계없는 원소들의 이동이 필요하다는 단점이 존재한다."
ex) 10만개의 배열 리스트에서 0번째 인덱스에 원소의 추가가 필요하다면 100000 - 1 번의 이동이 필요..

#### 4. 연결 리스트의 개념

연결 리스트는 리스트를 포인터를 이용해 구현한다는 점에서 배열 리스트와 차이가 있다. 
즉, 일련의 자료를 저장함에 있어서 C의 포인터를 이용해 물리적으로 떨어져 있는 자료를 순서대로 연결시켜 놓았다. (배열 리스트 - 물리적 위치, 연결 리스트 - 논리적 위치 --> 배열 리스트(물리적 위치)는 최대 원소 개수를 지정해줘야 함)

  <img width="596" alt="스크린샷 2024-01-20 오후 8 15 23" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/d0cced1b-8ec6-4555-ab72-0c791ac1bfab">

연결 리스트는 자료와 링크로 구성된 노드들로 구성된다. 앞서 배열 리스트에서도 이러한 노드가 사용되었지만, 자료만 저장한다는 점에서 '원소 === 노드' 인 반면,
연결 리스트에서의 노드는 '자료 + 링크'를 의미하기에 원소를 포함하는 개념이다(연결 리스트 모든 연산의 기본 단위).

연결 리스트의 노드 추가, 제거는 링크 정보를 추가, 제거 한다는 의미와 동일하다.
배열 리스트의 노드 추가, 제거는 원소의 이동을 의미한다.

  <img width="272" alt="스크린샷 2024-01-20 오후 8 18 36" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/14f981fa-d2bb-456c-94cc-f1be4e479b51">

##### 4-1. 연결 리스트의 노드 추가 또는 제거

ㅁ 새로운 노드 추가 / 제거 전

  <img width="323" alt="스크린샷 2024-01-20 오후 8 26 17" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/4792a078-76e3-4ec5-be06-38ab65848708">
  <img width="293" alt="스크린샷 2024-01-20 오후 8 27 51" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/6fd9e872-61f2-4afe-8188-c0bb173d5000">

ㅁ 기존 링크 제거 / 새로운 링크 추가

  <img width="318" alt="스크린샷 2024-01-20 오후 8 26 33" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/88802212-c02b-41bc-8e55-dcfcf9830ac3">
  <img width="296" alt="스크린샷 2024-01-20 오후 8 28 17" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/bea23309-db58-4f05-a8db-ffb6d097ecf7">

ㅁ 새로운 노드 추가 / 제거 후

  <img width="324" alt="스크린샷 2024-01-20 오후 8 26 41" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/3b165c65-f277-4ab2-bd70-3e73066439d2">
  <img width="292" alt="스크린샷 2024-01-20 오후 8 28 24" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/ddc92691-776c-44aa-ac35-c318daa28cae">

##### 4-2. 연결 리스트 vs 배열 리스트

1. 추가 원소 이동 연산 불필요

원소가 중간에 삽입 또는 제거되는 경우 모든 원소를 한 칸씩 이동시켜 주는 추가 이동이 필요하다. 
단순히 연결된 노드 사이의 포인터만 변경시켜주면 되기에 추가/삭제가 빈번히 발생하는 경우 바람직하다.

2. 최대 원소 개수 지정 불필요

연속적인 메모리 공간이 필요하므로 리스트 생성 시 반드시 최대 원소 개수를 지정해 주어야 하는 배열 리스트.
예상보다 많은 원소가 추가될 경우 외부 단편화, 또한 저장되는 원소의 개수가 예상보다 적을 경우 내부 단편화가 발생한다.

1-a. 구현의 어려움

동적인 메모리 할당 및 포인터 연산 등으로 인해 배열 리스트보다 구현의 비용이 높다.
또한 메모리 관리와 관련해 메모리 누수 오류의 발생 가능성이 높다.

2-a. 높은 비용의 탐색 연산

배열 리스트의 경우 random access가 가능하지만 연결 리스트는 불가능하다. 원하는 원소를 찾을 때까지 포인터로 노드를 탐색해야 하기 때문에 시간이 오래 걸린다.

배열 리스트의 random access - O(1) / 원소 삽입 / 삭제 - O(n)
연결 리스트의 random access - O(n) / 배열보다 빨라질 수 있는 노드 삽입 / 삭제

##### 4-3. 연결 리스트의 종

1. 단순 연결 리스트(Singly Linked)

  <img width="217" alt="스크린샷 2024-01-20 오후 8 54 46" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/96da0b7e-0f57-477c-846b-891d7750151a">

2. 원형 연결 리스트(Circular Linked)

  <img width="228" alt="스크린샷 2024-01-20 오후 8 54 54" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/7b0c1d36-f8f1-4282-a415-2ecd7821a414">

3. 이중 연결 리스트(Doubly Linked)

    <img width="223" alt="스크린샷 2024-01-20 오후 8 55 00" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/5f76393a-db82-4f69-aef9-8b6cc680522a">

세 가지 연결 리스트의 특성을 비교할 때 이전(Previous)노드 접근 연산이 주로 사용된다.

#### 5. 단순 연결 리스트

#### 6. 원형 연결 리스트




[//]: # (#### 7. 이중 연결 리스트)

[//]: # (#### 8. 연결 리스트의 응용)

