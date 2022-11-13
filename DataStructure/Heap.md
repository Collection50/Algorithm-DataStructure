# Heap

#자료구조#힙#이진힙

## 개념

- 완전 이진 트리의 일종, 기본적으로 **_힙 == 트리_** (트리에 적용되는 것들이 힙에도 적용)
- **우선순위 큐를 위하여** 만들어진 자료구조
- 최소 힙, 최대 힙 등 존재
- 시간 복잡도  
  삽입: O(logN)  
  삭제: O(logN)

#### 이진 힙(Binary Heap)

- 이진 탐색 트리(Binary Serach Tree, BST)와 굉장히 비슷
- 각 노드는 **_최대 2개의 자식_** 을 가질 수 있음 (0 ~ 2)
- 최대 이진 힙 == (부모 노드 값 > 자식 노드 값)
- 최소 이진 힙 == (부모 노드 값 < 자식 노드 값)

### <아래 모든 내용 == **_이진 힙_** 기반>

## 특징

1. 여러 값들 중 **_최댓값 or 최솟값_** 빠르게 찾아내도록 만들어진 자료구조
2. **_느슨한 정렬_** 상태 유지
3. 모든 노드에 대해 **_(부모 노드 값 > 자식 노드 값)_** 만족 (in 최대 이진 힙, 최소 이진 힙은 반대)
4. **_형제 노드 간 대소 비교 없음_** (BST와의 차이점)
5. 최적 용량 사용 (가능한 가장 적은 용량)
6. 노드가 채워지는 순서: **_왼쪽 => 오른쪽_**
7. 보통 배열로 구현되며, 완전 이진 트리의 일종이기에 index를 통해  
   부모 <=> 자식 간 접근 용이
8. 부모 index 기준  
   **_왼쪽 자노드: index \* 2 + 1_** , **_오른쪽 자노드: index \* 2 + 2_**
9. 자식 index 기준  
   **_부모 노드 == Math.floor(index - 1) / 2_**

## 구현 메서드

1. `insert(value)`:

- 이진 힙에 데이터 추가
- 데이터 채우는 방향: 왼쪽 => 오른쪽 (왼쪽 트리 레벨이 모두 채워진 이후 오른쪽 트리의 레벨 순서)
- 배열 맨 끝에 삽입하는 것이지만 (Push to the end)
- 값에 따라 알맞은 장소를 찾을 때까지 자리를 바꿔줘야 함 (`Bubble up`)

2. `remove()`:

- 최대 힙 == 최대값(root) 제거
- 최소 힙 == 최소값(root) 제거
- 최적 용량을 유지하기 위해선 루트 제거 후 힙의 맨 뒤(배열의 맨 뒤)에 있는 값과 자리 교체(`Sink Down`)

### `remove()` 상세 설명

- `swap()`: 배열 값 바꾸기
- `bubbleUp()`: 양쪽 자노드의 유효(존재) 여부에 따라 재귀호출
  1. 왼쪽 노드, 오른쪽 노드 모두 유효하지 않을 때: `return`
  2. 왼쪽만 유효할 떄: 왼쪽 노드 값이 더 크다면 왼쪽과 `swap()`
  3. 오른쪽만 유효할 떄: 오른쪽 노드 값이 더 크다면 오른쪽과 `swap()`
  4. 양쪽 다 유효할 때: 현재 노드, 왼쪽 노드, 오른쪽 노드 중 `max`값을 찾고 큰 값과 `swap()`, 현재 노드가 가장 크다면 `return`

### `remove()` summary

1. 루트, 맨 끝 값 교환
2. index가 유효한지 확인 (자노드의 존재 여부 확인)
3. 현재 노드의 값과 양쪽 값 중 큰 값과 교환 반복
4. 만약 양쪽 값이 자기 자신보다 작다면 멈춤

### Implementation

