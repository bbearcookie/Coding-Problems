Queue 자료구조가 필요할 때 사용하는 연결 리스트 기반의 큐

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedListQueue {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  push(value) {
    const newNode = new Node(value);

    if (this.size === 0) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      this.tail.next = newNode;
      this.tail = newNode;
    }

    this.size++;
  }

  shift() {
    if (this.size <= 0) {
      return;
    }

    const resultNode = this.head;

    this.head = this.head.next;
    this.size--;

    return resultNode.value;
  }

  get length() {
    return this.size;
  }
}
```
