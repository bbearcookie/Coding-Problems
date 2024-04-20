Union-Find 알고리즘이 필요할 때 적용할 서로소 집합 자료구조

```js
class DisjointSet {
  constructor(size) {
    this.parent = Array.from({ length: size }, (_, i) => i);
  }

  find(value) {
    const { parent } = this;
    if (parent[value] === value) return value;
    return (parent[value] = this.find(parent[value]));
  }

  union(a, b) {
    const { parent } = this;

    a = this.find(a);
    b = this.find(b);

    if (a < b) parent[b] = a;
    else parent[a] = b;
  }

  isConnected(a, b) {
    return this.find(a) === this.find(b);
  }
}
```
