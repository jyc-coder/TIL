## 정렬 알고리즘

### 버블 정렬
- 양옆에 인접한 두 원소를 비교하여 자리를 바꾸는 정렬방법
- 한번 순회할 때마다 원소들이 거품이 올라오는 것처럼 보여서 버블 정렬이라고 부름


### 선택 정렬
- 입력 배열(정렬되지 않은 값들) 이외에 다른 추가 메모리를 요구하지 않는 정렬 방법

- 해당 순서에 원서를 넣을 위치는 이미 정해져 있고 ,어떤 원소를 넣을지 선택하는 알고리즘
	- 첫번째 순서에서는 첫번째 위치에 가장 최솟값
    - 두번째 순서에서는 두번째 위치에서 남은 값중에서의 최솟값을 넣는다.
    - ...
    

### 삽입 정렬
- 새로운 원소를 이전까지 정렬된 원소 사이에 올바르게 삽입시키는 정렬 방법
- 인간이 직접 정렬하는 순서와 제일 흡사

- 0번째 원소는 건너뜀

- 0번 째 원소 ~ 1번 째 원소 중 1번 째 원소 값이 들어가야할 위치를 찾아서 넣기

- 0번 째 원소 ~ 2번 째 원소 중 2번 째 원소 값이 들어가야할 위치를 찾아서 넣기

     .

     .

     .

- 0번 째 원소 ~ n번 째 원소 중 n번 째 원소 값이 들어가야할 위치를 찾아서 넣기

