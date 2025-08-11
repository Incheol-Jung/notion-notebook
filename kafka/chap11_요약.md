# 📚 카프카 커넥트 학습 요약 & 문제 세트

## 1. 요점 정리

### 📌 카프카 커넥트(Kafka Connect) 개요
- **오픈소스 프레임워크**로 외부 시스템(DB, 파일 등)과 카프카를 쉽게 연동.
- 대용량 데이터 이동 가능, 코드 작성 없이 간단히 사용.
- **REST API** 제공 → 설정 변경 및 모니터링 가능.
- **주요 장점**
  - 데이터 중심 파이프라인 구현
  - 유연성·확장성 (Standalone / Distributed Mode 지원)
  - 재사용성 및 기능 확장
  - 장애 복구 및 고가용성

---

### 📌 구성 요소
- **소스 커넥터(Source Connector)**: 외부 → 카프카로 데이터 전송 (프로듀서 역할)
- **싱크 커넥터(Sink Connector)**: 카프카 → 외부로 데이터 전송 (컨슈머 역할)
- **워커(Worker)**: 커넥트를 실행하는 프로세스 단위.
- **태스크(Task)**: 실제 데이터 이동 작업 단위.

---

### 📌 실행 모드
1. **단독 모드(Standalone Mode)**  
   - 하나의 인스턴스 실행  
   - 테스트·소규모 환경 적합  
   - 장애 시 자동 복구 불가

2. **분산 모드(Distributed Mode)**  
   - 여러 워커에서 실행, 태스크 자동 분산  
   - 장애 시 다른 워커로 태스크 이동(Fault Tolerance)  
   - 메타데이터를 카프카 내부 토픽에 저장  
   - 안정적, 운영 환경 적합

---

### 📌 설정 파일 주요 항목
- **connect-file-source.properties**
  - `name`: 커넥터 이름
  - `connector.class`: 클래스 지정
  - `tasks.max`: 최대 태스크 수
  - `file`: 읽을 파일 경로
  - `topic`: 전송할 토픽명
- **connect-standalone.properties**
  - `bootstrap.servers`: 브로커 주소
  - `key/value.converter`: 직렬화 방식
  - `offset.storage.file.filename`: 오프셋 저장 위치
  - `offset.flush.interval.ms`: 오프셋 저장 주기(ms)

---

### 📌 REST API 주요 명령
- GET `/connectors` : 커넥터 리스트 조회
- GET `/connectors/{name}/status` : 커넥터 상태 확인
- PUT `/connectors/{name}/config` : 설정 변경
- PUT `/connectors/{name}/pause` / `/resume` : 일시중지 / 재시작
- DELETE `/connectors/{name}` : 커넥터 삭제

---

### 📌 미러 메이커(Mirror Maker) 2.0
- **다중 클러스터 간 데이터 복제** 도구
- **양방향 복제 지원** (Active-Active 가능)
- **에일리어스(Alias)** 기능 → 동일 토픽명 복제 시 충돌 방지
- 장애 복구, 다중 데이터센터 운영에 활용

---

## 2. 객관식 문제 (단일·다중 혼합)

1. (단일) 카프카 커넥트의 주된 목적은 무엇인가?  
   A. 메시지 브로커 확장  
   B. 외부 시스템과의 데이터 연동  
   C. 데이터 압축  
   D. 토픽 파티션 조정  

2. (다중) 카프카 커넥트의 장점에 해당하는 것은?  
   A. 코드 작성 불필요  
   B. REST API 지원  
   C. 수동 데이터 전송  
   D. 운영 오버헤드 증가  

4. (다중) 단독 모드의 특징을 모두 고르시오.  
   A. 하나의 인스턴스 동작  
   B. 테스트 환경 적합  
   C. 자동 장애 복구  
   D. 메타데이터 카프카 저장  

5. (단일) 소스 커넥터의 역할은?  
   A. 카프카에서 외부로 데이터 전송  
   B. 외부에서 카프카로 데이터 전송  
   C. 데이터 포맷 변환  
   D. REST API 호출  

6. (단일) Sink 커넥터의 역할은?  
   A. 데이터 압축  
   B. 외부로 데이터 전송  
   C. 데이터 직렬화  
   D. 태스크 관리  

7. (다중) 분산 모드의 특징을 모두 고르시오.  
   A. 여러 워커 실행  
   B. Fault Tolerance  
   C. 태스크 자동 재배치  
   D. 하나의 서버만 사용  

8. (단일) `tasks.max` 설정은 무엇을 의미하는가?  
   A. 브로커 수  
   B. 최대 태스크 수  
   C. 파티션 수  
   D. 메타데이터 크기  

9. (단일) `offset.storage.file.filename`은 어떤 경로를 지정하는가?  
   A. 소스 데이터 저장소  
   B. 로그 파일  
   C. 오프셋 저장 위치  
   D. 설정 백업 위치  

10. (다중) 커넥터 설정 파일에 포함될 수 있는 항목은?  
    A. name  
    B. connector.class  
    C. bootstrap.servers  
    D. ping.interval  

11. (단일) REST API 명령 중 커넥터 일시 중지를 수행하는 것은?  
    A. `/pause`  
    B. `/resume`  
    C. `/stop`  
    D. `/disable`  

12. (다중) REST API를 사용할 때 필수로 포함해야 하는 헤더는?  
    A. Content-Type  
    B. Accept  
    C. Hostname  
    D. application/json  

