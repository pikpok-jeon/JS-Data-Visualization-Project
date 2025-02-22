# Data Visualization Projects 
- Demo Pages <br />
<a href="https://pikpokjeon.github.io/JS-Data-Visualization-Project/Algorithms/binarySearch.html">1. Binary Search</a> <br />
<a href="https://pikpokjeon.github.io/JS-Data-Visualization-Project/Charts/index.html">2. Line Charts</a>

### I am aiming at ... 
- developing this listed projects without any help of foreign JS libraries
- containing at least one with custom interactive animation 
- using interactive and dynamic data given by users 
- Creating a chart web-component system

### HOW TO RUN 
- git clone this repo
- make sure there's at least a couple of html and js files in the project folder you found it interesting <br />
To start Chart project
```
npm run parcel
```

Insert it in the file named event.js for preventing async/await error on Parcel
```
import 'regenerator-runtime/runtime' 

```
To start Binary search
- Just drag and drop the html file to your choice of browser!

## Project List

### 1. Algorithms 
### (1) Binary search
#### Last update (17,Apr) : TODO > Add searching failure case, adjust and maintain its width

![demonstration-webpages](https://github.com/pikpokjeon/JS-Data-Visualization-Project/blob/main/bs.gif)

### 2. Chart 
### (1) Line chart
##### Last update (24,Apr): TODO > make width responsive

![demonstration-webpages](https://github.com/pikpokjeon/JS-Data-Visualization-Project/blob/main/linechart.gif)

## Chart 프로젝트가 리팩토링이 필요한 이유
- 공유하는 동적 데이터의 흐름을 추적하기 힘들다
- 해당 데이터의 변화에 따른 메서드의 호출를 명확히 파악하기 힘들다
- SVG 요소를 여러번 수정해야하는 경우 하드코딩을 하고 있다

## 리팩토링 계획
- 공통적으로 접근해야하는 데이터를 관리하기 위해 PubSub 패턴을 사용하여 데이터를 생성하는 주체와 사용하는 주체를 분리하여 관리
- SVG 요소를 변화 해야하는 경우, 예정된 변화될 요소들을 그룹화하고 적용하는 함수 생성
- SVG 요소의 변화에 순서가 있다면 순서에 따른 동기식 적용 호출 함수 생성

## Pub/sub 서비스 ? 
- 메시지 전송자(Publisher)가 메시지 수신자(Subscriber)와 분리되는 메시징 서비스
```
상에서 하로 진행, 한가지 주제에 일대다 다대일 다대다 관계 성립가능.
1. Publisher (pub/sub 서비스 주제 생성) publisher messages a topic to  subscriber (might via broker)
3. Message (페이로드/컨텐츠설명하는 속성) 
4. MessageStorage filtered with topic/path (메시지는 구독자가 메시지를 소비할 때까지 구독에 보관)
5. Subscription (target to assign message, message)
6. Subscriber (target : 수신된 메세지에 확인 (Ack/Notify)) receives subscription 
```
#### 주요 개념
```
메시지: 서비스를 통해 이동하는 데이터..
주제: 메시지 피드를 나타내는, 이름이 지정된 항목.
구독: 특정 주제의 메시지 수신을 받고자 하는, 이름이 지정된 항목.
게시자(publisher): 특정 주제에 대한 메시지를 만들어 메시징 서비스로 전송(게시)합니다.
구독자(subscribe): 지정한 구독에 대한 메시지를 수신합니다. 
```
#### SVG 라이브러리 구조
- Topic 메세지(상태)를 관리하는 스토어 (MessageStorage)
- svg 요소와 사용자 입력데이터에 대한 주제
- 새로운 메세지를 생성하는 메서드 (Publisher) 주체
- 발행된 메세지가 가공되 이동하는 파이프라인 ->  Subscriber로 이동
- Subscriber가 메세지를 수신하면, 승인 (update)를 진행
