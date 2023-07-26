## 풀이 날짜
2023-07-17

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/17677

## 고민 포인트
문제를 이해하는 것에서 살짝 막혔는데, 교집합과 합집합을 어떻게 원소의 중복을 허락해서 만든다는 것인지를 이해하는데 시간이 좀 걸렸다.  

## 아이디어
객체를 해시 테이블처럼 사용해서 풀었다.  

1. 먼저 두 문자열 입력 데이터를 두 글자씩 추출하면서 아래 조건을 따라서 배열 형태로 가공한다: 
    1. 대소문자의 구분이 없으므로, 대문자로 변환한다.
    2. 영문자가 아닌 문자가 포함되었다면 그 글자 쌍은 버린다.
2. 배열을 순회하면서 중복되는 요소는 카운팅하여 집합을 나타내는 객체를 만든다.
3. 아래 알고리즘에 따라 교집합을 만든다:
    1. `set1` 객체를 순회하면서, `set2` 객체에도 포함된 요소인 경우 **중복되는 최소한의 갯수**를 구한다.
4. 아래 알고리즘에 따라 합집합을 만든다:
    1. `set1` 객체를 순회하면서, 각 요소의 **중복되는 최대한의 갯수**를 구한다.
    2. `set2` 객체를 순회하면서, 각 요소의 **중복되는 최대한의 갯수**를 구한다.
5. `Math.floor((교집합 원소의 갯수) / (합집합 원소의 갯수) * 65536)` 을 반환한다.


## 소스코드

### 리팩토링 전
```js
const makeArr = (str) => {
  return str
    .split('')
    .filter((_, idx) => idx !== 0)
    .map((_, idx) => (str[idx] + str[idx + 1]).toUpperCase())
    .filter((item) => /[^A-Za-z]/.test(item) === false);
};

const makeSet = (str) => {
  const set = {};

  makeArr(str).forEach((item) => {
    if (set[item]) set[item] = set[item] + 1;
    else set[item] = 1;
  });

  return set;
};

const makeIntersection = (set1, set2) => {
  const result = {};

  Object.keys(set1).forEach((key) => {
    if (set2[key]) result[key] = Math.min(set1[key], set2[key]);
  });

  return result;
};

const makeUnion = (set1, set2) => {
  const result = {};

  Object.keys(set1).forEach((key) => {
    if (set2[key]) result[key] = Math.max(set1[key], set2[key]);
    else result[key] = set1[key];
  });

  Object.keys(set2).forEach((key) => {
    if (result[key]) result[key] = Math.max(result[key], set2[key]);
    else result[key] = set2[key];
  });

  return result;
};

const getLength = (set) => {
  return Object.values(set).reduce((acc, value) => acc + value, 0);
};

function solution(str1, str2) {
  const set1 = makeSet(str1);
  const set2 = makeSet(str2);

  const intersections = makeIntersection(set1, set2);
  const unions = makeUnion(set1, set2);
  const result = Math.floor((getLength(intersections) / getLength(unions)) * 65536);

  return isNaN(result) ? 65536 : result;
}
```

### 리팩토링 후
```js
const makeArr = (str) =>
  str
    .split('')
    .filter((_, idx) => idx !== 0)
    .map((_, idx) => (str[idx] + str[idx + 1]).toUpperCase())
    .filter((item) => /[^A-Za-z]/.test(item) === false);

const makeSet = (str) => makeArr(str).reduce((result, item) => ({ ...result, [item]: result[item] + 1 || 1 }), {});

const makeIntersection = (set1, set2) =>
  Object.keys(set1)
    .filter((key) => set2[key])
    .reduce((result, key) => ({ ...result, [key]: Math.min(set1[key], set2[key]) }), {});

const makeUnion = (set1, set2) => {
  const union = [...new Set([...Object.keys(set1), ...Object.keys(set2)])];
  return union.reduce((result, key) => ({ ...result, [key]: Math.max(set1[key] || 0, set2[key] || 0) }), {});
};

const getLength = (set) => Object.values(set).reduce((acc, value) => acc + value, 0);

function solution(str1, str2) {
  const set1 = makeSet(str1);
  const set2 = makeSet(str2);

  const intersections = makeIntersection(set1, set2);
  const unions = makeUnion(set1, set2);
  const result = Math.floor((getLength(intersections) / getLength(unions)) * 65536);

  return isNaN(result) ? 65536 : result;
}
```
