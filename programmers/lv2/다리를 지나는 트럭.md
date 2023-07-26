## 풀이 날짜
2023-07-22

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/42583

## 첫 번째 시도
큐를 활용해서 풀어야겠다고 생각했다.  

모든 트럭을 순회하면서 도로에 올라갈 수 있는 트럭을 최대한 큐에 넣고, 더 이상 무거워서 들어갈 수 없다면 큐에 있는 트럭을 한꺼번에 빼주는 방식으로 구현했다.  

그러나 올바른 풀이가 아니었던게 **앞에 있는 트럭이 뒤에 있는 트럭보다 빨리 나가는 경우**도 있기 때문에 한번에 묶어서 처리하는 방법은 오답이었다.

### 소스코드
```js
function solution(bridge_length, weight, truck_weights) {
  let answer = 0;
  let bridgeWeight = weight;
  let queue = [];

  const flushQueue = () => {
    if (queue.length > 0) {
      answer += bridge_length;
      bridgeWeight = weight;
      queue = [];
    }
  };

  const pushQueue = (truck) => {
    bridgeWeight -= truck;
    queue.push(truck);
  };

  truck_weights.forEach((truck) => {
    if (bridgeWeight - truck >= 0) {
      pushQueue(truck);
      answer++;
    } else {
      flushQueue();
      pushQueue(truck);
    }
  });

  flushQueue();
  return answer;
}
```

## 두 번째 시도
마찬가지로 큐를 활용해서 풀었는데 이번에는 큐에 `[종료 시각, 트럭의 무게]` 데이터를 넣었다.  

무게가 가능한 만큼 트럭을 올리고, 더 이상 무거워서 트럭을 올릴 수 없어지면 다음 트럭을 올릴 수 있을 때까지 도로에 올라와있는 트럭을 내린 뒤 시간을 계산하는 방법을 떠올렸다.  

제공되어 있는 테스트 케이스는 통과했지만, 정답은 아니었다.

### 소스코드
```js
function solution(bridge_length, weight, truck_weights) {
  const queue = [];
  let time = 0;
  let bridge = 0;

  truck_weights.forEach((truck) => {
    time++;

    if (bridge + truck <= weight) {
      // 현재 트럭 올리기
      bridge += truck;
      queue.push([time + bridge_length, truck]);
    } else {
      // 현재 트럭을 올릴 수 있을 때까지 큐에 있는 녀석 빼기
      while (bridge + truck > weight) {
        const [endTime, poppedTruck] = queue.shift();
        bridge -= poppedTruck;
        time = endTime;
      }

      // 다 뺐으면 현재 트럭 올리기
      bridge += truck;
      queue.push([time + bridge_length, truck]);
    }
  });

  return queue.length > 0 ? queue.pop()[0] : time;
}
```

## 성공한 풀이
### 아이디어
도로의 정보를 배열로 표현해서 풀었다. 모든 트럭을 순회하면서 아래 로직을 수행한다:  

1. 다음 트럭의 차례가 되었다는 것은 시간이 1초 지났다는 것이다.  
따라서 시간을 증가시키고 도로의 맨 앞에 존재하는 요소를 빼서 도로 무게를 계산한다.  
2. 만약 현재 트럭을 도로에 넣을 수 있는 용량이 되지 않는다면  
다음 로직을 반복하여 도로 상황을 정리한다:  
>    1. 도로에서 맨 앞을 달리는 트럭을 구한다.
>    2. 그 트럭까지의 도로 앞 부분을 제거하고 뒤쪽에 붙인다.
>    3. 그 트럭이 도착하기까지의 시간을 더한다.
>    4. 도로에 점유된 그 트럭의 무게를 뺀다.
3. 도로의 맨 뒷쪽에 현재 순회중인 트럭을 추가하고 도로 무게를 증가시킨다.

모든 트럭의 순회가 끝나면 도로의 배열에 마지막 트럭이 아직 남아있기 때문에 배열의 길이만큼 `time` 값을 더해준다.

### 소스코드
```js
function solution(bridge_length, weight, truck_weights) {
  let bridge = Array.from({ length: bridge_length }, () => 0);
  let time = 0;
  let onLoad = 0;

  truck_weights.forEach((truck) => {
    time++;
    onLoad -= bridge.shift();

    while (onLoad + truck > weight) {
      const nextIdx = bridge.findIndex((item) => item !== 0);
      const nextTruck = bridge[nextIdx];

      bridge = bridge.slice(nextIdx + 1, bridge.length).concat(Array.from({ length: nextIdx + 1 }, () => 0));

      time += nextIdx + 1;
      onLoad -= nextTruck;
    }

    onLoad += truck;
    bridge.push(truck);
  });

  time += bridge.length;
  return time;
}
```

## 참고 자료
https://jyeonnyang2.tistory.com/202  
https://jie0025.tistory.com/428  
