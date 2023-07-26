## 풀이 날짜
2023-04-26

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/155652

## 초기 풀이
1. 모든 알파벳을 나열한 문자열을 정의하고, `skip` 문자열에 들어간 문자는 제외한다.
2. 다음 인덱스의 문자열을 찾을 때 배열에서 해당 부분이 `undefined` 가 되지 않게끔 내용을 충분히 덧붙인다.
3. `s` 의 문자를 순회하면서 결과 문자열을 만들어낸다.

### 소스코드
```js
function solution(s, skip, index) {
  var answer = '';
  let alphabets = 'abcdefghijklmnopqrstuvwxyz';
  skip.split('').forEach(c => (alphabets = alphabets.replace(c, '')));
  alphabets = alphabets.concat(alphabets).concat(alphabets);

  const dict = alphabets.split('').map(e => e);
  s.split('').forEach((c, i) => {
    answer = answer.concat(dict[dict.findIndex(d => d === c) + index]);
  });

  return answer;
}
```

## 리팩토링한 풀이
정답을 제출하고 확인할 수 있는 다른 분의 풀이를 참고하니 **같은 알파벳 내용을 뒤에 덧붙이는 과정**을 생략하고 `%` 연산자를 사용해서 <b style="color: red">**배열의 범위를 초과한 경우에는 다시 배열의 첫 부분으로 돌아가도록**</b> 구현할 수 있었다.

### 소스코드
```js
function solution(s, skip, index) {
  var answer = '';
  const dict = 'abcdefghijklmnopqrstuvwxyz'.split('').filter(c => !skip.includes(c));

  s.split('').forEach((c, i) => {
    const nextIdx = (dict.indexOf(c) + index) % dict.length;
    answer = answer.concat(dict[nextIdx]);
  });

  return answer;
}
```
