# 도서예약주문 bstore

## 서비스 소스 레파지토리
- https://github.com/joongsa/bs-assignment.git
- https://github.com/joongsa/bs-book.git
- https://github.com/joongsa/bs-reservation.git
- https://github.com/joongsa/bs-gateway.git
- https://github.com/joongsa/bs-customercenter.git

## 서비스 시나리오

### 기능적 요구사항
1. 고객이 특정 책을 주문한다.
2. 책의 수량을 체크하여 예약가능/예약불가 확인한다.
3. 예약시 도서 수량이 줄고 배정에 전달되어 도서관을 배정한다.
4. 배정이 완료되면 예약완료상태로 주문을 변경한다.
5. 예약완료 시 예약취소할수 있다.
6. 예약취소되면 책 수량을 원복하고, 예약상태를 bookCanceled 로 변경한다.

### 비기능적 요구사항

#### 트랜잭션
 - 책수량이 없는 경우 예약은 처리되지 않는다. Sync 호출(Req/Res)

#### 장애격리
 - 배정관리 서비스가 되지않더라도 책예약은 정상적으로 처리가 되어야한다. Async (event-driven)

## 이벤트스토밍

![이벤트스토밍](https://user-images.githubusercontent.com/45332921/93431907-c2fe0e00-f8ff-11ea-859c-209d37777fb6.jpg)

## 구현
- 분석/설계 단계에서 도출된 헥사고날 아키텍처에 따라, 각 BC별로 대변되는 마이크로 서비스들을 스프링부트로 구현
- 구현한 각 서비스를 로컬에서 실행하는 방법은 아래와 같다 

### DDD 의 적용
- 각 서비스내에 도출된 핵심 객체를 Entity 로 선언
  - 예약 -> reservation
  - 배정 -> assignment
  - 도서 -> book

## SAGA 패턴

- 책 주문을하면, 수량이 줄고, 배정이 됨 
- 취소하면 수량 원복되고, 예약상태 변경됨

![1](https://user-images.githubusercontent.com/45332921/93431995-e32dcd00-f8ff-11ea-9d12-962ee4eff5f1.JPG)
![1](https://user-images.githubusercontent.com/45332921/93431995-e32dcd00-f8ff-11ea-9d12-962ee4eff5f1.JPG)
![2](https://user-images.githubusercontent.com/45332921/93431996-e3c66380-f8ff-11ea-98fe-2ad98a7a69b2.JPG)
![3](https://user-images.githubusercontent.com/45332921/93431998-e3c66380-f8ff-11ea-9710-38e914f39731.JPG)
![4](https://user-images.githubusercontent.com/45332921/93431999-e45efa00-f8ff-11ea-8490-73352224156c.JPG)
![5 cancle](https://user-images.githubusercontent.com/45332921/93432003-e45efa00-f8ff-11ea-90a8-78027ac3136e.JPG)
![6 assignment 삭제](https://user-images.githubusercontent.com/45332921/93432005-e4f79080-f8ff-11ea-8840-944f934f682f.JPG)
![7 cancled](https://user-images.githubusercontent.com/45332921/93431994-e2953680-f8ff-11ea-8060-085e164c8ac0.JPG)

## CQRS

![CQRS](https://user-images.githubusercontent.com/45332921/93432352-60f1d880-f900-11ea-823b-a681fe4ea015.JPG)


## 동기식 호출 

![동기식](https://user-images.githubusercontent.com/45332921/93432665-d2318b80-f900-11ea-8b0e-0b2806b09aa3.JPG)


## 폴리글랏

![폴리글랏](https://user-images.githubusercontent.com/45332921/93432751-eaa1a600-f900-11ea-81d8-4397053cc48a.JPG)


## 서킷 브레이킹

![섯킷브레이커](https://user-images.githubusercontent.com/45332921/93432533-a31b1a00-f900-11ea-8c1b-de2069a6a2a1.jpg)

## 오토스케일 아웃

![HPA 1](https://user-images.githubusercontent.com/45332921/93432499-9ac2df00-f900-11ea-8e75-95202b3b07f3.jpg)
![HPA 2](https://user-images.githubusercontent.com/45332921/93432506-9b5b7580-f900-11ea-95ab-02e1bfe8f58d.jpg)
![HPA 3](https://user-images.githubusercontent.com/45332921/93432507-9bf40c00-f900-11ea-98da-0f4f1adc6e81.jpg)
