## 유데미 알고리즘 자료구조 마스터 클래스 

### Singly Linked Lists

- 다음 데이터 엘리먼트를 가리키는 인덱스 없이 그냥 다수의 데이터 엘리먼트들로 구성된 자료 구조를 뜻한다. 마치 여러개의 칸으로 구성된 기차와 같다고 보면 된다.
하지만 데이터에 접근하기 위해 사용할 인덱스가 존재하지 않는다는 특징이 있다.

- 각각의 엘리먼트 === 노드 => value, pointer(다른 노드를 가리키거나 null 값)


따라서 연결 리스트는 다수의 노드로 구성되고, 각각의 노드는 문자열 혹은 숫자와 같은 하나의 데이터 엘리먼트를 저장한다. 또한 각각의 노드들은 다음 노드를 가리키는 정보 역시 저장하고 있어야하고, 다음 노드가 없다면 아무것도 없음을 뜻하는 null을 저장하게 되는 것이다. 

#### 속성 
- head : 연결리스트의 시작 노드
- tail : 연결리스트의 마지막 노드 
- length : 연결 리스트의 길이

단방향 연결리스트 : 각 노드가 다음 노드로 오직 단일 방향으로만 연결된 리스트 

![](https://velog.velcdn.com/images/jhs000123/post/ae58695c-f6b7-4f5e-8c82-45237351a96a/image.png)


임의 접근이 필요하지 않은 아주 긴 데이터 세트나 많은 정보에 대해서 작업할 경우나 리스트에 저장만 하면 되는 경우에는 연결 리스트를 사용하는 것이 바람직하다. 

```js
class Node {
    constructor(val){
        this.val = val;
        this.next = null;
    }
}

class SinglyLinkedList{
    constructor(){
        this.head = null;
        this.tail = null;
        this.length = 0;
    }
    push(val){
        let newNode = new Node(val)
        if(!this.head){
            this.head = newNode;
            this.tail = this.head;
        } else {
            this.tail.next = newNode;
            this.tail = newNode;
        }
        this.length++;
        return this;
    }
}

let list = new SinglyLinkedList()

```
연결리스트의 push를 간단하게 구현해봤다. 
head가 존재하지 않으면 head 와 tail 모두 추가된 Node에 부여한다. 만약 리스트의 노드가 존재했다면 tail.next를 추가된 노드로 지정하고 tail의 값을 newNode로 지정한다.


## 자료구조 알고리즘 강의 

### 연결 리스트 

각 노드가 데이터와 포인터를 가지고 한줄로 연결되어있는 방식으로 데이터를 저장하는 자료구조


- 장점
	- 추가/제거에 좋다
- 단점
	- 읽기/쓰기에 안좋다. 배열에 비해서 빠르지 않음 

- 특징
	- 언제든 크기는 동적이다.
    - 주소는 비연속적이다.
    
- 요약 : 주소가 비연속적이고 크기가 동적이다.