첫 번째 `remove()` == 혼자서 생각하며 재귀로 구현  
4가지 경우의 수로 나누어 재귀 함수를 호출하거나 종료하도록 하였는데  
**빈틈없이, 겹치지 않게** 작성하였지만 막상 짜놓고 보니 가독성도 좋지 않고 지저분한 코드처럼 보인다  
~~값 교환이 지저분할까봐 `swap()`도 만들었는데...~~  
`while (true)`와 `break`를 사용하기 싫어 재귀로 작성하였는데 중요한 것이 무엇인지 잊은 느낌이다  
하지만 재귀의 호출 방식을 정확하게 이해했으며 혼자 힘으로 문제를 해결했다는 것에 큰 의의를 느끼며 기록한다  
진짜 실무에서는 어떠한 방식으로 어떻게 사용될지 궁금하다

## Implementation Code

```js
class MaxBinaryHeap {
  constructor() {
    this.values = [];
  }

  insert(value) {
    this.values.push(value);
    let index = this.values.length - 1;

    // Bubble up
    const element = this.values[index];
    while (index > 0) {
      const parentIdx = Math.floor((index - 1) / 2);
      const parent = this.values[parentIdx];
      if (element < parent) break;

      this.values[parentIdx] = element;
      this.values[index] = parent;
      index = parentIdx;
    }
    return this;
  }

  remove() {
    const swap = (array, index1, index2) => {
      const temp = array[index1];
      array[index1] = array[index2];
      array[index2] = temp;
    };
    const sinkDown = function (array, index, size) {
      const leftIndex = index * 2 + 1;
      const rightIndex = index * 2 + 2;
      let swapIndex;

      if (leftIndex >= size && rightIndex >= size) return;

      if (leftIndex < size && rightIndex >= size) {
        swapIndex = array[leftIndex] > array[index] ? leftIndex : index;
      } else if (leftIndex >= size && rightIndex < size) {
        swapIndex = array[rightIndex] > array[index] ? rightIndex : index;
      } else {
        const max = Math.max(array[index], array[rightIndex], array[leftIndex]);
        if (max === array[index]) swapIndex = index;
        else if (max === array[leftIndex]) swapIndex = leftIndex;
        else swapIndex = rightIndex;
      }
      if (index === swapIndex) return;
      swap(array, index, swapIndex);
      sinkDown(array, swapIndex, size);
    };

    if (this.values.length < 1) return null;
    const ZERO = 0;
    const lastIndex = this.values.length - 1;
    swap(this.values, lastIndex, ZERO);

    const removedNode = this.values.pop();

    sinkDown(this.values, ZERO, this.values.length);
    return removedNode;
  }
}
```

### Refactoring(`remove()`)

iteration을 활용한 Refatoring  
아래 코드는 노드의 제거 기능을 수행하기엔 코드의 가독성이 목적에 명확히 들어맞는다는 느낌이 들지 않았었는데  
내가 직접 작성한 코드보다 나은 것 같기도..?

### `remove()` Refactoring Code

```js
remove() {
  sinkDown() {
    let index = 0;
    const size = this.values.length;
    const element = this.values[0];

    while (true) {
      let leftChildIdx = 2 * index + 1;
      let rightChildIdx = 2 * index + 2;
      let leftChild;
      let rightChild;
      let swap = null;

      if (leftChildIdx < size) {
        leftChild = this.values[leftChildIdx];
        if (leftChild > element) swap = leftChildIdx;
      }
      if (leftChildIdx < size) {
        rightChild = this.values[rightChildIdx];
        if (
          (swap === null && rightChild > element) ||
          (swap !== null && rightChild > leftChild)
        ) swap = rightChildIdx;
      }
      if (swap === null) break;
      this.values[index] = this.values[swap];
      this.values[swap] = element;
      index = swap;
    }

    const max = this.values[0];
    const end = this.values.pop();
    if (this.values.length > 0) {
      this.values[0] = end;
      this.sinkDown();
    }
    return max;
  }
}
```
