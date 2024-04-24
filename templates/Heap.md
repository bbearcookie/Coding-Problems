우선순위 큐가 필요할 때 사용하는 최소/최대 힙

1. 생성자에 `min | max` 타입 지정해서 사용할 수 있음
2. compare 함수에서 원소 값 자체를 비교했는데, 원소가 객체, 배열이어서 추가적인 핸들링이 필요하다면 이 부분만 변경하면 됨.

```js
class Heap {
  /** @param {'min' | 'max'} type - Heap의 타입입니다. 'min' 또는 'max'만 가능합니다. */
  constructor(type) {
    this.memory = [null];
    this.type = type;
  }

  get size() {
    return this.memory.length - 1;
  }

  compare(higher, lower) {
    const { memory, type } = this;

    if (type === 'max') {
      return memory[higher] > memory[lower];
    } else {
      return memory[lower] > memory[higher];
    }
  }

  push(value) {
    const { memory } = this;
    memory.push(value);

    let child = memory.length - 1;

    while (child > 1) {
      let parent = Math.floor(child / 2);

      if (this.compare(parent, child)) {
        break;
      }

      [memory[parent], memory[child]] = [memory[child], memory[parent]];
      child = parent;
    }
  }

  pop() {
    const { memory } = this;

    if (memory.length === 1) {
      return undefined;
    } else if (memory.length === 2) {
      return memory.pop();
    }

    const result = memory[1];
    memory[1] = memory.pop();

    let parent = 1;
    let child = 2;

    while (child < memory.length) {
      if (child + 1 < memory.length && this.compare(child + 1, child)) {
        child++;
      }

      if (this.compare(parent, child)) {
        break;
      }

      [memory[parent], memory[child]] = [memory[child], memory[parent]];
      parent = child;
      child *= 2;
    }

    return result;
  }
}
```
