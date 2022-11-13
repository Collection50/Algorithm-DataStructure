# GCD & LCM

#알고리즘#최소공배수#최대공약수

## 개념

- 최대공약수 : 두 수의 **공통 약수 중 가장 큰 것**
- 최소공배수 : 두 수의 **공통 배수 중 가장 작은 것**
- **유클리드 호제법**을 활용한 최소공배수 & 최대공약수 구하기
- 유클리드 호제법 : 두 양의 정수 or 두 다항식의 최대공약수를 구하는 방식

## Explanation

- 최대공약수 구하기 (GCD)
- 두 양의 정수 `a, b (a > b)`에 대해
- `a % b === r` 일 때 `GCD(a, b) === GCD(b, r)`이다
- 위 과정을 반복하며 `r === 0`이면 그때의 `b === a, b`의 최대공약수이다

- 최소공배수 구하기
- 유클리드 호제법을 통해 구한 최대공약수를 이용하여 구한다
- 두 양의 정수 `a, b (a > b)`에 대해 `LCM == Math.abs(a * b) / GCD(a, b)`이다

## GCD Implementation

```js
function GCD(a, b) {
  // a > b
  return b === 0 ? a : GCD(b, a % b);
}
```

## LCM Implementation

```js
function LCM(a, b) {
  // a > b
  return Math.floor((a * b) / GCD(a, b));
}
```
