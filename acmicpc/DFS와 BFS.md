## 풀이 날짜

2023-12-27

## 문제

https://www.acmicpc.net/problem/1260

## 아이디어

DFS는 실행 스택 기반으로 풀고, BFS는 큐를 이용해서 풀면 된다. (배열을 큐처럼 활용했는데 우선 효율성 테스트에 통과하긴 했다.)

1. 그래프 정보를 `graph` 배열 안의 Set 자료구조로 표현한다.
2. 모든 간선 정보를 순회하면서 Set에 연결 정보를 표현한다.
3. `graph` 배열을 순회하면서 Set을 Array로 바꾸고, 오름차순으로 원소를 정렬한다.
4. 시작점부터 시작해서 DFS, BFS 순회한다.

## 소스코드

```js
const INPUT_NAME = '3.txt';
const IN_BAEKJOON = false;

const filePath = IN_BAEKJOON
  ? '/dev/stdin'
  : require('path').join(__dirname, 'inputs', INPUT_NAME);

const input = require('fs')
  .readFileSync(filePath)
  .toString()
  .trim()
  .split('\n')
  .map((item) => item.trim());

const [n, m, v] = input[0].split(' ').map((item) => Number(item));
let graph = Array.from({ length: n + 1 }, () => new Set());

for (let i = 1; i < input.length; i++) {
  const [a, b] = input[i].split(' ').map((item) => Number(item));
  graph[a].add(b);
  graph[b].add(a);
}

graph = graph.map((set) => [...set].sort((a, b) => a - b));

function findDFSSolution(n, m, v) {
  const result = [];
  const visited = Array.from({ length: n + 1 }, () => false);

  const dfs = (target) => {
    visited[target] = true;
    result.push(target);

    for (const next of graph[target]) {
      if (!visited[next]) {
        dfs(next);
      }
    }
  };

  dfs(v);

  return result;
}

function findBFSSolution(n, m, v) {
  const result = [];
  const queue = [];
  const visited = Array.from({ length: m + 1 }, () => false);

  visited[v] = true;
  queue.push(v);

  while (queue.length > 0) {
    const target = queue.shift();
    result.push(target);

    for (const next of graph[target]) {
      if (!visited[next]) {
        visited[next] = true;
        queue.push(next);
      }
    }
  }

  return result;
}

function solution(n, m, v) {
  console.log(findDFSSolution(n, m, v).join(' '));
  console.log(findBFSSolution(n, m, v).join(' '));
}

solution(n, m, v);
```
