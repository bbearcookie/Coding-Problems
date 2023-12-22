## 풀이 날짜

2023-12-22

## 문제

https://school.programmers.co.kr/learn/courses/30/lessons/250137

## 아이디어

매 초마다 회복하거나 데미지를 받는 것을 계산해서 풀었다.

`attacks` 배열만 순회하면 더 빠른 시간 복잡도로 처리할 수 있긴 하겠지만 입력 데이터가 많지 않으므로 마지막 공격이 끝나는 시간까지 반복을 돌려도 될 거라고 판단했다.

## 소스코드

```js
function solution(bandage, health, attacks) {
  let nextAttackIndex = 0;
  let successCount = 0;
  const maxHealth = health;

  for (let i = 0; i <= attacks[attacks.length - 1][0]; i++) {
    const [nextAttackTime, nextAttackDamage] = attacks[nextAttackIndex];

    if (i === nextAttackTime) {
      // 공격
      health -= nextAttackDamage;
      successCount = 0;
      nextAttackIndex++;
    } else {
      // 회복
      successCount++;
      health = Math.min(health + bandage[1], maxHealth);
    }

    // 추가 회복
    if (successCount === bandage[0]) {
      successCount = 0;
      health = Math.min(health + bandage[2], maxHealth);
    }

    // 사망
    if (health <= 0) {
      health = -1;
      break;
    }
  }

  return health;
}
```
