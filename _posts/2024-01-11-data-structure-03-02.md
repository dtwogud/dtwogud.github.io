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

##### 5-1. 노드 추가

![스크린샷 2024-01-31 오후 2 38 24](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/e3b636ad-a119-4eed-bdb5-d7fa7fe17fcc)

0 <= position <= currentNode (position의 범위는 전체 NodeElement의 개수보다 -1이 되어야 하는것 아닌가?)
<br/>
  => 맨 마지막에 추가될 경우(append)를 고려하여 개수의 범위는 전체 노드의 개수 -1을 하지 않는다.

![스크린샷 2024-01-31 오후 2 43 57](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/ba1b4d8b-c51b-411f-9562-397f69e70be2)

새로 추가된 노드를 가리키는 포인터 변수 pNewNode.

```c
pNewNode->pLink = pPreNode->pLink
pPreNode->pLink = pNewNode
```

pNewNode->pLink(추가된 노드의 다음 노드)에 pPreNode->pLink(추가하려는 위치의 노드)를 대입한다.
<br/>
또한 포인터 변수 pPreNode는 추가하려는 위치 position에 있는 노드의 이전 노드를 가지고 있다. 따라서 포인터 변수 pPreNode->pLink(pPreNode의 다음 연결 노드)에 pNewNode(새로 추가된 노드)를 재설정한다.
<br/>
노드 정보 재설정까지 끝난 후 연결 리스트의 노드 개수(currentElementCount)를 1 증가시키고, 노드의 추가가 성공했는지를 함수 호출의 결과로 반환한다.

##### 5-2. 노드 제거

![스크린샷 2024-01-31 오후 3 13 30](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/f8139996-ccbb-42b7-b45f-c71c84dc1634)

앞서 노드 추가와 마찬가지로 먼저 제거하려는 노드의 위치 position이 유요한 범위의 값인지를 점검하고( !== null) 해당 위치로 이동해 노드를 제거, 링크 정보를 재설정한다.

0 <= position < nodeCount
<br/>
삭제하려는 위치의 이전 노드 위치인 (position-1)까지 이동한다.
headerNode를 시작으로 (position-1)번 만큼 다음 노드로 이동하며 삭제하려는 위치의 이전 노드까지 이동이 가능하다.
삭제하려는 노드의 이전 노드는 pNode가 되고, 삭제하려는 노드는 pDelNode가 된다.

#### 6. 원형 연결 리스트

![스크린샷 2024-01-31 오후 3 44 10](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/bd290937-1dd3-4540-bc82-09997cc874d4)

리스트의 마지막 노드가 리스트의 첫 번째 노드와 연결된 단순 연결 리스트를 말한다.(단순 연결 리스트와 동일한 구조를 지니지만, 순환 구조를 지닌다는 차이)

원형 연결 리스트의 첫 번째 노드의 위치 인덱스 값은 0이며 마지막 노드의 위치 인덱스는 currentElementCount - 1 이다.

(단순 연결 리스트에서 마지막 노드의 pLink값은 null)

```c
pPreNode === pNode->pLink
```

위치가 (position - 1)에 있는 노드가 이전 노드임을 아는 방법?

원형 연결 리스트에서 이전 노드에 접근하려면 각 노드의 링크를 따라 계속 이동하면서 위의 조건을 만족하는지를 점검하면 된다.

위 조건을 만족하는 노드이면 이전 노드이고, 그렇지 않으면 다음 노드로 이동하면 된다.

##### 6-1. 노드 추가

헤드 포인터를 이용해 구현하기 때문에 3가지 경우를 고려해야 한다.

![스크린샷 2024-01-31 오후 4 39 47](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/2915bf07-df91-49ff-86b3-050c72b3b2d1)

(case1) 리스트의 첫 번째 노드로 추가하는 경우와 (case2) 리스트의 중간에 노드를 추가하는 경우로 나눌 수 있다.

추가하는 노드가 리스트의 첫 번째 노드인지 아닌지로 나누는 이유는 마지막 노드의 처리 때문이다. (=== 첫 번째 노드인지 여부에 따라 마지막 노드의 다음 노드를 어떻게 설정해야 되는지가 달라진다.)

