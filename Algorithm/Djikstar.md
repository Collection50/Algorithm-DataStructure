# Djikstar Algorithm

#알고리즘#다익스트라#최단경로

## 개념

- 음의 가중치가 없는 그래프 두 정점 사이에 존재하는 최단 경로 찾는 알고리즘
- 단순히 두 정점 사이의 최단 경로뿐만 아니라 특정 정점에서 모든 정점으로 가는 최단거리를 구할 수 있음
- 선수 학습: [그래프](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Graph.md), [우선순위 큐](https://github.com/Collection50/Algorithm-DataStructrue/blob/master/Priority%20Queue.md)

## 특징

1. 우선순위 큐, 그래프를 사용하여 두 정점 사이에 존재하는 최단 경로 탐색 알고리즘
2. 최단 거리를 찾아내는 문제에 많이 사용되며, 다익스트라를 확장시킨 알고리즘이 **A\*** 알고리즘이다
3. **시간 복잡도 O(ElogV)**

## 구현 메서드

- `Djikstra(start, end)`: 시작(start)부터 끝(end)까지의 최단 경로 반환
- 나머지 설명은 위 그래프, 우선순위 큐 링크 참조

## Implementation

```js
class WeightedGraph {
  constructor() {
    this.adjacencyList = {};
  }

  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) {
      this.adjacencyList[vertex] = [];
    }
  }

  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ node: vertex2, weight });
    this.adjacencyList[vertex2].push({ node: vertex1, weight });
  }

  Dijkstra(start, end) {
    const queue = new PriorityQueue();
    const distances = {};
    const previous = {}; // 모든 노드 값들로 가는 최단 경로 저장
    const path = []; // to return at end
    let smallest;

    // 시작 노드 거리를 0으로 초기화
    // 나머지 노드 거리는 Infinity로 초기화
    for (const vertex in this.adjacencyList) {
      if (vertex === start) {
        distances[vertex] = 0;
        queue.enqueue(vertex, 0);
      } else {
        distances[vertex] = Infinity;
        queue.enqueue(vertex, Infinity);
      }

      // 탐색 시작 X, 모든 노드에 대한 방문값 null
      previous[vertex] = null;
    }

    // 방문할 노드가 존재하는 한 반복
    while (queue.values.length) {
      // 우선순위 큐에 저장되므로
      // 우선 순위가 가장 낮은 정점 dequeue
      smallest = queue.dequeue().value;

      if (smallest === end) {
        // 만약 가장 낮은 정점 === end라면 종료

        // 종료 조건이 만족되었으므로, 끝에서부터 previous값 push (경로 저장)
        while (previous[smallest]) {
          path.push(smallest);
          smallest = previous[smallest];
        }
        break;
      }

      if (smallest || distances[smallest] !== Infinity) {
        // 정점이 가리키는 각 노드에 대해 반복
        for (const neighbor in this.adjacencyList[smallest]) {
          // 다음으로 탐색할 노드 찾기
          const nextNode = this.adjacencyList[smallest][neighbor];
          // 이웃 노드에 대한 새로운 거리 계산
          const candidate = distances[smallest] + nextNode.weight;
          const nextNeighbor = nextNode.node;

          // 계산된 거리가 입력된 거리보다 작다면 업데이트
          if (candidate < distances[nextNeighbor]) {
            // 거리 값 업데이트
            distances[nextNeighbor] = candidate;
            // 이전 노드 업데이트
            previous[nextNeighbor] = smallest;
            // 우선순위 큐에 추가
            queue.enqueue(nextNeighbor, candidate);
          }
        }
      }
    }
    // 저장되었던 경로는 역순으로 입력되었으므로 reverse 메서드 사용하여 순서 반전
    return [...path, smallest].reverse();
  }
}

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

  enqueue(val, priority) {
    const newNode = new Node(val, priority);

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
    const { length } = this.values;
    const element = this.values[0];

    while (true) {
      const leftChildIdx = 2 * index + 1;
      const rightChildIdx = 2 * index + 2;
      let leftChild;
      let rightChild;
      let swap = null;

      if (leftChildIdx < length) {
        leftChild = this.values[leftChildIdx];

        if (leftChild.priority < element.priority) {
          swap = leftChildIdx;
        }
      }
      if (rightChildIdx < length) {
        rightChild = this.values[rightChildIdx];

        if (
          (swap === null && rightChild.priority < element.priority) ||
          (swap !== null && rightChild.priority < leftChild.priority)
        ) {
          swap = rightChildIdx;
        }
      }
      if (swap === null) break;

      this.values[index] = this.values[swap];
      this.values[swap] = element;
      index = swap;
    }
  }
}
```
