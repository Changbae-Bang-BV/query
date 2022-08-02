---
id: query-keys
title: Query Keys
---

핵심적으로, React Query 는 쿼리키를 기반으로 쿼리 케시를 관리합니다. 쿼리 키는 최상위 수준에서 배열이어야 합니다. 쿼리키는 단일어로 이루어져 있는 배열처럼 단순할 수도 있고 복합어과 중첩된 개체의 배열처럼 복잡할 수도 있습니다. 쿼리 키가 직력화가 가능해야하며 **쿼리 데이터 별로 고유**하게 사용해야 합니다.

## Simple Query Keys (간단한 쿼리키)

상수 값 배열이 키의 가장 단순한 형태입니다. 이러한 형태는 다음 상황과 같을때 유용합니다.
- Generic List/Index resources (일반 목록/인덱스 리소스)
- Non-hierarchical resources (계층이 없는 리소스)

```tsx
// A list of todos
useQuery(['todos'], ...)``

// Something else, whatever!
useQuery(['something', 'special'], ...)
```

## Array Keys with variables (변수를 사용하는 쿼리키)

고유한 값을 가지고 쿼리의 데이터를 더 정확하기 설명하기 위해셔 문자 혹은 숫자로 이루어진 쿼리 키를 사용할 수 있습니다. 다음과 같은 경우에 유용합니다.

- Hierarchical or nested resources (계층 혹은 중첩된 리소스)
  - ID, 인덱스와 같이 고유 식별이 가능한 값을 쿼리키로 사용합니다.
- Queries with additional parameters (파라메터가 있는 쿼리)
  - 파라메터를 쿼리키로 사용합니다.

```tsx
// An individual todo
useQuery(['todo', 5], ...)

// An individual todo in a "preview" format
useQuery(['todo', 5, { preview: true }], ...)

// A list of todos that are "done"
useQuery(['todos', { type: 'done' }], ...)
```

> `When a query needs more information to uniquely describe its data, you can use an array with a string and any number of serializable objects to describe it.` 를 간단하게 쿼리 키로서 문자 혹은 숫자로 이루어진 쿼리키를 사용할 수 있다고 의역하였습니다.

## Query Keys are hashed deterministically! (쿼리 키는 결국 해시됩니다!!)
> 쿼리 키는 순서 영향이 있습니다.
> 객체로 설정된 키에 대해서는 순서 상관이 없지만, 그렇지 않다면 순서가 고려 되어야 합니다.

아래 쿼리들은 쿼리 키의 순서와 상관 없이 같은 것으로 취급됩니다.

```tsx
useQuery(['todos', { status, page }], ...)
useQuery(['todos', { page, status }], ...)
useQuery(['todos', { page, status, other: undefined }], ...)
```

하지만 다음 쿼리키들은 같지 않습니다. 배열의 아이탬 순서가 중요합니다.

```tsx
useQuery(['todos', status, page], ...)
useQuery(['todos', page, status], ...)
useQuery(['todos', undefined, page, status], ...)
```

## If your query function depends on a variable, include it in your query key (변수가 데이터를 변경한다면 쿼리키에 넣어주세요)

Since query keys uniquely describe the data they are fetching,
they should include any variables you use in your query function that **change**.
For example:

Since query keys uniquely describe the data they are fetching, they should include any variables you use in your query function that **change**. For example:


쿼리 키는 가져오는 데이터와 고유하게 연결되어 있기 때문에, **정확한** 데이터를 가져오기 위해 모든 변수를 포함해야 합니다. 아래는 예입니다.

```tsx
function Todos({ todoId }) {
  const result = useQuery(['todos', todoId], () => fetchTodoById(todoId))
}
```

## Further reading (더 읽으면 좋은 것)

더 큰 어플리케이션을 위한 쿼리 키에 대한 팀은 Community Resources 의 [Effective React Query Keys](../community/tkdodos-blog#8-effective-react-query-keys) 를 참고해 주세요.