case 1-1. 공백 리스트의 첫 번째 노드로 추가하는 경우

![스크린샷 2024-01-31 오후 4 42 28](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/000398b7-13a0-4848-a353-41c3cf166e43)

공백으로 기존 null을 가리키던 헤드 포인터는 추가되는 pLink를 가리키며, 마지막 노드와 첫 노드 동일하므로 역시 자기 자신을 가리키게 된다.

원형 리스트의 pList의 헤드 포인터는 pList->pLink이며, 새로 추가하려는 노드를 가리키는 포인터 변수는 pNewNode이다.

```c
pList->pLink = pNewNode
pNewNode->pLink = pNewNode
```

case 1-2. 공백이 아닌 리스트의 첫 번째 노드로 추가하는 경우

![스크린샷 2024-01-31 오후 4 50 43](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/d8589117-3065-453f-a06b-64b70450d369)

기존 존재하는 첫 노드로 pList->pLink는 헤드 포인터 이다. 새로 추가하려는 노드를 가리키는 포인터 변수는 pNewNode이다.

원형 연결 리스트에서는 마지막 노드의 다음 노드가 첫 번째 노드가 되어야 하기 때문에 마지막 노드를 찾아야 한다.
<br/>
아래 조건을 만족하는 노드가 마지막 노드임을 알 수 있다.

```c
pLastNode->pLink === pList->pLink
```

```c
pList->pLink = pNewNode //리스트의 헤드 포인터는 새로 추가된 노드를 가리키도록 설정 
pNewNode->pLink = pLastNode->pLink  //새로 추가된 노드의 다음 노드로 기존 첫 번째 노드를 연결
pLastNode->pLink = pNewNode  //마지막 노드의 다음 노드로 새로 추가된 노드로 설정
```

case 2. 리스트의 중간 노드로 추가하는 경우

![스크린샷 2024-01-31 오후 5 00 03](https://github.com/dtwogud/dtwogud.github.io/assets/81230679/527cc491-1161-4db8-8636-e1728fb26b2f)

단순 연결 리스트와 거의 유사한 구조를 띈다.

추가하려는 위치(position)의 이전 노드를 탐색한다. position-1 인 노드를 찾는다.
-> 헤드 포인터 pList->pLink부터 시작해 position-1번 만큼 다음 노드로 이동하면 된다.

```c
pPreNode = pList->pLink
for(i = 0; i < position - 1; i ++){
  pPreNode = pPreNode->pLink
}
```

위치 인덱스가 position-1인 노드를 가리키는 포인터 변수가 pPreNode이다. 이 때 위치 인덱스가 position인 노드는 pPreNode->pLink이며, 새로 추가되는 노드는
pNewNode이다.

1) 추가하려는 노드의 다음 노드로 추가하려는 위치의 노드(pPreNode->pLink)를 설정한다.
<br/>
2) 위치 인덱스가 position-1인 노드의 다음 노드(pPreNode->pLink)로 새로 추가된 노드 pNewNode를 설정한다.

```c
pNewNode->pLink = pPreNode->pLink //1)
pPreNode->pLink = pNewNode  //2)
```

##### 6-1. 노드 제거

노드를 제거하는 경우에도 헤드포인터를 사용하기에 아래와 같이 세 가지 경우로 정리가 가능하다.

<img width="377" alt="스크린샷 2024-02-18 오후 5 49 54" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/d08ad58c-ac71-4179-8373-799368f8e3cb">

첫 번째 노드 제거일 경우에서 파생되는 '마지막 노드'의 경우는 현재 리스트에 남아있는 노드가 1개뿐이라는 뜻이다.
<br/>
즉, 마지막 노드를 삭제하면 원형 연결 리스트가 공백 리스트가 된다.

case 1-1. 마지막 노드 이면서 동시에 첫 번째 노드를 제거하는 경우

