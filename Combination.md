# Combination

#알고리즘#조합#Combination

## 개념

- `N`개의 수 중 `R`개를 뽑는 경우의 수
- 순서 상관 없음
- 즉, 순서가 달라도 구성요소가 같다면 같은 것으로 취급 [순열과 다름]()
- 이항계수와 동일한 개념

## 특징

- `nCr`의 경우 n개 원소의 집합에서 r개를 뽑는 것을 의미한다
- `nCr` === `n! / r! \* (n - r)!`
- `nCr` === `nC(n - r)`

## Explanation

1. `getCombinations(nums, pickCount)` 함수는 정수 배열과 조합 개수를 전달하여 조합을 얻는다
2. `combination` 함수는 빈 배열과 `0`을 매개변수로 전달받으며 처음 호출됨
3. `current.length`가 `pickCount`와 동일하다면 목표 개수를 모두 넣은 것이므로 해당 조합을 `result`에 `push`
4. 아니라면, `start`부터 1개씩 선택하여, 재귀 호출되는 `combination` 함수의 인수로 전달한다.
5. 이 때 `i + 1`을 전달하게 되는데, 이는 중복된 것이 선택되지 않도록 한다
6. `for`문의 초기값을 `start`로 지정함으로써, 순열을 구할 수 있게 된다

주의점  
`combination`에 인수를 전달할 때, `push`와 같은 부수 효과가 발생하는 함수를 사용하여도 구현할 수 있다  
하지만 이때는 배열에 `push`했던 값을 다시 `pop`하여야 정상적으로 동작한다  
ES6부터 스프레드 문법을 지원하므로, 스프레드 문법 사용을 권장한다

## Implementation

```js
const getCombinations = (nums, pickCount) => {
  const result = [];

  const combination = (current, start) => {
    if (current.length === pickCount) {
      result.push(current);
      return;
    }

    for (let i = start; i < nums.length; i++) {
      combination([...current, nums[i]], i + 1);
    }
  };

  combination([], 0);
  return result;
};

console.log(getCombination([1, 3, 4, 6, 2, 8], 3));
```

### push && pop을 활용한 풀이

```js
for (let i = start; i < nums.length; i++) {
  current.push(nums[i]);
  combination(current, i + 1);
  current.pop();
}
```

## 배운 점

스프레드 문법을 사용하는 것이  
조합을 구할 때, **가장 효율적이면서 가독성이 좋은** 방법이지 않을까 생각한다
