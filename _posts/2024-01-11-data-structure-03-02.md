---
title : "[Reading] 열혈강의 자료구조 - CHAPTER 03 - 02"
excerpt: "C로 만드는 자료구조와 적용 알고리즘 해설서"

categories:
  - reading
tags:
  - [C, Data Structure, Algorithm, Reading]

toc: true
toc_sticky: true

date: 2024-01-30
last_modified_at: 2024-01-30
---
<br>

## 열혈강의 자료구조
<a href="https://freelec.co.kr/lecture/%EC%97%B4%ED%98%88%EA%B0%95%EC%9D%98-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0/" target="_blank">
  <img src="https://image.aladin.co.kr/product/616/52/cover500/8989345022_1.jpg">
</a>

> 연결 리스트의 종류

### CHAPTER 03 - 리스트

#### 5. 단순 연결 리스트

구조체

<img width="371" alt="스크린샷 2024-01-30 오후 11 17 31" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/11836e85-0071-4f21-ab62-afcba8ba25ea">

- 현재 노드 개수 저장
- 헤더 노드: 첫 번째 리스트 노드에 대한 포인를 저장하는 노(노드의 추가/삭제 구현의 간편성을 위해 사용되는 일종의 더미 노드)
- 헤드 포인트: 연결 리스트의 첫 번째 노드를 가리키는 포인터

헤더 노드를 통해 연결 리스트에 저장된 다른 노드들이 접근할 수 있다.
<br/>
리스트 노드의 element는 실제 데이터를 저장하며, pLink는 다음 노드로의 링크를 저장한다.

#### 6. 원형 연결 리스트

#### 7. 이중 연결 리스트

#### 8. 연결 리스트의 응용

