## A 4-step process for effective system design interview

- 최종 design보다 과정 중요
- 기술적 디자인 능력, 문제 해결 능력, 의사소통 능력, 협업 능력 평가
- over-engineering 지양

### Step 1 - Understand the problem and establish design scope

- 빠르게 답변하는 것 중요 X
- 문제를 이해하고 질문을 통해 요구 사항과 가정을 명확히 하는 것이 중요

#### List of questions to help you get started:

- What specific features are we going to build?
- How many users does the product have?
- How fast does the company anticipate to scale up? What are the anticipated scales in 3 months, 6 months, and a year?
- What is the company's technology stack? What existing services you might leverage to simplify the design?

#### Example

뉴스 피드 시스템을 설계하라
- Mobile app / web app / both?
- Most important features?
- News feed sorted order?
- How many friends can a user have?
- What is the traffic volume? (ex. 10 million DAU)
- Can feed contain images, videos, or just text? (ex. 미디어 파일 - 이미지, 비디오)

### Step 2 - Propose high-level design and get buy-in

initial blueprint (초기 설계안) 만들기  
-> feedback 요청  
-> back-of-the-envelope 계산을 통해 설계안이 규모에 맞는지 평가

- API endpoint / database schema를 포함할지 여부?
  - API endpoint : 클라이언트가 API endpoint를 통해 서버에 데이터를 요청하거나 보낼 수 있음
  - Database schema : 데이터베이스 구조를 정의하는 틀이나 청사진, 데이터베이스에 저장되는 데이터의 조직, 형식, 관계 등 설명, 테이블, 칼럼, 데이터 유형, 인덱스, 외래 키 등 포함

#### Example

1. Feed publishing : 사용자가 게시물을 작성하면 해당 데이터가 cache/database에 기록되고 친구의 뉴스 피드에 게시물이 나타남
2. Newsfeed building : 친구들의 게시물을 역순으로 집계하여 만들어짐

![스크린샷 2024-02-11 오후 8 30 45](https://github.com/jylee2033/book-study/assets/85793553/98c9e9bc-b45a-43cd-ba65-0e6a80008229)

##### Feed publishing

웹 서버의 3가지 주요 서비스
1. Post service : 게시물과 관련된 로직 처리
2. Fanout service : 사용자의 뉴스피드를 구성할 때, 사용자의 친구들의 게시물을 뉴스피드 cache에 빠르게 배포하는 기능 담당
3. Notification service : 사용자가 새 게시물을 올릴 때, 친구들에게 알림 보내는 서비스

![스크린샷 2024-02-12 오후 8 36 44](https://github.com/jylee2033/book-study/assets/85793553/15efc7f8-395a-44e5-a67b-ed289abecfd8)

##### Newsfeed building

### Step 3 - Design deep dive

#### Achieved following objectives:

- Agreed on the overall goals and feature scope
- Sketched out a high-level blueprint for the overall design
- Obtained feedback from your interviewer on the high-level design

-> architecture 내의 구성 요소를 identify하고 우선순위를 정함  
-> high-level design에 집중하거나 일부 세부 사항에 집중

중요한 것 : 세부 사항에 너무 몰두하지 않기

#### Example

#### Feed publishing

![스크린샷 2024-02-12 오후 9 23 34](https://github.com/jylee2033/book-study/assets/85793553/9162414b-bbfc-4873-ae21-223c9f636582)

##### News feed system의 전체 architecture

- Web servers : 인증과 속도 제한 기능 처리
- Fanout Service : Message Queue와 Fanout Workers를 통해 사용자의 뉴스 피드 구성
- Graph DB : 사용자 간의 관계 관리

#### News feed retrieval

![스크린샷 2024-02-12 오후 9 24 18](https://github.com/jylee2033/book-study/assets/85793553/6cdc351c-7f88-43e3-afb1-57a5e3d5dd95)

#### News feed building 과정에 초점을 맞춤

- 사용자 요청은 CDN(Content Delivery Network)을 통해 처리될 수 있음
- News Feed Service : User와 Post DB 사이의 상호작용 관리
- Cache : 빠른 데이터 액세스를 위해 사용

### Step 4 - Wrap up

추가적인 point
- 시스템의 bottlenecks
- design을 요약하여 기억 refresh
- server failure, network loss 등의 오류 사례

#### Dos

- assumption이 무조건 맞다고 생각하지 말고 확인 요청
- 문제의 requirements 이해
- 다양한 접근 방법 제안
- interviewer와 아이디어를 공유하며 협력

#### Don'ts

- requirements와 assumptions를 명확히 하지 않고 solution에 뛰어들지 않음
- single component에 대해 너무 많은 detail에 초점을 맞추지 않음
- 막혔을 때 도움 요청하는 것을 주저하지 않음
- design을 마친 후 인터뷰가 끝났다고 생각하지 않음

#### Time allocation on each step

- Step 1 : 3 - 10 min
- Step 2 : 10 - 15 minutes
- Step 3 : 10 - 25 minutes
- Step 4 : 3 - 5 minutes
