# Queue

#자료구조#큐

## 개념

- 먼저 온 사람이 먼저 나가는 줄서기와 같은 구조
- 프로세스 관리, BFS 알고리즘 등

## 특징

1. 데이터의 삽입과 삭제는 **_한 쪽 끝에_** 서만 수행됨
2. 나중에 들어간 것이 먼저 나오는 **FIFO (First-In, Fisrt-Out)\_** 구조
3. **_삽입, 삭제_** 에서 시간 복잡도 O(1)을 유지해야 함

## 구현 메서드

- `enqueue(value)`: 맨 뒤에 데이터 추가
- `dequeue()`: 맨 앞 데이터 삭제하고 삭제된 데이터 반환
- `imEmpty()`: `Queue`가 비어있는 상태인지 `Boolean` 값 반환
- `front()`: 맨 앞에 있는 데이터 반환

## 구현 방법

1. `Array`
2. `Linked List`

## Array Implementation

```js
const Queue = function (array = []) {
  this.queue = array;
};

Queue.prototype.enqueue = function (value) {
  this.queue.push(value);
  return this;
};

Queue.prototype.dequeue = function () {
  return this.queue.shift();
};

Queue.prototype.isEmpty = function () {
  return this.queue.length === 0;
};

Queue.prototype.front = function () {
  return this.isEmpty() ? undefined : this.queue[0];
};
```

## Linked List Implementation

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Queue {
  constructor() {
    this.front = null;
    this.rear = null;
    this.length = 0;
  }

  enqueue(value) {
    const newNode = new Node(value);
    if (!this.front) {
      this.front = newNode;
      this.rear = newNode;
    } else {
      this.rear.next = newNode;
      this.rear = newNode;
    }
    this.length++;
    return this;
  }

  dequeue() {
    if (!this.front) return undefined;

    const removedNode = this.front;
    this.front = removedNode.next;
    removedNode.next = null;
    this.length--;
    if (this.length === 0) this.rear = null;
    return removedNode.value;
  }

  isEmpty() {
    return this.length === 0;
  }

  front() {
    if (!this.front) return undefined;
    return this.front.value;
  }
}
```
