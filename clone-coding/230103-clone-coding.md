## 클론 코딩 연습

### 막힌 부분

![](https://velog.velcdn.com/images/jhs000123/post/21a33f24-8bcb-4c2d-ab31-980f09b22dbe/image.png)

input을 입력하는 부분의 길이가 끝까지 늘어나지 않는 현상이 생겼고 width를 100%로 늘려도 변동하지 않음

#### 해결 방법

전체의 display가 `flex`였기 때문에 `flex-grow`의 값을 1로 해줘서 최대로 늘리는것까지는 좋았지만,
![](https://velog.velcdn.com/images/jhs000123/post/a86c098a-53cc-4a62-a99d-935483c4d427/image.png)

이렇게 `placeholder`가 가운데오 와버렸다. 내가 원하는 것은 왼쪽으로 치우쳐져 있는 것인데, 하고 다시 살펴보다가 input의 길이 자체를 100%로 늘려서 구현에 성공했음.
            
 ![](https://velog.velcdn.com/images/jhs000123/post/ed7ee62d-6cf7-4c3a-84c3-c2c10d478102/image.png)
 
 내용을 둘러싸는 껍데기를 100%로 했으니 내용물이 늘어나지 않았던 것이다.
 
 ![](https://velog.velcdn.com/images/jhs000123/post/7f77c9ac-7de9-4603-a996-6965cf1eca60/image.png)



