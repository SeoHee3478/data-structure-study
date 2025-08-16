# 덱 (Deque)

덱(Deque)은 데이터의 삽입과 제거를 **head**와 **tail** 양쪽에서 자유롭게 할 수 있는 자료 구조
이를 이용하면 **스택**과 **큐**를 모두 구현 가능

---

## 📌 덱의 추상 자료형

| 메서드명        | 설명 |
|----------------|------|
| `printAll`     | 모든 데이터 출력 |
| `addFirst`     | head에 데이터 삽입 |
| `removeFirst`  | head에서 데이터 제거 |
| `addLast`      | tail에 데이터 삽입 |
| `removeLast`   | tail에서 데이터 제거 |
| `isEmpty`      | 리스트가 비었는지 확인 |

---

## 📂 Deque.mjs

```js
import { DoublyLinkedList } from './DoublyLinkedList.mjs';

class Deque {
    constructor() {
        this.list = new DoublyLinkedList();
    }

    printAll() {
        this.list.printAll();
    }

    addFirst(data) {
        this.list.insertAt(0, data); // O(1) 성능
    }

    removeFirst() {
        return this.list.deleteAt(0); // O(1) 성능
    }

    addLast(data) {
        this.list.insertAt(this.list.count, data);
    }

    removeLast() {
        return this.list.deleteLast();
    }

    isEmpty() {
        return (this.list.count === 0);
    }
}

export { Deque };
📂 DequeTest.mjs
js
복사
편집
import { Deque } from './Deque.mjs';

let deque = new Deque();

console.log("==== addFirst =====");
console.log(`isEmpty: ${deque.isEmpty()}`);
deque.addFirst(1);
deque.addFirst(2);
deque.addFirst(3);
deque.addFirst(4);
deque.addFirst(5);
deque.printAll();
console.log(`isEmpty: ${deque.isEmpty()}`);
console.log("\n");

console.log("=== removeFirst =====");
deque.removeFirst();
deque.printAll();
deque.removeFirst();
deque.printAll();
deque.removeFirst();
deque.printAll();
deque.removeFirst();
deque.printAll();
deque.removeFirst();
deque.printAll();
console.log(`isEmpty: ${deque.isEmpty()}`);
console.log("\n");

console.log("=== addLast =====");
deque.addLast(1);
deque.addLast(2);
deque.addLast(3);
deque.addLast(4);
deque.addLast(5);
deque.printAll();
console.log(`isEmpty: ${deque.isEmpty()}`);
console.log("\n");

console.log("=== removeLast =====");
deque.removeLast();
deque.printAll();
deque.removeLast();
deque.printAll();
deque.removeLast();
deque.printAll();
deque.removeLast();
deque.printAll();
deque.removeLast();
deque.printAll();
console.log(`isEmpty: ${deque.isEmpty()}`);
console.log("\n");
💡 덱 활용 예시
스택: addFirst + removeFirst 또는 addLast + removeLast

큐: addFirst + removeLast