<img width="376" alt="스크린샷 2024-02-18 오후 5 53 19" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/6663c891-0775-4276-8be6-41d24a3091d1">

가장 단순하면서 특별한 경우로 해드 포인터는 null이 된다.

```c
pDelNode = pList -> pLink

free(pDelNode)
pList -> pLink = NULL
```

리스트의 첫 번째 노드를 삭제하려 하기 때문에 헤드 포인터가 가리키는 노드가 곧 삭제하려는 노드 pDelNoded이다.

case 1-2. 마지막 노드가 아닌, 첫 번째 노드를 제거하는 경우 

<img width="563" alt="스크린샷 2024-02-18 오후 5 55 10" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/17fccad6-11ec-4fc1-88e8-0101d01f05a2">

노드가 최소한 2개 이상이 있는 원형 연결 리스트의 첫 번째 노드를 제거하는 경우(즉, 첫 번째 노드가 삭제된 이후에도 리스트가 공백 상태가 아닌 경우)이다.

```c
pLastNode -> pLink = pDelNode -> pLink //1)
pList -> pLink = pDelNode -> pLink     //2)
free(pDelNode)                         //3)
```
마지막 노드를 찾았다면,
1) 마지막 노드의 다음 노드로 두 번째 노드를 대입
2) 기존 두 번째 노드가 원형 연결 리스트의 새로운 첫 번째 노드가 되도록 설정
3) 기존 첫 번째 노드의 메모리 해제

case 2. 리스트의 중간 노드를 삭제하는 경우

<img width="560" alt="스크린샷 2024-02-18 오후 6 00 17" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/f8ab7db8-940b-45ea-9717-2aa8e014743a">

삭제하려는 노드의 이전 노드를 찾아(위치 인덱스가 position -1 인 노드)

```c
pPreNode = pList -> pLink
for(i=0; i<position-1; i++) {
  pPreNode = pPreNode -> pLink;
}
pDelNode = pPreNode -> pLink; 
```

삭제하려는 노드의 이전 노드는 pPreNode로

```c
pPreNode -> pLink = pDelNode -> pLink  //1)
free(pDelNode)                         //2)
```

1) 이전 노드의 다음 노드 pPreNode -> pLink는 삭제하려는 노드의 다음 노드 pDelNode -> pLink가 되어야 한다.
2) 삭제 노드의 메모리를 해제시킨다.

#### 7. 이중 연결 리스트

<img width="328" alt="스크린샷 2024-02-18 오후 6 15 43" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/2317fc09-dadc-4c53-aadc-f30c73f28339">

노드들이 양방향으로 연결되어 있는 리스트로
<br/>
이전 노드에 접근에 대한 접근 편리성 vs 추가 링크(메모리)에 대한 필요

<img width="605" alt="스크린샷 2024-02-18 오후 6 17 14" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/29233d2b-dc2d-479a-b0fe-196ab5d6bd8c">

'헤더 노드'라는 특별한 노드를 사용한다.
<br/>
앞서 원형 연결 리스트에서 사용되던 헤드 포인터는 단순히 시작 노드를 가리키는 포인터였다면, 헤더 노드는 노드 자체를 사용하는 것으로 노드의 추가/삭제 구현을 쉽게 해준다.
<br/>
헤더 노드는 오른쪽 링크와 왼쪽 링크에 각각 연결 리스트의 첫 번째 노드와 마지막 노드를 가리키는 포인터 값을 저장하며
<br/>
이를 통해 노드의 추가/ 삭제 구현이 간편해 진다.

```c
pNode == pNode -> pLLink -> pRLink == pNode -> pRLink -> pLLink
//ㅋㅋㅋㅋ..?
```

##### 7-1. 이중 연결 리스트 노드 추가

<img width="544" alt="스크린샷 2024-02-18 오후 6 30 01" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/45efcd75-90d1-4b10-8456-a86084157611">

위치 인덱스가 position - 1 인 노드에 접근하기 위해 헤더 노드부터 시작해 position - 1 번 만큼 루프를 돌며 각 노드의 다음 노드로 이동한다.

