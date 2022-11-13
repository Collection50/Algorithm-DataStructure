# Tortoise & Hare

#알고리즘#연결리스트#토끼와거북이

## 개념

- 링크드 리스트의 Cycle 여부 확인
- 거북이와 토끼가 경주를 하는 것처럼, 토끼 == 2개 노드씩 이동하고 거북이 == 1개씩 이동
- 거북이와 토끼 만남 == Cycle 존재, 토끼가 먼저 `null`에 도달 == Cycle 없음

## 특징

1. 사이클이 존재할 때, 사이클의 **시작점**과 **길이**를 _**시간 복잡도 O(N)**_ 으로 탐색 가능
2. 링크드 리스트의 **투 포인터**

## 구현 메서드

- `hasCycle(linkedList)`: 링크드 리스트 Cycle 존재 여부(`true`, `false`), `tort`, `hare` 반환
- `getCycleStartNode(value)`: Cycle 존재 시 Cycle 시작 노드 반환
- `getCycleLength()`: Cycle 존재 시 Cycle의 길이 반환

## Explanation

1. `hasCycle()`: `tort`, `hare가` 같은 노드를 가리키는지에 따라 링크드 리스트의 여부 확인
2. `getCycleStartNode()`: Cycle이 존재한다면, tort를 시작 노드로 보내고  
   `tort`, `hare`가 같은 노드를 가리킬 때까지 한 칸 씩 이동, 같은 노드를 가리킬 때 == Cycle의 시작점
3. `getCycleLength()`: Cycle이 존재한다면, Cycle을 한 번 순회할 때 방문하는 노드의 개수 반환

## Implementation

```js
const hasCycle = (linkedList) => {
  let tort = linkedList;
  let hare = linkedList;
  while (hare && hare.next) {
    tort = tort.next;
    hare = hare.next.next;
    if (hare === tort) return [tort, hare, true];
  }
  return [tort, hare, false];
};

const getCycleStartNode = function (linkedList) {
  let [tort, hare, isCycle] = hasCycle(linkedList);
  if (!isCycle) return null;

  tort = linkedList;
  while (hare !== tort) {
    tort = tort.next;
    hare = hare.next;
  }
  return hare;
};

const getCycleLength = function (linkedList) {
  const cycleStartNode = getCycleStartNode(linkedList);
  const cycleEndNode = cycleStartNode.next;
  if (!cycleStartNode) return null;

  let count = 1;
  while (cycleStartNode !== cycleEndNode) {
    cycleStartNode = cycleStartNode.next;
    count++;
  }
  return count;
};
```

## 배운 점

- 처음엔 `hasCycle()`을 아래처럼 작성하였다
- 하지만 반환 값에서 `push` 사용을 자제해야 한다는 것을 알고 리팩토링 시행

```js
const hasCycle = (linkedList) => {
  let tort = linkedList;
  let hare = linkedList;
  let isCycle = false;
  while (tort && tort.next && !isCycle) {
    hare = hare.next;
    tort = tort.next.next;
    if (hare === tort) isCycle = true;
  }
  return [tort, hare].push(isCycle);
};
```

## 궁금한 점

- 더하여 `is~`, `has~`로 시작하는 함수들의 반환형은 불리언인데, `hasCycle()`처럼 `boolean`값과 다른 값이 포함된 형태로 반환해도 되는지 의문이 남는다
