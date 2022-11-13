# Graph

#자료구조#그래프#해시함수

## 개념

- 노드나 노드들의 연결을 모은 것
- 간선 == 노드와 노드 연결
- vertex(node): 정점(V)
- edge: 간선(E)
- [시간 복잡도](#시간-복잡도)

## 종류

- Undirected Graph (무방향 그래프)
- Directed Graph (방향 그래프)
- Unweighted Grpah (비가중 그래프)
- Weighted Graph (가중 그래프)

## 그래프 표현 방법

1. 인접 행렬 (Adjacency Matrix)
2. 인접 리스트 (Adjacency List)

### 왜 인접리스트로 그래프를 구현하나요?

- 그래프가 실제 생활에서 사용되는 경우,(SNS 등) 노드의 개수는 굉장히 많지만 모두가 연결되어 있지는 않음
- 따라서 더 적은 공간을 차지하는 인접 리스트 사용

## 그래프의 사용

- SNS
- route
- 최단 경로
- 추천 매커니즘
- [그래프 구조 시각화](https://musicmap.info/)

## 그래프 순회(탐색)

- DFS(깊이 우선 탐색)
- BFS(너비 우선 탐색)

## 구현 메서드

1. `addVertex(vertex)`: 정점 추가
2. `addEdge(vertex1, vertex2)`: 간선 추가
3. `removeVertex(vertex)`: 정점 삭제
4. `removeEdge(vertex1, vertex2)`: 간선 삭제
5. `DFSR(start)`: DFS 구현 with Recursion
6. `DFSI(start)`: DFS 구현 with Iteration
7. `DFSI(start)`: BFS 구현 with Iteration

## Implementation Code

```js
// Undirected Graph
class Graph {
  constructor() {
    this.adjacencyList = {};
  }

  addVertex(vertex) {
    if (this.adjacencyList[vertex]) return;
    this.adjacencyList[vertex] = [];
  }

  addEdge(vertex1, vertex2) {
    if (this.adjacencyList[vertex1]) {
      this.adjacencyList[vertex1].push(vertex2);
    }
    if (this.adjacencyList[vertex2]) {
      this.adjacencyList[vertex2].push(vertex1);
    }
  }

  removeVertex(vertex) {
    if (!this.adjacencyList[vertex]) return;

    while (this.adjacencyList[vertex].length) {
      const adjacentVertex = this.adjacencyList[vertex].pop();
      this.removeEdge(vertex, adjacentVertex);
    }
    delete this.adjacencyList[vertex];
  }

  removeEdge(vertex1, vertex2) {
    if (this.adjacencyList[vertex1]) {
      this.adjacencyList[vertex1] = this.adjacencyList[vertex1].filter(
        (v) => v !== vertex2
      );
    }
    if (this.adjacencyList[vertex1]) {
      this.adjacencyList[vertex2] = this.adjacencyList[vertex2].filter(
        (v) => v !== vertex1
      );
    }
  }

  DFSR(start) {
    const result = [];
    const visited = {};
    const adjacencyList = this.adjacencyList;

    (function dfs(vertex) {
      if (!vertex) return;
      visited[vertex] = true;
      result.push(vertex);
      adjacencyList[vertex].forEach((neighbor) => {
        if (!visited[neighbor]) {
          return dfs(neighbor);
        }
      });
    })(start);
    return result;
  }

  DFSI(start) {
    const stack = [start];
    const result = [];
    const visited = {};
    let currentVertex;

    visited[start] = true;
    while (stack.length) {
      currentVertex = stack.pop();
      result.push(currentVertex);

      this.adjacencyList[currentVertex].forEach((neighbor) => {
        if (!visited[neighbor]) {
          visited[neighbor] = true;
          stack.push(neighbor);
        }
      });
    }
    return result;
  }

  BFSI(start) {
    const queue = [start];
    const result = [];
    const visited = {};
    let currentVertex;

    visited[start] = true;
    while (queue.length) {
      currentVertex = queue.shift();
      result.push(currentVertex);
      this.adjacencyList[currentVertex].forEach((neighbor) => {
        if (!visited[neighbor]) {
          visited[neighbor] = true;
          queue.push(neighbor);
        }
      });
    }
    return result;
  }
}
```

## 시간 복잡도

1. 인접 행렬

- 정점 추가 O(|V^2|)
- 간선 추가 O(1)
- 정점 삭제 O(|V^2|)
- 간선 삭제 O(1)
- 쿼리 O(1)
- 저장 공간 O(|V^2|)
- 모든 간선 순회 속도 느림
- 특정 간선의 존재 여부 확인 빠름

2. 인접 리스트

- 정점 추가 O(1)
- 간선 추가 O(1)
- 정점 삭제 O(|V| + |E|)
- 간선 삭제 O(|E|)
- 쿼리 O(|V| + |E|)
- 저장 공간 O(|V| + |E|)
- 간선이 많지 않고 퍼져있는 그래프 == 인접 행렬보다 더 적은 공간 복잡도 차지
- 모든 간선 순회 속도 빠름
- 특정 간선의 존재 여부 확인 느림
