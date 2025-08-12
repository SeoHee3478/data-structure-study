#  큐(Queue) 구현

## 1. 큐의 추상 자료형(ADT)
- **enqueue** : 데이터 삽입
- **dequeue** : 데이터 제거
- **front** : 맨 앞 데이터 참조
- **isEmpty** : 비었는지 확인

---

## 2. 이중 연결 리스트 (Doubly Linked List) 구현

`DoublyLinkedList.mjs`
```js
class Node {
    constructor(data, next = null, prev = null) {
        this.data = data;
        this.next = next;
        this.prev = prev; 
    }
}

class DoublyLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.count = 0;
    }

    printAll() {
        let currentNode = this.head;
        let text = "[";
        while (currentNode != null) {
            text += currentNode.data;
            currentNode = currentNode.next;
            if (currentNode != null) text += ", ";
        }
        text += "]";
        console.log(text);
    }

    clear() {
        this.head = null;
        this.count = 0;
    }

    insertAt(index, data) {
        if (index > this.count || index < 0) {
            throw new Error("범위를 넘어갔습니다.");
        }

        let newNode = new Node(data);

        if (index == 0) { // 맨 앞 삽입
            newNode.next = this.head;
            if (this.head != null) this.head.prev = newNode;
            this.head = newNode;
        } else if (index == this.count) { // 맨 뒤 삽입
            newNode.next = null;
            newNode.prev = this.tail;
            this.tail.next = newNode;
        } else { // 중간 삽입
            let currentNode = this.head;
            for (let i = 0; i < index - 1; i++) {
                currentNode = currentNode.next;
            }
            newNode.next = currentNode.next;
            newNode.prev = currentNode;
            currentNode.next = newNode;
            newNode.next.prev = newNode;
        }

        if (newNode.next == null) this.tail = newNode;
        this.count++;
    }

    insertLast(data) {
        this.insertAt(this.count, data);
    }

    deleteAt(index) {
        if (index >= this.count || index < 0) {
            throw new Error("제거할 수 없습니다.");
        }

        let currentNode = this.head;

        if (index == 0) { // 맨 앞 제거
            let deleteNode = this.head;
            if (this.head.next == null) { // 데이터 1개
                this.head = null;
                this.tail = null;
            } else { // 데이터 2개 이상
                this.head = this.head.next;
                this.head.prev = null;
            }
            this.count--;
            return deleteNode;
        } else if (index == this.count - 1) { // 맨 뒤 제거
            let deletedNode = this.tail;
            this.tail.prev.next = null;
            this.tail = this.tail.prev;
            this.count--;
            return deletedNode;
        } else { // 중간 제거
            for (let i = 0; i < index - 1; i++) {
                currentNode = currentNode.next;
            }
            let deletedNode = currentNode.next;
            currentNode.next = currentNode.next.next;
            currentNode.next.prev = currentNode;
            this.count--;
            return deletedNode;
        }
    }

    deleteLast() {
        return this.deleteAt(this.count - 1);
    }

    getNodeAt(index) {
        if (index >= this.count || index < 0) {
            throw new Error("범위를 넘어갔습니다.");
        }

        let currentNode = this.head;
        for (let i = 0; i < index - 1; i++) {
            currentNode = currentNode.next;
        }
        return currentNode;
    }
}

export { Node, DoublyLinkedList };
```

---

## 3. 큐 클래스 구현

`Queue.mjs`
```js
import { DoublyLinkedList } from "./DoublyLinkedList.mjs";

class Queue {
    constructor() {
        this.list = new DoublyLinkedList();
    }

    // 데이터 삽입
    enqueue(data) {
        this.list.insertAt(0, data);
    }

    // 데이터 제거
    dequeue() {
        try {
            return this.list.deleteLast();
        } catch (e) {
            return null;
        }
    }

    // 맨 앞 데이터 참조
    front() {
        return this.list.tail;
    }

    // 비었는지 확인
    isEmpty() {
        return (this.list.count == 0);
    }
}

export { Queue };
```

---

## 4. 큐 동작 테스트

`test_queue.mjs`
```js
import { Queue } from "./Queue.mjs";

let queue = new Queue();

console.log("====== enqueue() 세 번 호출 ======");
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);

console.log(queue.front());

console.log("====== dequeue() 네 번 호출 ======");
console.log(queue.dequeue());
console.log(queue.dequeue());
console.log(queue.dequeue());
console.log(queue.dequeue());

console.log(`isEmpty: ${queue.isEmpty()}`);
```

---

## 실행 결과 예시
```
====== enqueue() 세 번 호출 ======
Node { data: 1, next: null, prev: null }
====== dequeue() 네 번 호출 ======
Node { data: 1, ... }
Node { data: 2, ... }
Node { data: 3, ... }
null
isEmpty: true
```
