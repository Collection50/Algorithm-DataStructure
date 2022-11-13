# Permutation

#알고리즘#순열#permutation

## 개념

- `N`개의 수 중 `R`개를 뽑는 경우의 수
- 순서 상관 있음
- 즉 순서가 다르면 다른 것으로 취급 [순열과 다름](https://github.com/Collection50/Algorithm-DataStructure/blob/master/Combination.md)
- 이항계수와 동일한 개념

## 특징

- `nPr` === `n`개 원소의 집합에서 `r`개를

## Explanation

1. `getPermutation`: 조합 배열 `nums`, 원하는 개수 `pickCount`를 매개변수로 받는다
2. `permute`: `nums`를 순회하며 방문처리를 위해 `fixed`에 추가하고 `permute` 재귀 호출
3. `fixed.size === pickCount`라면 해당 값들을 `result`에 추가
4. 재귀 호출 종료 시 `fixed.delete`를 해주어 원상복귀

## Implementation

```js
const getPermutation = function (nums, pickCount) {
  const result = [];

  const permute = (fixed) => {
    if (fixed.size === pickCount) {
      result.push([...fixed]);
      return;
    }

    for (let i = 0; i < nums.length; i++) {
      const value = nums[i];

      if (!fixed.has(value)) {
        fixed.add(value);
        permute(fixed);
        fixed.delete(value);
      }
    }
  };
  permute(new Set());
  return result;
};

console.log(getPermutation([1, 2, 3, 4, 5], 3));
```

## 배운 점

`permute` 호출 시 매개변수로 배열을 전달할 수도 있었지만  
시간 복잡도를 줄이고자 해시를 활용하였다(`Set`)

PS.  
순열과 조합 모두 커밋한 줄 알고 있었는데  
순열이 안 되어 있었다  
꼼꼼히
