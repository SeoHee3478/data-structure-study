#  스택 (Stack) - 구현

## 1. 스택의 추상 자료형 
스택은 **LIFO (Last In First Out)** 구조를 가지며, 다음과 같은 주요 연산이 있습니다.

- **push(data)**: 데이터를 스택에 삽입
- **pop()**: 스택의 맨 위 데이터를 제거하고 반환
- **peek()**: 스택의 맨 위 데이터를 제거하지 않고 반환
- **isEmpty()**: 스택이 비어있는지 여부 확인

---

## 2. 스택 구현 (JavaScript + LinkedList 활용)

> **`stack.mjs`**

```js
import { LinkedList } from './linkedList2.mjs';

class Stack {
    constructor() {
        this.list = new LinkedList();
    }

    // 데이터 삽입 (맨 앞에 추가)
    push(data) {
        this.list.insertAt(0, data);
    }

    // 데이터 제거 (맨 앞 제거)
    pop() {
        try {
            return this.list.deleteAt(0);
        } catch (e) {
            return null; // 비어있을 경우 null 반환
        }
    }

    // 맨 위 데이터 확인
    peek() {
        return this.list.getNodeAt(0);
    }

    // 비었는지 여부
    isEmpty() {
        return this.list.count === 0;
    }
}

export { Stack };
```

---

## 3. 테스트 코드

> **`test_stack.mjs`**

```js
import { Stack } from './stack.mjs';

let stack = new Stack();

console.log("=== 첫번째 출력 ===");
stack.push(1);
stack.push(2);
stack.push(3);
stack.push(4);
console.log(stack.pop().data); // 4
console.log(stack.pop().data); // 3
console.log(stack.pop().data); // 2
console.log(stack.pop().data); // 1

console.log("=== 두번째 출력 ===");
stack.push(1);
stack.push(2);
stack.push(3);
stack.push(4);
console.log(stack.peek().data); // 4
stack.pop();
console.log(stack.peek().data); // 3
console.log(`isEmpty: ${stack.isEmpty()}`); // false
stack.pop();
stack.pop();
stack.pop();
console.log(`isEmpty: ${stack.isEmpty()}`); // true
console.log(stack.pop()); // null
```

---

## 4. 실행 예시

```
=== 첫번째 출력 ===
4
3
2
1
=== 두번째 출력 ===
4
3
isEmpty: false
isEmpty: true
null
```

---

## 5. 특징
- 연결 리스트(LinkedList)를 기반으로 구현 → 배열처럼 크기 제한이 없음
- 삽입, 삭제가 항상 **O(1)** 시간에 가능

---

💡 **활용 예시**  
- 되돌리기(Undo) 기능  
- 브라우저 방문 기록 저장  
- 함수 호출 스택 관리
