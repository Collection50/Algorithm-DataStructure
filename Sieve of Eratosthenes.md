# 에라토스테네스의 체(Sieve of Eratosthenes)

#알고리즘#소수

## 개념

- **자연수 N 이하에 존재하는 소수를 가장 빠르게** 찾는 알고리즘
- 체를 사용하듯 수를 걸러낸다고 하여 에라토스테네스의 체라고 부름

## 특징

- 일정 범위 이내 소수를 찾는 알고리즘 중 **에라토스테네스의 체보다 빠른 알고리즘 존재하지 않음**
- 이는 소수 간의 규칙이 존재하지 않기 때문

## Explantation

1. `1 ~ N`까지의 숫자와 각 `index`를 사용해야 하므로, `N + 1`개의 배열을 생성한다
2. 0은 사용할 수 없고, 소수도 합성수도 아닌 유일한 자연수이므로 각 0과 1에 `false` 할당
3. `2 ~ N`까지의 수를 순회
4. 순회 값을 제외한 순회 값의 배수들을 모두 제거 (`false` 할당)
5. 어떤 수의 배수로 존재한다는 것은 이미 소수가 아니므로 소수일 수 없음
6. `nums` 순회하며 소수들만 모아 출력
7. `j = i * i`를 초기값으로 전달함으로써 **최적화** 수행

## Implementation

```js
const getPrimeNumber = (N) => {
  const nums = Array.from({ length: N + 1 }, () => true).fill(false, 0, 2);

  for (let i = 2; i <= N; i++) {
    for (let j = i * i; j <= N; j += i) {
      nums[j] = false;
    }
  }

  return nums.reduce((acc, cur, index) => (cur ? [...acc, index] : acc), []);
};
```