```c
pPreNode = &(pList -> headerNode)
for(i=0; i<position; i++){
  pPreNode -> pPreNode -> pRLink;
}
```

추가하려는 위치의 이전 노드 pPreNode를 찾았기 때문에 다음과 같이 노드 사이의 링크를 재정의한다.
(!!구문 순서에 유의)

```c
pNewNode -> pLLink = pPreNode;              1)
pNewNode -> pRLink = pPreNode -> pRLink;    2)
pPreNode -> pRLink = pNewNode;              3)
pNewNode -> pRLink -> pLLink = pLLink -> pNewNode;    4)
```

1) 추가된 노드의 왼쪽 노드(pNewNode -> pLLink)를 추가하려는 위치 이전의 노드(pPreNode)로 설정한다.
2) 추가된 노드의 오른쪽 노드(pNewNode -> pRLink)는 추가하려는 위치의 노드(pPreNode -> pRLink, position)로 설정한다.
3) 추가하려는 위치 이전 노드의 오른쪽 노드(pPreNode -> pRLink)는 추가된 노드(pNewNode)로 설정한다.
4) 기존에 위치 인덱스 position에 있던 노드의 왼쪽 노드(pNewNode -> pRLink -> pLLink)로 추가된 노드(pNewNode)를 설정한다.

!! 위치 position에 있던 기존 노드는 pNEwNode -> pRLink로 접근할 수 있다.

##### 7-2. 이중 연결 리스트 노드 삭제

<img width="524" alt="스크린샷 2024-02-18 오후 6 45 50" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/e3c31ada-efee-4871-8c39-480c7560cf8f">

위치 인덱스(position - 1)인 노드를 기반으로
<br/>
삭제 노드의 위치 인덱스가 position인 pPreNode -> pRLink, 삭제 노드의 다음 노드는 위치 인덱스가 (position + 1)이며 pDelNode -> pRLink가 가리킨다.

```c
pPreNode -> pRLink = pDelNode -> pRLink    //1)
pDelNode -> pRLink -> pLLink = pPreNode    //2)
free(pDelNode)    //3) 
```

1) pPreNode의 오른쪽 노드로 pDelNode -> pRLink를 대입한다.
2) 포인터 변수 pDelNode -> pRLink -> pLLink(삭제하려는 노드의 오른쪽 노드의 왼쪽 노드)에 pPreNode를 대입해준다.
3) 삭제 노드의 메모리를 해제 시킨다.


#### 8. 연결 리스트의 응용

지금까지 살펴본 연결 리스트를 다항식 문제에 적용해 연결 리스트가 실제 문제에서 어떻게 적용되는지 알아보자.
<br/>
앞서 다루지 않은 몇 가지 리스트 연산은 각각 아래와 같다.
<br/>

1) 순회(Iteration) 함수<br/>
2) 다른 연결 리스트를 연결시키는(Concatenate) 함수<br/>
3) 특정 연결 리스트를 역순(Reverse)으로 만드는 함수


##### 8-1. 연결 리스트 순회

기존 연결 리스트의 노드들을 차례대로 방문해 노드의 데이터를 출력하는 함수는 위치 인덱스 position을 입력받아 항상 첫 번째 노드부터 시작해 해당 위치의 노드로 이동하는 방식이었다.  
즉, 위치 인덱스 position에 해당하는 노드로 이동할 때까지 모두 (position + 1)번 만큼의 노드 이동 연산을 수행해야 한다.  

<img width="565" alt="스크린샷 2024-02-25 오후 4 18 03" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/833072d9-c5e5-4a3f-85ea-3c0f3ddb3f7e">  
  
노드 사이에 연결된 노드의 링크를 이용해 효율적으로 리스트를 순회하고 있다. 링크(pLink)를 이용해 리스트의 시작 노드부터 시작해 노드가 끝날 때 까지 다음 노드로 이동한다.

기존 displayLinkedList()는 현재 position에서 다음 노드(position + 1)까지의 이동만 구현하는 경우에도  
리스트의 헤더노드로 이동해 position + 1 번 만큼의 이동이 필요해 효율적이지 못한 경우가 있었다.