13. (단일) 미러 메이커 2.0의 특징이 아닌 것은?  
    A. 양방향 복제 가능  
    B. 단방향 복제만 가능  
    C. 에일리어스 기능 지원  
    D. 장애 복구 활용 가능  

14. (단일) 카프카 커넥트에서 데이터 직렬화를 담당하는 구성요소는?  
    A. 컨버터(Converter)  
    B. 워커(Worker)  
    C. 태스크(Task)  
    D. 토픽(Topic)  

15. (다중) 미러 메이커 2.0에서 에일리어스 기능의 목적은?  
    A. 동일 토픽명 구분  
    B. 데이터 압축  
    C. 파티션 개수 조정  
    D. 직렬화 방식 변경  

16. (다중) 컨버터의 역할에 해당하는 것은?  
    A. 직렬화  
    B. 역직렬화  
    C. 토픽 생성  
    D. 태스크 스케줄링  

17. (단일) 분산 모드 필수 조건은?  
    A. 최소 2대 워커  
    B. 최소 3대 브로커  
    C. 최소 1대 서버  
    D. 메타데이터 저장소  

18. (다중) 미러 메이커 2.0의 활용 사례는?  
    A. 데이터센터 간 복제  
    B. 다중 클러스터 운영  
    C. 토픽명 암호화  
    D. 장애 복구  

19. (다중) 카프카 커넥트에서 태스크와 관련된 설명으로 옳은 것?  
    A. 데이터 전송의 최소 단위  
    B. 워커 내부에서 실행  
    C. 커넥터의 하위 단위  
    D. 브로커 관리 기능  

20. (단일) Fault Tolerance의 의미로 옳은 것은?  
    A. 데이터 암호화 방식  
    B. 장애 발생 시 작업 지속 가능  
    C. 네트워크 대역폭 절감  
    D. 파일 크기 압축  

---

## 3. 주관식 문제
1. 카프카 커넥트에서 소스 커넥터와 싱크 커넥터의 차이점을 설명하시오.  
2. Standalone Mode와 Distributed Mode의 주요 차이점을 2가지 이상 서술하시오.  
3. Fault Tolerance란 무엇이며, 카프카 커넥트에서 어떻게 구현되는지 설명하시오.  
5. `offset.flush.interval.ms` 설정의 의미를 설명하시오.  
6. 카프카 커넥트에서 REST API를 통해 할 수 있는 작업 3가지를 쓰시오.  
7. 미러 메이커 2.0에서 에일리어스 기능이 필요한 이유를 설명하시오.  
8. 커넥터 설정 시 `tasks.max`를 늘리는 것이 어떤 영향을 미치는지 설명하시오.  
9. 분산 모드에서 워커 하나가 장애를 일으켰을 때 태스크는 어떻게 동작하는지 설명하시오.  
10. 카프카 커넥트에서 컨버터가 중요한 이유를 설명하시오.

---

## 4. 정답 및 해설

### 🔹 객관식
1. **B** – 외부 시스템과의 데이터 연동  
2. **A, B** – 코드 작성 불필요, REST API 지원  
3. **C** – 하나의 인스턴스에서 동작  
4. **A, B** – 하나의 인스턴스, 테스트 환경 적합  
5. **B** – 외부에서 카프카로 데이터 전송  
6. **B** – 카프카에서 외부로 데이터 전송  
7. **A, B, C** – 여러 워커, Fault Tolerance, 자동 재배치  
8. **B** – 최대 태스크 수 지정  
9. **C** – 오프셋 저장 위치  
10. **A, B, C** – name, connector.class, bootstrap.servers  
11. **A** – `/pause` 명령  
12. **A, B, D** – Content-Type, Accept, application/json  
13. **B** – 단방향 복제만 가능은 오답  
14. **A** – 컨버터가 직렬화 담당  
15. **A** – 동일 토픽명 구분  
16. **A, B** – 직렬화/역직렬화  
17. **A** – 최소 2대 워커  
18. **A, B, D** – 데이터센터 복제, 다중 클러스터, 장애 복구  
19. **A, B, C** – 태스크는 데이터 전송 최소 단위, 워커 내부 실행, 커넥터 하위 단위  
20. **B** – 장애 발생 시 작업 지속 가능

---

### 🔹 주관식 모범답안
1. **소스 커넥터**: 외부 시스템 → 카프카, **싱크 커넥터**: 카프카 → 외부 시스템  
2. Standalone: 하나의 인스턴스, 장애 복구 불가 / Distributed: 여러 워커, 장애 복구 가능  
3. 장애 시 태스크를 다른 워커로 자동 재배치하는 기능  
4. 소스 데이터가 저장될 카프카 토픽명 지정  
5. 오프셋 저장 주기를 밀리초 단위로 지정  
6. 커넥터 상태 조회, 일시정지, 삭제  
7. 동일 토픽명 복제 시 충돌 방지  
8. 병렬 처리량 증가, 처리 속도 향상  
9. 장애 워커의 태스크를 다른 워커로 재배치해 실행  
10. 다양한 포맷 직렬화/역직렬화 지원으로 호환성 확보


# 카프카 커넥트 활용 사례
- https://helloworld.kurly.com/blog/kafka-connect-pipeline/
- https://blog.soomgo.com/posts/6673bb8c52107866fb86a782?source=related_content
- https://techblog.uplus.co.kr/debezium%EC%9C%BC%EB%A1%9C-db-synchronization-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0-1b6fba73010f
- https://techblog.woowahan.com/10000/
- https://tech.kakaopay.com/post/kakaopaysec-mongodb-cdc/
