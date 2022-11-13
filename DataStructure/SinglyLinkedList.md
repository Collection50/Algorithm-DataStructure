# SinglyLinkedList

#자료구조#단방향연결리스트

## 개념

- 링크의 리스트(목록)
- 데이터 + 다음 노드의 주소를 가진 노드들을 **늘여놓은 형태**의 자료구조
- 따라서 다음 노드를 가리키는 **포인터**를 사용하여 구현

## 특징

1. `Array`와 비교하여, 데이터 추가 및 삭제 시 데이터의 이동이 적으나 `Random Access` 불가
2. 인덱스 없음, 오직 **포인터**를 사용해 이동
3. 다음 노드의 **주소를 통해서만** 다른 노드로 접근 가능
4. 시간 복잡도  
   삽입 : O(1)  
   삭제 : O(1) ~ O(N)  
   탐색 : O(N)  
   접근 : O(N)

## 구현 메서드

- `push(value)`: 링크드 리스트 맨 뒤에 `value` 값을 가지는 노드 추가
- `pop()`: 링크드 리스트의 맨 뒤 노드 삭제
- `shift()`: 링크드 리스트의 맨 앞 노드 삭제
- `unshift(value)`: 링크드 리스트의 맨 앞에 `value` 값을 가지는 노드 추가
- `get(index)`: `index`번째 순서에 있는 노드 반환
- `set(value, index)`: `index`번째 순서에 있는 노드 변경
- `insert(value, index)`: `index`번째 순서에 있는 노드의 `value` 변경
- `remove(index)`: `index`번째 순서에 있는 노드 삭제
- `reverse()`: 링크드 리스트의 순서 반전

## Implementation

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }

  push(value) {
    const newNode = new Node(value);
    if (!this.head) {
      this.head = newNode;
      this.tail = this.head;
    } else {
      this.tail.next = newNode;
      this.tail = newNode;
    }
    this.length++;
    return this;
  }

  pop() {
    if (!this.head) return undefined;

    let current = this.head;
    let newTail = this.tail;

    while (current.next) {
      newTail = current;
      current = current.next;
    }
    this.tail = newTail;
    this.tail.next = null;
    this.length--;

    if (this.length === 0) {
      this.head = null;
      this.tail = null;
    }
    return current;
  }

  shift() {
    if (!this.head) return undefined;

    const shiftHead = this.head;
    this.head = shiftHead.next;
    this.length--;

    if (this.length === 0) this.tail = null;
    return shiftHead;
  }

  unshift(value) {
    const newNode = new Node(value);
    if (!this.head) {
      this.head = newNode;
      this.tail = this.head;
    } else {
      newNode.next = this.head;
      this.head = newNode;
    }
    this.length++;
    return this;
  }

  get(index) {
    if (index >= this.length || index < 0) return null;

    let i = index;
    let current = this.head;
    while (i--) current = current.next;
    return current;
  }

  set(value, index) {
    const setNode = this.get(index);
    if (setNode) {
      setNode.value = value;
      return true;
    }
    return false;
  }

  insert(value, index) {
    if (index < 0 || this.length < index) return false;
    if (index === this.length) return !!this.push(value);
    if (index === 0) return !!this.unshift(value);

    const newNode = new Node(value);
    const preNode = this.get(index - 1);
    newNode.next = preNode.next;
    preNode.next = newNode;
    this.length++;
    return true;
  }

  remove(index) {
    if (index < 0 || this.length < index) return undefined;
    if (index === 0) return this.shift();
    if (index === this.length - 1) return this.pop();

    const preNode = this.get(index - 1);
    const removedNode = preNode.next;
    preNode.next = removedNode.next;
    this.length--;
    return removedNode;
  }

  reverse() {
    let tempNode = this.head;
    this.head = this.tail;
    this.tail = tempNode;
    let post;
    let pre = null;

    for (let i = 0; i < this.length; i++) {
      post = tempNode.next;
      tempNode.next = pre;
      pre = tempNode;
      tempNode = post;
    }
    return this;
  }

  toArray() {
    const array = [];
    let current = this.head;

    while (current) {
      array.push(current.value);
      current = current.next;
    }
    return array;
  }
}
```
