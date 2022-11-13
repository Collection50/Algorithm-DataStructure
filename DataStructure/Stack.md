# Stack

#자료구조#스택

## 개념

- 책을 쌓아올려 놓은 것처럼 구성되는 자료구조
- 함수의 복귀 주소, 웹 방문 기록 등에 사용

## 특징

1. 데이터의 삽입과 삭제는 **_한 쪽 끝에_** 서만 수행됨
2. 나중에 들어간 것이 먼저 나오는 **_LIFO (Last-In, Fisrt-Out)_** 구조
3. **_삽입, 삭제_** 의 시간 복잡도를 O(1)을 유지해야 함

## 구현 메서드

- `push(value)`: 맨 끝에 데이터 추가
- `pop(value)`: 맨 끝 데이터 삭제하고 삭제된 데이터 반환
- `imEmpty()`: `stack`이 비어있는 상태인지 `Boolean` 값 반환
- `peak()`: 맨 끝에 있는 데이터가 무엇인지 반환

## 구현 방법

1. `Array`
2. `Linked List`

## Array Implementation

```js
const Stack = function (array = []) {
  this.stack = array;
};

Stack.prototype.push = function (value) {
  this.stack.push(value);
  return this;
};

Stack.prototype.pop = function () {
  return this.stack.pop();
};

Stack.prototype.isEmpty = function () {
  return this.stack.length === 0 ? true : false;
};

Stack.prototype.peak = function () {
  return this.stack.length === 0 ? undefined : this.stack[this.length - 1];
};
```

## Linked List Implementation

#### **_Linked List에서의 삽입과 삭제_**

- 스택의 데이터 추가 및 삭제 == 시간 복잡도 O(1)을 유지해야 하므로 (상수 시간)
- `push`, `pop`보다 `shift`, `unshift` 사용하는 것이 효율적
- (왜냐하면 Linked List는 맨 끝에 데이터를 추가하려면 O(N)의 시간 복잡도가 소요되기 때문)
- 링크드 리스트는 배열과 다르게 shift, unshift 사용 시 데이터의 이동 없음
- 즉, LIFO 구조 + 삽입 삭제의 시간 복잡도 O(1)을 만족시킨다면 괜찮다는 의미

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class Stack {
  constructor() {
    this.front = null;
    this.rear = null;
    this.length = 0;
  }

  // unshift
  push(value) {
    const newNode = new Node(value);
    if (!this.front) {
      this.front = newNode;
      this.rear = newNode;
    } else {
      newNode.next = this.front;
      this.front = newNode;
    }
    this.length++;
    return this;
  }

  // shift
  pop() {
    if (!this.front) return undefined;
    const poppedNode = this.front;
    this.front = poppedNode.next;
    poppedNode.next = null;
    this.length--;

    if (this.length === 0) this.rear = null;
    return poppedNode.value;
  }

  isEmpty() {
    return !this.front ? true : false;
  }

  peak() {
    return !this.front ? undefined : this.front.value;
  }
}
```
