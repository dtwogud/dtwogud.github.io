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

[//]: # (#### 4. 연결 리스트의 개념)

[//]: # (#### 5. 단순 연결 리스트)

[//]: # (#### 6. 원형 연결 리스트)

[//]: # (#### 7. 이중 연결 리스트)

[//]: # (#### 8. 연결 리스트의 응용)