```c
for(i=0; i<pList->currentElementCount; i++) {
  printf("[%d], %d\n", i, getLLElement(pList, i) -> element);
}
```

그에 반해 iterateLinkedList() 는 링크(pLink)를 이용해 리스트의 시작 노드부터 시작해 노드가 끝날 때 까지 다음 노드로 이동한다.  

```c
pNode = pList -> headerNode.pLink;
while(pNode !== NULL) {
  printf("[%d], %d\n", count, pNode -> data);
  count ++;
  
  pNode = pNode -> pLink
}
```

추가로 위 함수는 리스트의 노드를 순회할 때 연결 리스트의 현재 노드 개수를 계산하고 있어, currentElementCount를 이용하지 않고 각 노드 순회 시 count변수를 1씩 증가시켜 노드 개수를 구한다.  

##### 8-2. 연결 리스트 연결  

특정 연결 리스트의 마지막에 다른 연결 리스트를 연결(Concatenate)하는 함수를 구현해보자.  

<img width="476" alt="스크린샷 2024-02-25 오후 4 41 15" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/9810f310-f11b-43b8-b44f-6fe11708dacb">

!! 주의 사항
1. 기존 연결 리스트의 링크는 끊어줘야 한다.(메모리 할당 해제 시 중복될 수 있음)  
2.합쳐진 리스트의 노드 개수는 서로 변경되었기에 노드 개수를 변경시켜 주어야 한다  
  
1) 연결 리스트A의 마지막 노드 찾기  
2) 연결 리스트A의 마지막 노드로 연결 리스트B의 첫 번째 노드로 연결 및 노드 개수 증가  
3) 연결 리스트B의 첫 번째 노드로 NULL대입 및 노드 개수 0으로 설정

```c
//1)
pNodeA = pListA -> headerNode.pLink;
while(pNodeA->pLink !== NULL){
  pNodeA = pNodeA -> pLink;
}
//pNodeA가 리스트 A의 마지막 노드가 됨.

//2)
pNodeA -> pLink = pListB -> headerNode.pLink;
pListA -> currentElementCount += pListB -> currentElementCount;   //주의 사항 2번

//3)
pListB -> headerNode.pLink = NULL;
pListB -> currentElementCount = 0;
```

##### 8-3. 연결 리스트 역순 만들기

리스트의 첫 번째 노드가 마지막 노드가 되고, 두 번째 노드가 뒤에서 두 번째 노드가 된다.  
각 노드의 다음 노드로의 링크를 통해 순서대로 이동하면서 기존 링크 대신 역방향 링크를 설정한다.  

<img width="537" alt="스크린샷 2024-02-25 오후 4 49 07" src="https://github.com/dtwogud/dtwogud.github.io/assets/81230679/9eec1895-b245-456d-b53f-5bab8067f073">

1) 시작 노드 설정 (헤더 노드의 다음 노드)  
2) 두개 노드의 순서 바꾸기  
   1) 포인터 변수 pPrevNode는 이전 노드를 가리키도록 대입(리스트의 첫 번째 노드의 경우는 NULL)  
   2) 포인터 변수 pCurrentNode는 현재 노드를 가리킨다.  
   3) 포인터 변수 pNode는 리스트의 노드 순회를 위한 변수로 현재 노드의 다음 노드로 이동시킨다.  
   4) 현재 노드의 다음 노드(pCurrentNode -> pLink)가 이전 노드(pPrevNode)가 되도록 설정  
3) 헤더 노드의 다음 노드 설정  
4) 리스트의 제일 마지막 노드 pReverse 대입

```c
//1)
pNode = pList -> headerNode.pLink;

//2)
while(pNode !== NULL) {
  pPrevNode = pCurrentNode;  //2-1)
  pCurrentNode = pNode       //2-2)
  pNode = pNode -> pLink;    //2-3)
  pCurrentNode -> pLink = pPrevNode    //2-4)
}

//3), 4)
pList -> headerNode.pLink = pCurrentNode; 
```