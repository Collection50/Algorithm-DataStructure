# HashTable

#자료구조#해시테이블#해시함수

## 개념

- 키(`key`)를 통해 값(`value`)에 접근
- 해시 함수: 해시 함수를 활용하여 임의 크기의 데이터 => 고정된 크기의 데이터로 **_변환_** (해싱)
- 해시 테이블: 해시 함수를 사용하여 변환한 키 값을 **_고유한_** `hashCode`로 삼아 키-값(pair) 저장을 위해 사용되는 자료구조
- 해시 충돌: 각 키는 고유한 `hashCode`를 사용해야 하지만, 2개 이상의 다른 키가 동일한 `hashCode`를 사용하는 경우

## 특징

1. 시간 복잡도 **_O(1)_** 로, 데이터 탐색, 삽입, 접근 가능
2. 데이터 구조 크기 == **_소수(PRIME NUMBER)_** 사용 ([WHY PRIME NUMBER?](https://www.quora.com/Does-making-array-size-a-prime-number-help-in-hash-table-implementation-Why))
3. 다양한 데이터를 저장할 수 있지만 아래에서는 **문자열** 해시 테이블 구현

### 좋은 해시 함수의 조건

1. 빠른 속도: 상수 시간, O(1)
2. 1:1 대응: 동일한 키 === 동일한 해시 값 생성
3. 균일성: 데이터 구조(배열) 전체를 최대한 활용

### 해시 충돌

- 2개 이상의 키의 해시 값이 **_동일_** 한 경우
- ex) `hashCode('pink')` === `hashCode('hello')`
- 이 경우 키를 통해 값에 접근 시, 2개 이상의 값이 존재 == 오류 발생

#### 해시 충돌 해결

1. 분리 연결법(Separate Chaining): 해시 충돌 == 데이터를 연결하는 방식(배열)
2. 개방 주소법(Open Addressing): 해시 충돌 == 비어있는 hash를 찾아 데이터 저장 (key, value의 1:1 대응 유지)
3. 키에 해당하는 hashCode(index)를 다양하게 구하기: `djb2`

## Implementations

1. **_Separate Chaining_**
2. **_doubleHashing_**
3. **_djb2_**

### 공통 구현 메서드

1. `set(key, value)`: 키, 값을 해시 테이블에 저장
2. `get(key)`: 키에 해당하는 값 반환
3. `keys()`: 해시 테이블의 모든 키 출력
4. `values()`: 해시 테이블의 모든 값 출력

## 분리 연결법 (Separate Chaining)

해시 충돌 발생 == 배열 내 배열을 생성하여 순서대로 저장  
**_2개 이상의 키가 같은 해시 값을 소유_** 하지만, **_배열 내 순서_** 를 통해 구분

### 구현 메서드

1. `hashCode(key)`:

- 키에 고유한 해시 값을 부여하는 해시 함수
- 최대 50글자 비교하여 저장

### Separate Chaining

```js
class HashTable {
  constructor(size = 53) {
    this.keyArr = new Array(size);
  }

  getHashCode(key) {
    let hash = 0;
    const PRIME_NUM = 31;

    for (let i = 0; i < Math.min(key.length, 50); i++) {
      const char = key[i];
      const value = char.charCodeAt(0);
      hash = hash * PRIME_NUM + value;
    }
    return hash % this.keyArr.length;
  }

  set(key, value) {
    const hashCode = this.getHashCode(key);
    if (!this.keyArr[hashCode]) this.keyArr[hashCode] = [];
    this.keyArr[hashCode].push([key, value]);
  }

  get(key) {
    const KEY = 0;
    const VALUE = 1;
    const hashCode = this.getHashCode(key);
    if (!this.keyArr[hashCode]) return undefined;

    for (let i = 0; i < this.keyArr[hashCode].length; i++) {
      if (this.keyArr[hashCode][i][KEY] === key)
        return this.keyArr[hashCode][i][VALUE];
    }
    return undefined;
  }

  keys() {
    const keySet = new Set();
    const KEY = 0;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      for (let j = 0; j < this.keyArr[i].length; j++) {
        keySet.add(this.keyArr[i][j][KEY]);
      }
    }
    return [...keySet];
  }

  values() {
    const valueSet = new Set();
    const VALUE = 1;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      for (let j = 0; j < this.keyArr[i].length; j++) {
        valueSet.add(this.keyArr[i][j][VALUE]);
      }
    }
    return [...valueSet];
  }
}
```

## doubleHashing (개방 주소법, Open Addressing)

- 이중 해싱
- 어떠한 키의 해시 값이 **_이미 존재_** == **_재해싱_** 하여 새로운 해시 값 부여
- 해싱 저장 방식에 따른 `set(key, value)`, `get(key)`, `keys()`, `values()` 메서드도 함께 변경

### 구현 메서드

1. `hashCode(key)`:

- 키에 고유한 해시 값을 부여하는 해시 함수
- 최대 50글자 비교하여 저장

2. `doubleHashing(key)`:

- 해시 충돌 방지를 위한 추가 함수
- 어떠한 키의 해시 값이 데이터 구조에 존재하지 않을 때까지 해싱

### doubleHashing

```js
class HashTable {
  constructor(size = 53) {
    this.keyArr = new Array(size);
  }

  getHashCode(key) {
    let hash = 0;
    const PRIME_NUM = 31;

    for (let i = 0; i < Math.min(key.length, 50); i++) {
      const char = key[i];
      const value = char.charCodeAt(0);
      hash *= PRIME_NUM + value;
    }
    return hash % this.keyArr.length;
  }

  doubleHashing(key) {
    const DOUBLE_HASH_SIZE = 47;
    return DOUBLE_HASH_SIZE - (this.getHashCode(key) % DOUBLE_HASH_SIZE);
  }

  set(key, value) {
    if (!this.keyArr.every((elem) => elem)) return;

    let hashCode = this.getHashCode(key);
    let keyValue = this.keyArr[hashCode];

    while (keyValue) {
      hashCode += this.doubleHashing(key);
      hashCode = hashCode > this.size ? hashCode - this.size : hashCode;
      keyValue = this.keyArr[hashCode];
    }
    this.keyArr[hashCode] = [key, value];
  }

  get(key) {
    if (this.keyArr.every((elem) => !elem)) return;

    const KEY = 0;
    let hashCode = this.getHashCode(key);
    let keyValue = this.keyArr[hashCode];

    while (keyValue) {
      if (keyValue[KEY] === key) return keyValue;
      hashCode += this.doubleHashing(key);
      hashCode = hashCode > this.size ? hashCode - this.size : hashCode;
      keyValue = this.keyArr[hashCode];
    }
    return undefined;
  }

  keys() {
    const keySet = new Set();
    const KEY = 0;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      keySet.add(this.keyArr[i][KEY]);
    }
    return [...keySet];
  }

  values() {
    const valueSet = new Set();
    const VALUE = 1;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      valueSet.add(this.keyArr[i][VALUE]);
    }
    return [...valueSet];
  }
}
```

### djb2

문자열 해싱에 있어서 성능이 굉장히 좋은 해시 함수이지만  
충돌이 일어났을 때를 감안하여 분리 연결법 방식으로 구현 (이중 배열)

### 구현 메서드

1. `djb2(key)`:

- 문자열에서 성능이 훌륭한 해시 함수
- 소수가 아닌 33을 사용하지만 훨씬 효율적 [WHY 33?](http://www.cse.yorku.ca/~oz/hash.html)

```js
class HashTable {
  constructor(size = 1013) {
    this.keyArr = new Array(size);
  }
  // getHashCode(key)
  djb2(key) {
    let hash = 5381;
    for (let i = 0; i < key.length; i++) {
      hash = hash * 33 + key.charCodeAt(i);
    }
    return hash % size;
  }

  set(key, value) {
    const hashCode = this.djb2(key);
    if (!this.keyArr[hashCode]) this.keyArr[hashCode] = [];
    this.keyArr[hashCode].push([key, value]);
  }

  get(key) {
    const KEY = 0;
    const VALUE = 1;
    const hashCode = this.djb2(key);
    if (!this.keyArr[hashCode]) return undefined;

    for (let i = 0; i < this.keyArr[hashCode].length; i++) {
      if (this.keyArr[hashCode][i][KEY] === key)
        return this.keyArr[hashCode][i][VALUE];
    }
    return undefined;
  }

  keys() {
    const keySet = new Set();
    const KEY = 0;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      for (let j = 0; j < this.keyArr[i].length; j++) {
        keySet.add(this.keyArr[i][j][KEY]);
      }
    }
    return [...keySet];
  }

  values() {
    const valueSet = new Set();
    const VALUE = 1;

    for (let i = 0; i < this.keyArr.length; i++) {
      if (!this.keyArr[i]) continue;
      for (let j = 0; j < this.keyArr[i].length; j++) {
        valueSet.add(this.keyArr[i][j][VALUE]);
      }
    }
    return [...valueSet];
  }
}
```

## Further more...

- 해시 충돌 해결 방법 중 [개방 주소법(Open Addressing)](http://wiki.hash.kr/index.php/%ED%95%B4%EC%8B%9C%EC%B6%A9%EB%8F%8C)은  
  **_이중 해싱_** 에 더하여 **_선형 탐사_**, **_제곱 탐사_** 등 존재
- 해시 충돌 == **_필연적_** 으로 나타날 수밖에 없음 (최소화에 목적)  
  [비둘기집 원리](https://ko.wikipedia.org/wiki/%EB%B9%84%EB%91%98%EA%B8%B0%EC%A7%91_%EC%9B%90%EB%A6%AC), [Birthday Paradox](https://ko.wikipedia.org/wiki/%EC%83%9D%EC%9D%BC_%EB%AC%B8%EC%A0%9C)에 의해 간접 증명
- 해시 == 암호학에서 굉장히 활발하게 사용되고 있으며,  
  2022.07.09 현재 **_SHA-512_**, **_SHA-258_** 알고리즘이 가장 강력
