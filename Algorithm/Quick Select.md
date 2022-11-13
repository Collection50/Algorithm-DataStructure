# Quick Select

#알고리즘

## 개념

- 배열에서 `k`번째로 크거나 작은 숫자를 빠르게 찾는 알고리즘
- 보통 `k`번쨰로 크거나 작은 수를 찾을 때는 **정렬**을 이용한 후 찾기 마련인데, 이는 평균적으로 **O(NlogN)**이 소요되고 최악의 경우 **O(N^2)**이 소요된다
- **Divide & Conquer**를 이용한 **_Quick Select_**를 사용하여 **더 빠르게** 해결할 수 있다

## 특징

1. 퀵 정렬 응용 (pivot 사용)
2. 시간 복잡도 **O(N)**

## 구현

1. 배열의 요소 하나를 랜덤하게 설정한다 (`pivot` 설정)
2. `pivot`을 기준으로 작은 값은 `smaller`에, 큰 값은 `bigger`에 `push`
3. `bigger`의 크기에 따라 아래 3가지 경우로 나뉨

- `biger.length >= k` : `k`번째로 큰 요소는 `bigger` 안에 존재하므로 작게 분리하여 다시 재귀 호출
- `biger.length + 1 === k` : `biger` 안에 `k - 1`개의 요소가 존재하므로, `k`번째로 큰 요소는 `pivot`이다
- `biger.length + 1 < k` : `k`번째 요소가 `smaller`에 존재하므로,  
  다시 재귀 함수를 호출하며 두번째 인수는 `k - bigger.length - 1`이다  
  왜냐하면, `bigger`의 개수 + `pivot`을 제외하고 순서를 정해야 하기 때문이다

### 구현 메서드

- `getRamdom(min, max)`: 두 인수 사이의 정수를 랜덤하게 반환한다 (`max` 제외)
- `quickSelect(nums, k)`: `nums` 배열에서 `k`번째로 큰 수를 반환한다

## Implementation (k번째로 큰 수 찾기)

```js
const getRandom = (min, max) => min + Math.floor(Math.random() * (max - min));

const quickSelect = (nums, k) => {
  const pivotIndex = getRandom(0, nums.length);

  const bigger = [];
  const smaller = [];

  for (const [index, num] of nums.entries()) {
    if (index === pivotIndex) continue;

    if (num > nums[pivotIndex]) bigger.push(num);
    else smaller.push(num);
  }

  if (bigger.length >= k) {
    return quickSelect(bigger, k);
  }
  if (bigger.length + 1 === k) {
    return nums[pivotIndex];
  }
  return quickSelect(smaller, k - bigger.length - 1);
};
```

## Implementation (k번째로 작은 수 찾기)

- 다른 점은 모두 동일하나, 재귀 함수를 호출할 때 `pivot`보다 작은 값들이 존재하는 `smaller`가 기준이 된다

```js
const getRandom = (min, max) => min + Math.floor(Math.random() * (max - min));

const quickSelect = (nums, k) => {
  const pivotIndex = getRandom(0, nums.length);

  const bigger = [];
  const smaller = [];

  for (const [index, num] of nums.entries()) {
    if (index === pivotIndex) continue;

    if (num > nums[pivotIndex]) bigger.push(num);
    else smaller.push(num);
  }

  if (smaller.length >= k) {
    return quickSelect(smaller, k);
  }
  if (smaller.length + 1 === k) {
    return nums[pivotIndex];
  }
  return quickSelect(bigger, k - smaller.length - 1);
};
```
