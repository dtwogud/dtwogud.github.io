---
title : "[React] React Query (2)"
excerpt: "React Query의 caching, Synchronization"

categories:
  - React
tags:
  - [JavaScript, React, React Query]

toc: true
toc_sticky: true

date: 2022-09-10
last_modified_at: 2022-09-10
---
<br>

## React Query

> React-query를 이용한 상태관리를 공부해보자
> <a href="https://tanstack.com/query/v4/docs/guides/window-focus-refetching?from=reactQueryV3&original=https://react-query-v3.tanstack.com/guides/window-focus-refetching">공식 홈페이지</a>

### RCF 5861
> <a href="https://www.rfc-editor.org/rfc/rfc5861/">RFC 5861</a> (HTTP Cache-Control Extensoins for stale Content)
stale-while-validate: 백그라운드에서 stale response를 revalidate하는 동안 캐시가 가진 stale response를 반환
```js
Cache-Control: max-age=600, stale-while-revalideate=30
// 600초 내에는 유효한 캐시, 600초가 지난 이후 30초동안 stale data반환, API refetch
```

### 메모리 캐시에 적용
-. cacheTime: 메모리에 얼마만큼 있을건지(해당 시간 이후 GC에 의해 처리, defautl 5분)
-. staleTime: 얼마의 시간이 흐른 후 데이터를 stale취급할 것인지(default 0)
-. refetchOnMount / refetchOnWindowFocus / refetchOnReconnect: true일 경우 Mount / windowFocus / reconnect 시점에 data가 stale이라고 판단되면 모두 refetch(all default true)

### Query 상태 흐름

#### ㅁ 화면에 있다가 사라지는 query

<img src="https://velog.velcdn.com/images/godud2604/post/18d9c00f-83b4-4e3d-b803-3f7ae7a76316/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.38.07.png" />

#### ㅁ 화면에 있다가 없어지는 좀 더 복잠한 query
<img src="https://velog.velcdn.com/images/godud2604/post/4c737619-1318-4f17-9e4f-983fa3285bfc/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.41.41.png" />

#### zero-config에서 주의할 점
ㅁ staleTime: defalut 0
  -. Queries에서 cached data는 언제나 stale취급
ㅁ refetchOnMount / refetchOnWindowFocus / refetchOnReconnect: defalut true
  -. 각 시점에서 data가 stale이라면 항상 refetch 발생
ㅁ cacheTime: default 60*5*1000
  -. inActive Query들은 5분 뒤 GC에 의해 처리
ㅁ retry: defalut 3
ㅁ retryDelay: defalut exponential backoff function
  -. Query실패 시 3번까지 retry 발생

