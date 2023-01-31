# 단일 연결 리스트 

## shift()
- 첫번째 요소를 제거하는 메소드
- 두번째 요소가 head가 되며 리스트의 길이가 1 줄어들게 된다.
- 만약 this.head 의 값이 null일 경우에는 undefined가 리턴됨
- 메소드를 호출할 때 리스트의 길이가 0이라면 this.tail의 값은 null로 지정

```js
// 첫번째 항목을 제거함
    shift() {
       if(!this.head) return undefined
        let current = this.head;
        this.head = current.next;
        this.length--;
         if(this.length === 0) {
            this.tail = null;
        }
        return current
        
    }
```

## unshift(val)
- val값을 가진 새로운 노드를 첫 위치로 추가한다.
- 기존의 head 값을 새로운 노드의 next값으로 지정
- 리스트가 비어있다면 head는 새로운 노드의 값으로 지정, tail은 head와 같은 값으로 지정한다.
- 만약 리스트가 비어있지 않을 경우에는 head를 새로운 노드의 next로 지정한다.
- 리스트 길이를 1 늘려줄 것

```js
// 첫번째 항목 추가 
    unshift(val) {
         let newNode = new Node(val)
         let current = this.head;
        if(!this.head){
            this.head = newNode;
            this.tail = this.head;
        } else{
             newNode.next = current
            this.head = newNode;
            
        }
       
        this.length++;
            return newNode
    }
```
