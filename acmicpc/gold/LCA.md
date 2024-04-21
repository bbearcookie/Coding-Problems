## 풀이 날짜

2024-04-21

## 문제

https://www.acmicpc.net/problem/11437

## 아이디어

1. 1번 노드를 깊이 1로 간주하고 간선 노드를 순회한다. 그 과정에 자식 노드는 (부모 노드의 깊이 + 1) 의 값을 할당한다.
2. 두 정점의 가까운 공통 조상 정보를 나타내는 `pairs` 를 순회한다.  
   2-1. 먼저 두 노드의 depth를 맞춘다.
   2-2. depth를 맞췄다면, 두 노드가 동일한 노드를 가리킬 때까지 부모 노드로 올라간다.
   2-3. 동일하다면 정답 배열에 추가한다.
3. 정답 배열을 출력한다.

## 시간 초과

작성했으나, 시간 초과가 발생했다.

```js
const INPUT_NAME = '1.txt';
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

const N = Number(input[0]);
const M = Number(input[N]);
const lines = input
  .slice(1, N)
  .map((line) => line.split(' ').map((e) => Number(e)));
const pairs = input
  .slice(N + 1, input.length)
  .map((line) => line.split(' ').map((e) => Number(e)));

function solution() {
  const results = [];
  const depths = Array.from({ length: N + 1 }, () => Infinity);
  const parents = Array.from({ length: N + 1 }, () => 0);
  depths[1] = 1;

  lines.forEach(([a, b]) => {
    const p = depths[a] <= depths[b] ? a : b;
    const c = p === a ? b : a;

    depths[c] = depths[p] + 1;
    parents[c] = p;
  });

  pairs.forEach(([a, b]) => {
    let p = depths[a] <= depths[b] ? a : b;
    let c = p === a ? b : a;

    // depth를 맞춤
    while (depths[p] !== depths[c]) {
      c = parents[c];
    }

    // 공통 부모 찾기
    while (p !== c) {
      p = parents[p];
      c = parents[c];
    }

    results.push(p);
  });

  console.log(results.join('\n'));
}

solution();
```

## 해결 방법

depths 배열을 만드는 과정에서 시간 초과가 발생하는 것이었고, 1번 노드부터 DFS 순회하되 이미 거쳐간 정점은 스킵하는 방식으로 해결했다.

## 소스코드

```js
const INPUT_NAME = '1.txt';
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

const N = Number(input[0]);
const M = Number(input[N]);
const lines = input
  .slice(1, N)
  .map((line) => line.split(' ').map((e) => Number(e)));
const pairs = input
  .slice(N + 1, input.length)
  .map((line) => line.split(' ').map((e) => Number(e)));

function solution() {
  const results = [];
  const graphs = Array.from({ length: N + 1 }, () => []);
  const depths = Array.from({ length: N + 1 }, () => Infinity);
  const parents = Array.from({ length: N + 1 }, () => 0);
  const visited = Array.from({ length: N + 1 }, () => 0);

  lines.forEach(([a, b]) => {
    graphs[a].push(b);
    graphs[b].push(a);
  });

  const dfs = (current, depth) => {
    visited[current] = 1;
    depths[current] = depth;

    for (const next of graphs[current]) {
      if (visited[next]) continue;

      parents[next] = current;
      dfs(next, depth + 1);
    }
  };

  dfs(1, 1);

  pairs.forEach(([a, b]) => {
    let p = depths[a] <= depths[b] ? a : b;
    let c = p === a ? b : a;

    // depth를 맞춤
    while (depths[p] !== depths[c]) {
      c = parents[c];
    }

    // 공통 부모 찾기
    while (p !== c) {
      p = parents[p];
      c = parents[c];
    }

    results.push(p);
  });

  console.log(results.join('\n'));
}

solution();
```
