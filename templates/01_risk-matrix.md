# Risk Matrix (Risk-based Prioritization)

>목적
>- 이번 릴리즈의 **변경점(Change)** 을 기준으로 어디가 망가지기 쉬운지(Failure Mode)를 먼저 식별한다.
>- 리스크 점수(Impact x Likelihood)로 **테스트 우선순위(P0/P1/P2)** 와 집중 영역을 결정한다.
>- High 리스크는 반드시 **Smoke/BVT에 포함**하고, 배포 판단(Go/No-Go)과 롤백 준비까지 연결한다.
> 
> 연결
> - Risk Matrix의 **High/Medium 항목** → `Test Plan`의 **In Scope / Test Strategy**로 들어간다.
> - 각 Risk ID(R1/R2/R3)는 → `BVT/Smoke 케이스`에 **Risk ID 태그로 매핑**된다.
> - High 항목(예: R2 배포설정)은 → `smoke-checklist` + `rollback-playbook`에 **필수로 반영**된다.
> - 최종적으로 Risk별 결과는 → `release-go-no-go`의 **Risk Summary 표**에서 PASS/FAIL로 요약된다.
> 
> 사용 가이드
> - 릴리즈마다 최소 3~10개 리스크를 뽑고 **High는 0개가 되지 않게**(배포 변경은 항상 리스크가 있음) 작성한다.
> - Risk ID를 테스트케이스에도 적어 왜 이 케이스를 했는지 근거를 남긴다.

## Scoring
- Impact(1~3), Likelihood(1~3), Score=Impact*Likelihood
- 7~9 High / 4~6 Medium / 1~3 Low

| ID  | Change/Area      | Failure Mode       | Impact | Likelihood | Score | Level | Test Focus               |
| --- | ---------------- | ------------------ | -----: | ---------: | ----: | ----- | ------------------------ |
| R1  | Auth/API 변경      | 인증 실패/세션 만료        |      3 |          2 |     6 | M     | 만료/갱신/에러코드/로그            |
| R2  | 배포 설정 변경         | 환경별 설정 누락          |      3 |          3 |     9 | H     | smoke + rollback 검증      |
| R3  | Web UI 배포/라우팅 변경 | 화면 진입 불가/404/권한 오류 |      2 |          2 |     4 | M     | 핵심 화면 smoke + 콘솔/네트워크 확인 |
- Change/Area: 이번에 바뀐 기능/영역
	- 예) 인증 로직 변경, 배포 설정 변경, 결제 API 수정 등
- Failure Mode: 그게 망가지면 어떤 문제가 생기나
	- 예) 로그인 안됨, 세션이 자꾸 끊김,  환경에서만 500 터짐
- Impact(영향도): 터지면 얼마나 큰 사고가 나는지
	- 3: 서비스 핵심 기능 불가 / 장애급
	- 2: 일부 기능 불편 / 우회 가능
	- 1: 사소한 UI/문구
- Likelihood(발생 가능성): 터질 확률
	- 3: 자주 터지거나 변경 범위 큼 / 의존성 많음
	- 2: 중간
	- 1: 거의 안터짐
- Score: Impact x Likelihood
- Level(우선순위 등급): High/Medium/Low
- Test Focus: 테스트 집중 대상
