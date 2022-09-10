---
title : "[React] React Query"
excerpt: "React Query의 3가지 Core Concept"

categories:
  - React
tags:
  - [JavaScript, React, React Query]

toc: true
toc_sticky: true

date: 2022-00-04
last_modified_at: 2022-09-04
---
<br>

## React Query

> React-query를 이용한 상태관리를 공부해보자
> <a href="https://tanstack.com/query/v4/docs/guides/window-focus-refetching?from=reactQueryV3&original=https://react-query-v3.tanstack.com/guides/window-focus-refetching">공식 홈페이지</a>

### Query Key

> Key, Value 맵핑구조와 유사

- React Query는 Query Key에 따라 query chching을 관리한다.
-. String 형태
-. Array 형태

### Query Function

> Promise를 반환하는 함수로 데이터를 resolve하거나 error throw

### useQuery의 대표적인 반환값

```js
const {
  data,
  error,
  isFetching,
  status,
  isLoading,
  isSuccess,
  refetch,
  remove,
  ...
}
```

ㅁ data: 마지막으로 성공한 resolved된 데이터(Response)
ㅁ error: 에러가 발생했을 때 반환되는 객체
ㅁ isFetching: Request가 in-flight중일 때 true
ㅁ status, isLoading, isSuccess 등: 현재 query의 상태
ㅁ refetch: 해당 query refetch하는 함수 제공
ㅁ remove: 해당 query cache에서 지우는 함수 제공

### useQuery의 대표적인 Option

```js
} = useQuery(queryKey, queryFn?,{
  onSuccess,
  onError,
  onSettled,
  enabled,
  reyty,
  select,
  keepPreviousData,
  refetchInterval
})
```

ㅁ onSuccess, onError, onSettled: query fetching 성공,실패,완료 시 실행할 side Effect정의
ㅁ enable: 자동을 query를 실행시킬지 말지 여부
ㅁ retry: query동작 실패 시 자동으로 retry 할지 결정하는 옵션
ㅁ select: 성공 시 가져온 data를 가공해서 전달
ㅁ keepPreviousData: 새롭게 fetching시 이전 데이터 유지 여부
ㅁ refetchInterval: 주기적으로 refetch할지 결정하는 옵션

#### query가 여러 개일 때
```js
function App () {
  <!-- 알아서 병렬로 진행해 줌 -->
  const usersQuery = useQuery('users', fetchUsers)
  const teamsQuery = useQuery('teams', fetchTeams)
  const projectsQuery = useQuery('projects', fetchProjects)
}
```

#### Mutaiton
> Data를 생성/수정/삭제 에 사용

```js
const mutation = useMutation(newTodo => {
  return axios.post('/todos', newTodo)
})

const {
  data,
  error,
  isError,
  isIdle,
  isLoading,
  isPaused,
  isSuccess,
  mutate,
  mutateAsync,
  reset,
  status
} = useMutation(mutationFn,{
  mutationKey,
  onError,
  onMutate,
  onSettled,
  onSuccess,
  retry,
  retryDelay,
  useErrorBoundary,
  meta
  })
```
-. query와 달리 자동으로 실행 X
-. useQuery보다 더 심플하게 Promise 반환 함수만 있어도 가능(단, Query Key 넣어주면 devtools에서 확인 가능)

-. mutate: mutation을 실행하는 함수
-. mutateAsync: mutate와 비슷하지만, Promise 반환
-. reset: mutation 내부 상태 clean
-. 나머지는 useQuery와 비슷함

-. onMutate: 본격적인 Mutation동작 전에 먼저 동작하는 함수, Optimistic update 적용 시 유용
> OptimisticUI update
ex) 페이스북 좋아요 버튼과 같은 경우 누름과 동시에 색 변경이 먼저 진행(API 통신은 성공할 것이라는 예상(optimistic한 기대와 함께 선 동작)), 실패 시 rollback

#### Query Invalidation
-. 간단히 queryClient를 통해 invalidation 메소드를 호출
```js
//Invalidate every quert in chache
queryClient.invalidateQueris()
//Invalidate every query with a key that starts with 'todos'
queryClient.invalidateQueris('todos')
```
> 해당 Key를 가진 query는 stale 취급되고, 현재 rendering되고 있는 query들은 백그라운드에서 refetch

