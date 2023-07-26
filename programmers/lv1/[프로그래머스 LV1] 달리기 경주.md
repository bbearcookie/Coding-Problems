## 풀이 날짜
2023-04-27

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/178871

### 문제 설명
얀에서는 매년 달리기 경주가 열립니다. 해설진들은 선수들이 자기 바로 앞의 선수를 추월할 때 추월한 선수의 이름을 부릅니다. 예를 들어 1등부터 3등까지 "mumu", "soe", "poe" 선수들이 순서대로 달리고 있을 때, 해설진이 "soe"선수를 불렀다면 2등인 "soe" 선수가 1등인 "mumu" 선수를 추월했다는 것입니다. 즉 "soe" 선수가 1등, "mumu" 선수가 2등으로 바뀝니다.

선수들의 이름이 1등부터 현재 등수 순서대로 담긴 문자열 배열 players와 해설진이 부른 이름을 담은 문자열 배열 callings가 매개변수로 주어질 때, 경주가 끝났을 때 선수들의 이름을 1등부터 등수 순서대로 배열에 담아 return 하는 solution 함수를 완성해주세요.

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