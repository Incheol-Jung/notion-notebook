객관식 문제
Q1. Kafka 컨슈머의 _consumer_offsets 토픽의 주된 역할은?
① 프로듀서의 메시지를 저장한다
✅ ② 컨슈머가 읽은 메시지 위치(오프셋)를 저장한다 ← 정답
③ 파티션을 복제한다
④ 토픽의 파티션 수를 관리한다
👉 해설: _consumer_offsets는 컨슈머의 읽은 위치를 저장해 장애 복구 시 사용합니다.

Q2. 컨슈머 그룹에서 리더 컨슈머의 역할로 올바른 것은?
① 오프셋을 직접 관리한다
✅ ② 파티션 할당 전략을 실행하여 멤버들에게 할당 정보를 배포한다 ← 정답
③ 모든 파티션의 메시지를 직접 읽는다
④ 프로듀서의 트랜잭션을 커밋한다
👉 해설: 리더 컨슈머는 그룹 코디네이터와 통신하며 파티션 할당을 결정합니다.

Q3. 다음 중 Kafka에서 지원하는 파티션 할당 전략이 아닌 것은?
① Range Assigner
② Sticky Assigner
✅ ③ Priority Assigner ← 정답
④ Round Robin Assigner
👉 해설: Priority Assigner는 기본 제공되지 않으며 Range/Sticky/Round Robin/Cooperative Sticky가 기본 전략입니다.

Q4. Sticky Assigner 전략의 주된 목적은? (복수 선택)
✅ ① 리밸런싱 시 기존 파티션 매핑 최대 유지
② 컨슈머 간 파티션 불균형 극대화
✅ ③ 데이터 이동 최소화
④ 모든 파티션을 강제 재할당
👉 해설: Sticky는 불필요한 데이터 이동을 최소화해 불필요한 리밸런싱을 방지합니다.

Q5. Cooperative Sticky Assigner가 Eager 리밸런싱 방식보다 유리한 이유로 적절한 것은?
① 파티션 할당 변경 시 전체 그룹 리밸런싱을 동시에 수행한다
✅ ② 다운타임 없이 점진적으로 파티션을 재할당한다 ← 정답
③ 모든 컨슈머의 파티션을 무조건 제거한다
④ 컨슈머의 오프셋 저장을 비활성화한다
👉 해설: Cooperative는 일부 파티션만 점진 이동 → LAG 최소화.

Q6. 다음 중 컨슈머가 리밸런싱 상태를 유지하도록 도와주는 하트비트 옵션이 아닌 것은?
① heartbeat.interval.ms
② session.timeout.ms
③ max.poll.interval.ms
✅ ④ offsets.retention.minutes ← 정답
👉 해설: offsets.retention.minutes는 오프셋 보존 기간 옵션이며 하트비트와는 무관.

Q7. Kafka에서 메시지의 중복 처리 방지와 정확히 한 번 전송을 위해 사용하는 프로듀서 설정은?
✅ ① idempotent producer ← 정답
② consumer rebalance
③ sticky assignor
④ bootstrap server
👉 해설: Idempotent producer 옵션은 정확히 한 번 쓰기를 보장합니다.

Q8. Range Assigner 파티션 전략의 특징으로 올바른 것은? (복수 선택)
✅ ① 동일 토픽에서 연속된 파티션을 할당한다
✅ ② 파티션 수와 컨슈머 수가 다르면 일부 불균형이 생길 수 있다
③ 라운드 로빈보다 균등한 분배가 보장된다
✅ ④ 멀티 토픽 조합에서는 불균형이 발생할 수 있다
👉 해설: Range는 연속 구간 할당이라 멀티 토픽에서 불균형 가능성 있음.

Q9. 다음 중 리밸런싱이 과도하게 발생하면 실무에서 초래될 수 있는 문제로 적절하지 않은 것은?
① 메시지 LAG 증가
② 소비 중단에 따른 처리 지연
✅ ③ 트래픽 비용 절감 ← 정답
④ 시스템 부하 증가
👉 해설: 리밸런싱은 오히려 비용과 부하를 증가시키므로 트래픽 절감 효과는 없습니다.

Q10. 아래 중 Exactly Once Semantics(EOS)를 위해 반드시 고려해야 하는 요소는?
✅ ① 오프셋 커밋 시점 관리 ← 정답
② 파티션 replication factor
③ 하트비트 간격
④ 컨슈머 그룹 ID
👉 해설: 정확히 한 번 처리의 핵심은 오프셋 커밋과 트랜잭션 처리의 원자성 유지입니다.

✅ 📝 주관식 문제 (5문제 + 모범 답안)
Q1.
Q: Kafka 컨슈머 그룹이 리밸런싱 없이 안정적으로 유지되도록 session.timeout.ms와 heartbeat.interval.ms는 어떻게 설정해야 하나요?
A: heartbeat.interval.ms는 session.timeout.ms의 1/3 이하로 설정하여 하트비트가 끊기지 않도록 함. 너무 짧으면 오버헤드, 너무 길면 장애 감지 지연.

Q2.
Q: Range Assigner와 Round Robin Assigner 파티션 할당 전략은 어떤 환경에서 각각 더 유리한가요?
A: Range는 단일 토픽에서 연속 파티션 할당에 유리하며 키 순서를 유지하기 좋음. Round Robin은 멀티 토픽에서 균등 분배가 중요할 때 유리함.

Q3.
Q: Cooperative Sticky 리밸런싱 전략이 다운타임을 최소화하는 원리를 단계적으로 설명해보세요.
A:

새로운 컨슈머 조인 감지 → 리밸런싱 트리거

변경 필요한 일부 파티션만 제외

제외된 파티션만 점진적으로 재할당

전체 할당 재조정 없이 단계적 이동 → 소비 중단 최소화

Q4.
Q: Kafka에서 컨슈머 장애 발생 시 오프셋 관리와 리밸런싱 관점에서 주의할 점은?
A: _consumer_offsets에 정확히 커밋되어야 하고 장애 복구 시 마지막 읽은 위치부터 재시작. 불필요한 리밸런싱 방지를 위해 스티키/협력적 스티키 전략 사용 권장.

Q5.
Q: Exactly Once 처리를 위해 컨슈머 측에서 고려해야 할 설정이나 코드 예시를 쓰세요.
A: 트랜잭션 사용, idempotent producer 활성화, enable.auto.commit=false로 오프셋 수동 커밋, 프로세싱-커밋을 하나의 트랜잭션으로 묶음.

