## 풀이 날짜
2023-04-28

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/172928

## 아이디어
1. 강아지가 존재하는 초기 위치를 `y`와 `x`에 저장한다.
2. 각 이동 명령을 수행하는데, 공원 범위를 벗어나거나 장애물을 만나는 경우가 아니라면 명령을 수행한 결과 위치를 `y`와 `x`에 업데이트 한다.

## 소스코드
```js
function isInBoundary(y, x, park) {
  if (y < 0 || y >= park.length) return false;
  if (x < 0 || x >= park[0].length) return false;
  return true;
}

function move(y, x, route, park) {
  let [direction, amount] = route.split(' ');
  let dirs = { E: [0, 1], S: [1, 0], W: [0, -1], N: [-1, 0] };
  amount = Number(amount);

  for (let i = 0; i < amount; i++) {
    y += dirs[direction][0];
    x += dirs[direction][1];

    if (!isInBoundary(y, x, park) || park[y][x] === 'X') return null;
  }

  return [y, x];
}

function solution(park, routes) {
  let y;
  let x;

  // 강아지 초기 위치 설정
  for (let i = 0; i < park.length; i++) {
    for (let j = 0; j < park[i].length; j++) {
      if (park[i][j] === 'S') {
        y = i;
        x = j;
        break;
      }
    }
    if (y && x) break;
  }

  // 강아지 이동 명령 수행
  routes.forEach(route => {
    const result = move(y, x, route, park);
    if (result) [y, x] = result;
  });

  return [y, x];
}
```
