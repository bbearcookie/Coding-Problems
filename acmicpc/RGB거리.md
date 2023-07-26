## 풀이 날짜
2023-02-14

## 문제
https://www.acmicpc.net/problem/1149

## 아이디어
1. 모든 집에 대해서 특정 색상으로 색칠했을 때 필요한 비용을 기억하는 2차원 배열을 정의한다.
2. 맨 첫번째 집은 그 집을 색칠하는데 드는 비용 자체가 곧 필요한 비용이 되므로, cost 배열에 그 값을 초기화한다.
3. 이후에 집을 순회하면서 각각 색칠하는데 드는 비용을 계산하고 기록한다.
> - i번째 집을 빨간색으로 색칠할 때 드는 비용은 (i-1번째 집을 초록or파랑으로 칠했을 때의 최소 값) + (i번째 집을 빨간색으로 칠하는 자체 비용) 이다.
> - i번째 집을 초록색으로 색칠할 때 드는 비용은 (i-1번째 집을 빨강or파랑으로 칠했을 때의 최소 값) + (i번째 집을 초록색으로 칠하는 자체 비용) 이다.
> - i번째 집을 파랑색으로 색칠할 때 드는 비용은 (i-1번째 집을 빨강or초록으로 칠했을 때의 최소 값) + (i번째 집을 파랑색으로 칠하는 자체 비용) 이다.
4. 가장 마지막 집을 빨강or초록or파랑으로 색칠하는 비용 중에서 가장 최소 값을 출력한다.

## 소스코드
```js
// 입력 처리
const INPUT_NAME = "i5.txt";
const filePath = process.platform === "linux" ? "/dev/stdin" : `./${__dirname.split('\\').pop()}/${INPUT_NAME}`;
const input = require("fs").readFileSync(filePath).toString().trim().split("\n").map(item => item.trim());
const n = Number(input[0]);
const data = [];
for (let i = 1; i <= n; i++) {
  const [r, g, b] = input[i].split(" ").map(e => Number(e));
  data.push([r, g, b]);
}

// 각 집을 특정 색상으로 색칠했을 때 드는 비용을 기억한다.
const cost = Array.from({ length: n }, () => Array.from({ length: 3 }, () => Number.MAX_SAFE_INTEGER));
cost[0][0] = data[0][0];
cost[0][1] = data[0][1];
cost[0][2] = data[0][2];

// 문제
function solution() {

  // 현재 집을 각각 빨강, 초록, 파랑으로 색칠했을 때 드는 비용을 계산하면서 기록한다.
  for (let i = 1; i < n; i++) {
    cost[i][0] = Math.min(cost[i-1][1], cost[i-1][2]) + data[i][0];
    cost[i][1] = Math.min(cost[i-1][0], cost[i-1][2]) + data[i][1];
    cost[i][2] = Math.min(cost[i-1][0], cost[i-1][1]) + data[i][2];
  }

  // 마지막 집을 빨강, 초록, 파랑중 하나로 색칠했을 때 가장 비용이 적게 되는 값을 출력한다.
  console.log(Math.min(cost[n-1][0], cost[n-1][1], cost[n-1][2]));
}

// 실행
solution();
```
