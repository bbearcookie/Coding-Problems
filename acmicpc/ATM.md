## 풀이 날짜
2023-02-26

## 문제
https://www.acmicpc.net/problem/11399

## 아이디어
소요 시간이 적게 걸리는 사람부터 진행해야 전체 사람이 기다려야 하는 시간이 줄어들게 되고 전체 대기시간의 총합이 최소가 되므로,
소요 시간이 적은 사람부터 은행 업무를 처리하고 해당 사람들이 대기해야 하는 시간을 합하여 출력한다.

## 소스코드
```js
// 입력 처리
const INPUT_NAME = "i1.txt";
const filePath = process.platform === "linux" ? "/dev/stdin" : `./${__dirname.split('\\').pop()}/${INPUT_NAME}`;
const input = require("fs").readFileSync(filePath).toString().trim().split("\n").map(item => item.trim());
const n = Number(input[0]);
const arr = input[1].split(' ').map(e => Number(e)).sort((a, b) => a - b);

// 문제
function solution() {
  let time = 0;
  let result = 0;

  arr.forEach(e => {
    time += e;
    result += time;
  });

  console.log(result);
}

// 실행
solution();
```
