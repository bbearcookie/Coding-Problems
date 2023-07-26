## 풀이 날짜
2023-04-27

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/178871

## 첫 번째 시도
`callings` 의 요소를 순회하면서 해당 이름의 선수와, 그 선수보다 한 칸 앞에 있는 선수를 배열에서 서로 위치를 스왑하는 방식으로 구현했는데, 시간 초과가 발생했다.

### 소스코드
```js
function solution(players, callings) {
  callings.forEach(called => {
    const index = players.indexOf(called);
    [players[index - 1], players[index]] = [players[index], players[index - 1]];
  });

  return players;
}
```

## 성공한 풀이
특정 선수가 몇 등인지를 `O(1)` 의 속도로 참조할 수 있는 `indexes` 와, 특정 순위위 선수가 누구인지를 `O(1)` 의 속도로 참조할 수 있는 두 `Map` 자료구조를 만들었다.  

그리고 경주 과정을 진행하면서 순위 정보를 업데이트하고 마지막에 배열 형태로 결과를 반환했다.

### 소스코드
```js
function solution(players, callings) {
  const indexes = new Map();
  const datas = new Map();

  // 초기 값 설정
  players.forEach((p, i) => {
    indexes.set(p, i);
    datas.set(i, p);
  });

  // 경주 과정 진행
  callings.forEach(me => {
    const index = indexes.get(me);
    const faster = datas.get(index - 1);

    indexes.set(faster, index);
    indexes.set(me, index - 1);

    datas.set(index - 1, me);
    datas.set(index, faster);
  });

  return Array.from(datas.values());
}
```