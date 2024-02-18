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

#### 8. 연결 리스트의 응용

