## 풀이 날짜
2023-07-11

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/42885

## 첫 번째 시도
### 아이디어
몸무게가 가장 적은 사람부터 정렬하고, 순회하면서 보트에 더 들어가지 않는다면 새로운 보트에 태우는 방식으로 풀었다.  

그러나 올바른 풀이가 아니었다.

### 소스코드
```js
function solution(people, limit) {
  // 1. 몸무게 적은 사람순으로 정렬한다.
  // 2. 앞에서부터 처리하다가 용량 꽉차면 count 증가시키고 용량 리셋.
  people = people.sort((a, b) => a - b);
  let weight = limit;

  return people.reduce((acc, person) => {
    const boardable = weight - person >= 0;
    weight = boardable ? weight - person : limit - person;
    return boardable ? acc : acc + 1;
  }, 1);
}
```

## 두 번째 시도
### 아이디어
투 포인터를 활용해서 풀었다.  

1. 몸무게가 가장 많이 나가는 사람부터 정렬한다.  
2. `start` 를 첫 번째 인덱스로, `end` 를 마지막 인덱스로 설정한다.  
3. 모든 사람을 태울 때 까지 반복한다:
    1. 무거운 사람부터 체크하면서 최대한 태운다.  
    2. 더 이상 태울 수 없다면, 가벼운 사람부터 체크하면서 최대한 태운다.
    3. 가벼운 사람도 태울 수 없다면, 카운트를 증가시키고 새로운 보트로 로직을 반복한다.

우선 몸무게가 가장 많이 나가는 사람부터 정렬한다.  
그리고 몸무게가 많이 나가는 사람부터 보트에 태우기 시작하다가 더 이상 태우지 못한다면 몸무게가 적게 나가는 사람부터 태우도록 시도한다.  

### 소스코드
```js
function solution(people, limit) {
  let answer = 0;

  people = people.sort((a, b) => b - a);
  let start = 0;
  let end = people.length - 1;

  while (start <= end) {
    let boat = limit;

    while (boat >= people[start]) {
      boat -= people[start];
      start++;
    }

    while (boat >= people[end]) {
      boat -= people[end];
      end--;
    }

    answer++;
    boat = limit;
  }

  return answer;
}
```
