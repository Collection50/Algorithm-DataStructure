# Union Find

#자료구조#UnionFind

## 개념

- **두 개의 노드**가 **같은 그래프**에 속하는지 판별하는 알고리즘
- 대표적인 그래프 알고리즘이며 **서로소 집합(Disjoin-Set)**, **합집합 찾기**로도 불림
- 서로 다른 두 개의 노드를 병합하는 `union` 연산, 노드가 어떠한 그래프에 속해있는지 판단하는 `find` 연산으로 나뉨

## 특징

1. **트리** 이용하여 효율성 증대 [트리란?](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Tree.md)

## 구현 메서드

- `union(x, y)`: 두 개의 노드를 같은 부모 노드로 병합
- `find(x, y)`: 2개의 노드가 같은 그래프에 속했는지 가졌는지 확인하는 함수 (경로 압축 포함)

## Implementation 1

```js
class UnionFind {
  constructor(size) {
    this.parent = Array.from({ length: size }, (_, i) => i);
  }

  union(x, y) {
    const rortX = this.find(x);
    const rootY = this.find(y);

    if (rortX < rootY) {
      this.parent[rootY] = rortX;
    } else {
      this.parent[rortX] = rootY;
    }
  }

  find(x) {
    if (this.parent[x] === x) {
      return x;
    }
    return this.find(this.parent[x]);
  }
}
```

## Optimization

- `unionByRank(x, y)`: `union(x, y)`과 동일하지만, 트리 높이 최적화
- 최적화 && 경로 압축을 통해 시간 복잡도 단축
- 시간 복잡도 **O(a(N))** 으로, 상수 시간과 동일한 수준으로 빠름

## Implementation 2

```js
class UnionFind {
  constructor(size) {
    this.parent = Array.from({ length: size }, (_, i) => i);
    this.rank = Array.from({ length: size }, () => 1);
  }

  find(x) {
    if (this.parent[x] === x) {
      return x;
    }
    return this.find(this.parent[x]);
  }

  unionByRank(x, y) {
    const rootX = this.find(x);
    const rootY = this.find(y);

    if (rootX === rootY) {
      return;
    }

    if (this.rank[rootX] > this.rank[rootY]) {
      this.parent[rootY] = rootX;
    } else if (this.rank[rootX] === this.rank[rootY]) {
      this.parent[rootX] = rootY;
    } else {
      ++this.rank[rootX];
    }
  }
}
```
