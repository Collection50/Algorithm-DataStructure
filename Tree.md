# Tree

#자료구조#트리

## 개념

- **나무가 가지를 치듯** 하위 노드가 생성되는 구조
- 링크드 리스트보다 상위 단계이며 더 복잡
- **_노드 + 간선_** 으로 이뤄져있음 (Node + Edge)
- 노드 간 **_계층적인 구조_** 로 이뤄져있음(부모, 자식)
- 리스트 === 선형 자료구조 (linear)
- 트리 === 비선형 자료구조 (포인터, non-linear)
- 하지만 단일 연결 리스트 == 트리의 종류로 볼 수 있음(like 사향 트리(skew tree))

## 특징

1. 자식 노드만 가리킬 수 있음 (단방향)
2. 오직 1개의 루트 노드 존재
3. 순회 시 DFS, BFS 방법 존재, DFS: Stack 사용, BFS: Queue 사용

## 트리의 사용

- HTML DOM
- Network Routing
- Abstract Syntax Tree
- Artificial Intelligence
- Folders in Operating System
- JSON

## 이진 탐색 트리 (Binary Search Tree)

- 이진 트리의 한 종류
- **_'탐색'_** 특화 (비교가 가능한 데이터들에 한해 사용 가능)
- 부모 노드 기준으로
- (왼쪽 자노드 < 부모 노드)
- (오른쪽 자노드 > 부모 노드)
- 시간 복잡도
- 삽입: O(logN) (평균)
- 탐색: O(logN) (평균)

## 구현 메서드 (Binary Search Tree)

- `insert(value)`: 노드 추가
- `search(value)`: 노드 탐색
- `remove(value)`: 노드 삭제
- `BFS()`: 너비를 우선하여 노드 탐색
- `DFS` 종류 (깊이 우선 탐색)
- `preorder()`: 전위 순회(root node 먼저 탐색)
- `postorder()`: 후위 순회(맨 마지막에 root node 탐색)
- `inorder()`: 중위 순회(중간에 root node 탐색)

## Implementation 1

```js
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  insert(value) {
    const newNode = new Node(value);
    if (this.root === null) {
      this.root = newNode;
      return this;
    } else {
      let current = this.root;
      while (true) {
        if (value === current.value) return undefined;
        if (current.value < value) {
          if (current.right === null) {
            current.right = newNode;
            return this;
          } else {
            current = current.right;
          }
        } else if (current.value > value) {
          if (current.left === null) {
            current.left = newNode;
            return this;
          } else {
            current = current.left;
          }
        }
      }
    }
  }

  search(value) {
    if (this.root === null) return false;
    let current = this.root;
    let found = false;
    while (current && !found) {
      if (value < current.value) {
        current = current.left;
      } else if (value > current.value) {
        current = current.right;
      } else {
        found = true;
      }
    }
    if (!found) return undefined;
    return current;
  }
}
```

## Refactoring

`insert(value)`, `search(value)` 리팩토링  
`remove(value)`, `BFS()`, `preorder()`, `postorder()`, `inorder()` 구현

```js
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  insert(value) {
    const newNode = new Node(value);

    if (!this.root) this.root = newNode;
    else {
      let current = this.root;

      while (current.value !== value) {
        if (current.value < value) {
          if (!current.right) {
            current.right = newNode;
          }
          current = current.right;
        } else {
          if (!current.left) {
            current.left = newNode;
          }
          current = current.left;
        }
      }
    }

    return this;
  }

  search(value) {
    let current = this.root ? this.root : null;

    while (current) {
      if (current.value < value) {
        current = current.right;
      } else if (current.value > value) {
        current = current.left;
      } else {
        return current;
      }
    }
    return null;
  }

  remove(value) {
    this.root = this.removeNode(this.root, value);
  }

  removeNode(node, value) {
    if (!node) return null;

    if (value === node.value) {
      if (!node.left && !node.right) return null;
      if (!node.left) return node.right;
      if (!node.right) return node.left;

      const minNode = this.getMinNode(node.right);
      node.value = minNode.value;
      node.right = this.removeNode(node.right, minNode.value);
    }
    if (value < node.value) {
      node.left = this.removeNode(node.left, value);
    } else {
      node.right = this.removeNode(node.right, value);
    }

    return node;
  }

  getMinNode(node) {
    let current = node;

    while (current.left) {
      current = current.left;
    }
    return current;
  }

  BFS() {
    let node = this.root;
    if (!node) return null;

    const queue = [];
    const visited = [];
    queue.push(node);

    while (queue.length) {
      node = queue.shift();
      visited.push(node.value);
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    return visited;
  }

  preorder() {
    const visited = [];
    let current = this.root;
    const traverse = function (node) {
      visited.push(node.value);
      if (node.left) traverse(node.left);
      if (node.right) traverse(node.right);
    };
    traverse(current);
    return visited;
  }

  postorder() {
    const visited = [];
    let current = this.root;

    const traverse = function (node) {
      if (node.left) traverse(node.left);
      if (node.right) traverse(node.right);
      visited.push(node.value);
    };
    traverse(current);
    return visited;
  }

  inorder() {
    const visited = [];
    const current = this.root;

    const traverse = function (node) {
      if (node.left) traverse(node.left);
      visited.push(node.value);
      if (node.right) traverse(node.right);
    };

    traverse(current);
    return visited;
  }
}
```
