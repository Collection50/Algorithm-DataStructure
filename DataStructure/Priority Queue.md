# Priority Queue

#자료구조#우선순위큐

## 개념

- **_힙_** 을 활용해 구현되는 자료구조 [(힙)](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Heap.md)
- 따라서 힙의 성질을 동일하게 소유하고 있음
- 각 요소가 그에 해당하는 **_우선순위_** 소유 (가장 중요)
- 우선순위 큐를 위한 자료구조로 힙이 존재하지만, 추상적인 개념일뿐 **같은 것을 의미하지는 않음**
- 우선순위 큐를 보면서 힙을 이해하고, 힙을 보면서 우선순위 큐를 이해할 수 있도록 한다
- 아래 우선순위 큐는 힙과 동일하게 **_배열_** 을 사용하여 구현

## 특징

1. 더 낮은 우선순위를 가진 요소가 더 높은 우선순위를 가진 요소보다 **_먼저_** 처리
2. **_삽입, 제거_** 에 있어 굉장히 유용
3. 일반 큐(Queue)와의 차이점
   일반 큐: 선형적 형태(Linear)  
   우선순위 큐: 트리 형태(Tree)
4. 시간 복잡도  
   삽입: O(logN)  
   삭제: O(logN)

## 구현 메서드

- `enqueue(value, priority)`: 우선순위 큐에 노드 추가 (push)
- `bubbleUp()`: 추가된 노드의 우선순위에 따라 적절한 자리로 **_배치_** (우선순위 순서대로)
- `dequeue()`: 우선순위 큐의 노드 삭제, 우선순위가 가장 **_낮은 것이 먼저_** 처리되며, 가장 **끝**에 있는 노드를 맨 **앞**으로 위치시킴
- `sinkDown()`: `dequeue` 수행 이후 맨 앞으로 이동한 노드를 우선순위에 따라 적절한 자리로 **_배치_** (우선순위 순서대로)

## Implementation

```js
class Node {
  constructor(value, priority) {
    this.value = value;
    this.priority = priority;
  }
}

class PriorityQueue {
  constructor() {
    this.values = [];
  }

  enqueue(value, priority) {
    const newNode = new Node(value, priority);
    this.values.push(newNode);
    this.bubbleUp();
  }

  bubbleUp() {
    let index = this.values.length - 1;
    const element = this.values[index];
    while (index > 0) {
      const parentIdx = Math.floor((index - 1) / 2);
      const parent = this.values[parentIdx];
      if (element.priority >= parent.priority) break;
      this.values[parentIdx] = element;
      this.values[index] = parent;
      index = parentIdx;
    }
  }

  dequeue() {
    const min = this.values[0];
    const end = this.values.pop();
    if (this.values.length > 0) {
      this.values[0] = end;
      this.sinkDown();
    }
    return min;
  }

  sinkDown() {
    let index = 0;
    const size = this.values.length;
    const element = this.values[0];

    while (true) {
      const leftChildIdx = index * 2 + 1;
      const rightChildIdx = index * 2 + 2;
      let leftChild;
      let rightChild;
      let swap = null;

      if (leftChildIdx < size) {
        leftChild = this.values[leftChildIdx];
        if (leftChild.priority < element.priority) swap = leftChildIdx;
      }

      if (rightChildIdx < size) {
        rightChild = this.values[rightChildIdx];
        if (
          (swap === null && rightChild.priority < element.priority) ||
          (swap !== null && rightChild.priority < leftChild.priority)
        )
          swap = rightChild;
      }
      if (swap === null) break;
      this.values[index] = this.values[swap];
      this.values[swap] = element;
      index = swap;
    }
  }
}
```
