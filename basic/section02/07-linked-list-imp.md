# 연결 리스트 (Linked List) 구현

## 📌 추상자료형(ADT, Abstract Data Type)
> 데이터와 그 데이터를 다루는 연산을 **정의**하는 것.  
> 구체적인 구현 방법은 포함하지 않음.

예시:  
세탁기에 빨래를 하기 위해서는 옷(데이터)이 필요하고,  
빨래, 탈수, 배수 등의 기능(연산)이 필요함.  
하지만 내부 구조(모터 회전 방식 등)는 추상자료형 설명에 포함되지 않음.

---

## 📌 연결 리스트의 추상자료형
연결 리스트에서 정의할 수 있는 대표적인 연산:
1. **printAll()** - 모든 데이터 출력
2. **clear()** - 모든 데이터 제거
3. **insertAt(index, data)** - 특정 인덱스에 삽입
4. **insertLast(data)** - 마지막에 삽입
5. **deleteAt(index)** - 특정 인덱스 삭제
6. **deleteLast()** - 마지막 노드 삭제
7. **getNodeAt(index)** - 특정 인덱스의 노드 반환

---

## 📄 구현 코드

### `linkedList.mjs`
```js
class Node {
    constructor(data, next = null) {
        this.data = data;
        this.next = next;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.count = 0;
    }

    // 모든 데이터 출력
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

    // 모든 데이터 제거
    clear() {
        this.head = null;
        this.count = 0;
    }

    // 특정 위치에 삽입
    insertAt(index, data) {
        if (index > this.count || index < 0) {
            throw new Error("범위를 넘어갔습니다.");
        }
        let newNode = new Node(data);
        if (index === 0) {
            newNode.next = this.head;
            this.head = newNode;
        } else {
            let currentNode = this.head;
            for (let i = 0; i < index - 1; i++) {
                currentNode = currentNode.next;
            }
            newNode.next = currentNode.next;
            currentNode.next = newNode;
        }
        this.count++;
    }

    // 마지막에 삽입
    insertLast(data) {
        this.insertAt(this.count, data);
    }

    // 특정 위치 삭제
    deleteAt(index) {
        if (index >= this.count || index < 0) {
            throw new Error("제거할 수 없습니다.");
        }
        let currentNode = this.head;
        if (index === 0) {
            let deletedNode = this.head;
            this.head = this.head.next;
            this.count--;
            return deletedNode;
        } else {
            for (let i = 0; i < index - 1; i++) {
                currentNode = currentNode.next;
            }
            let deletedNode = currentNode.next;
            currentNode.next = currentNode.next.next;
            this.count--;
            return deletedNode;
        }
    }

    // 마지막 노드 삭제
    deleteLast() {
        return this.deleteAt(this.count - 1);
    }

    // 특정 인덱스 노드 가져오기
    getNodeAt(index) {
        if (index >= this.count || index < 0) {
            throw new Error("범위를 넘어갔습니다.");
        }
        let currentNode = this.head;
        for (let i = 0; i < index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode;
    }
}

export { Node, LinkedList };
```

---

### `test.mjs`
```js
import { Node, LinkedList } from "./linkedList.mjs";

// Node 연결 예시
let node1 = new Node(1);
let node2 = new Node(2);
let node3 = new Node(3);
node1.next = node2;
node2.next = node3;

console.log(node1.data);             // 1
console.log(node1.next.data);        // 2
console.log(node1.next.next.data);   // 3

// LinkedList 테스트
let list = new LinkedList();

console.log("===== insertAt() 다섯 번 호출 =====");
list.insertAt(0, 0);
list.insertAt(1, 1);
list.insertAt(2, 2);
list.insertAt(3, 3);
list.insertAt(4, 4);
list.printAll();

console.log("===== clear() 호출 =====");
list.clear();
list.printAll();

console.log("===== insertLast() 호출 =====");
list.insertLast(0);
list.insertLast(1);
list.insertLast(2);
list.printAll();

console.log("===== deleteAt() 호출 =====");
list.deleteAt(0);
list.printAll();
list.deleteAt(1);
list.printAll();

console.log("===== deleteLast() 호출 =====");
list.insertLast(5);
list.deleteLast();
list.deleteLast();
list.printAll();

console.log("===== getNodeAt() 호출 =====");
list.insertLast(1);
list.insertLast(2);
list.insertLast(3);
list.insertLast(4);
list.insertLast(5);

let secondNode = list.getNodeAt(2);
console.log(secondNode);
```

---

## 💻 실행 결과
```
1
2
3
===== insertAt() 다섯 번 호출 =====
[0, 1, 2, 3, 4]
===== clear() 호출 =====
[]
===== insertLast() 호출 =====
[0, 1, 2]
===== deleteAt() 호출 =====
[1, 2]
[1]
===== deleteLast() 호출 =====
[]
===== getNodeAt() 호출 =====
Node {
  data: 2,
  next: Node { data: 3, next: Node { data: 4, next: [Node] } }
}
```

---


## 📚 정리
- 연결 리스트는 **노드(Node)**가 `data`와 `next` 포인터를 가지는 구조
- 삽입/삭제 시 **인덱스를 직접 지정** 가능
- 마지막에 삽입/삭제는 `insertLast()`, `deleteLast()`로 처리
- 현재 데이터 개수는 `this.count`로 관리하여 범위 검사에 사용
