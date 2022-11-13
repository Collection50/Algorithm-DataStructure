# DoublyLinkedList

#자료구조#양방향연결리스트

## 개념

- 링크의 리스트(목록)
- 데이터 + 다음 노드 + 이전 노드의 주소를 가진 노드들을 **늘여놓은 형태**의 자료구조
- 이전 노드 + 다음 노드를 가리키는 **포인터**를 사용하여 구현

## 특징

1. 단방향 연결리스트와 비교하여, 더 편리하지만 더 많은 메모리 차지
2. **More Memory === More Flexibility**
3. 양쪽으로 노드를 연결하는 포인터의 존재를 제외하면 단방향 연결리스트와 **동일**
4. 탐색의 경우 범위를 반으로 줄이기 때문에 단방향 연결리스트보다 시간 복잡도 **소폭 향상(still O(N))**
5. **브라우저**의 앞으로 가기, 뒤로 가기가 양방향 연결리스트로 구현됨
6. 시간 복잡도  
   삽입: O(1)  
   삭제: O(1)  
   검색: O(N)  
   접근: O(N)

## 구현 메서드

- `push(value)`: 링크드 리스트 맨 뒤에 `value` 값을 가지는 노드 추가
- `pop()`: 링크드 리스트의 맨 뒤 노드 삭제
- `shift()`: 링크드 리스트의 맨 앞 노드 삭제
- `unshift(value)`: 링크드 리스트의 맨 앞에 `value` 값을 가지는 노드 추가
- `get(index)`: `index`번째 순서에 있는 노드 반환
- `set(value, index)`: `index`번째 순서에 있는 노드 변경
- `insert(value, index)`: `index`번째 순서에 있는 노드의 `value` 변경
- `remove(index)`: `index`번째 순서에 있는 노드 삭제
- `toArray()`: 링크드 리스트의 값을 요소로 가지는 배열 반환

## Implementation

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }

  push(value) {
    const newNode = new Node(value);
    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      this.tail.next = newNode;
      newNode.prev = this.tail;
      this.tail = newNode;
    }
    this.length++;
    return this;
  }

  pop() {
    if (!this.head) return undefined;

    const poppedNode = this.tail;
    this.tail = poppedNode.prev;
    poppedNode.prev = null;
    this.tail.next = null;
    this.length--;

    if (this.length === 0) this.head = null;
    return poppedNode;
  }

  shift() {
    if (!this.head) return undefined;

    const shiftedNode = this.head;
    this.head = shiftedNode.next;
    shiftedNode.next = null;
    this.head.prev = null;
    this.length--;

    if (this.length === 0) this.tail = null;
    return shiftedNode;
  }

  unshift(value) {
    const newNode = new Node(value);

    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.next = this.head;
      this.head.prev = newNode;
      this.head = newNode;
    }
    this.length++;
    return this;
  }

  get(index) {
    if (index < 0 || index >= this.length) return null;

    let getNode;
    let i;
    let leftSearchStart = true;
    if (this.length / 2 < index) leftSearchStart = false;

    if (leftSearchStart) {
      getNode = this.head;
      i = index;
      while (i--) getNode = getNode.next;
    } else {
      getNode = this.tail;
      i = this.length - index;
      while (i--) getNode = getNode.prev;
    }
    return getNode;
  }

  set(value, index) {
    const updatedNode = this.get(index);
    if (updatedNode !== null) {
      updatedNode.value = value;
      return true;
    }
    return false;
  }

  insert(value, index) {
    if (index < 0 || index > this.length) return false;
    if (index === 0) return !!this.unshift(value);
    if (index === this.length) return !!this.push(value);

    const newNode = new Node(value);
    const prevNode = this.get(index - 1);
    const nextNode = prevNode.next;

    newNode.next = nextNode;
    newNode.prev = prevNode;
    prevNode.next = newNode;
    nextNode.prev = newNode;
    this.length++;
    return true;
  }

  remove(index) {
    if (index < 0 || index >= this.length) return undefined;
    if (index === 0) return this.unshift();
    if (index === this.length - 1) return this.pop();

    const removedNode = this.get(index);
    const prevNode = removedNode.prev;
    const nextNode = removedNode.next;
    prevNode.next = removedNode.next;
    nextNode.prev = removedNode.prev;
    this.length--;
    return removedNode;
  }

  toArray() {
    const array = [];
    let current = this.head;
    for (let i = 0; i < this.length; i++) {
      array.push(current.value);
      current = current.next;
    }
    return array;
  }
}
```
