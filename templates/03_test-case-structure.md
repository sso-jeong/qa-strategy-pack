# BVT / Test Case Structure (Excel/Jira style)

> 목적
>- 테스트케이스를 “누가 실행해도” 같은 기준으로 PASS/FAIL 판정하고 **재현 가능한 Evidence**를 남기기 위함이다.
>- 엑셀/Jira에서 흔히 발생하는 문제(구두 공유/로그 누락/환경 정보 누락)를 방지하고 릴리즈/배포 판단에 바로 사용할 수 있는 형태로 정리한다.
>연결
>- `Risk Matrix`의 Risk ID(R1/R2/R3...)는 테스트케이스에 매핑되어 왜 이 케이스를 해야 하는지 근거가 된다.
>- `Test Plan`의 In Scope/Exit Criteria는 P0(BVT/Smoke) 케이스 선정 기준이 되고 최종 PASS율/Smoke 결과는 `release-go-no-go`로 요약된다.
>- 케이스 실패(특히 R2 배포설정, R1 인증)는 즉시 `rollback-playbook`의 Trigger 조건으로 이어진다.

## 1. Purpose
- 대분류/중분류/소분류: 기능 영역을 안정적으로 분류해 누락 없이 커버리지를 확인하기 위함
- Expected(판정 기준): 느낌이 아니라 **측정 가능한 조건(상태코드/로그키워드/예외명/DB반영)** 으로 PASS/FAIL을 결정하기 위함
- Steps/Request/Command: 같은 환경에서 반복 실행 가능하도록 재현 절차를 남기기 위함
- Evidence: 긴 로그/페이로드는 첨부(파일/링크)로 남기고, 본문은 요약만 남겨 유지보수성을 높이기 위함

---

## 2. When to use
- **BVT / Smoke**: 신규 바이너리/패치 적용 후 최초 정상 동작 확인
- **Regression(부분/전체)**: 핵심 시나리오 재검증(Newman/Jenkins 등)
- **옵션/조합 테스트**: CLI/Batch/런타임 옵션 조합( shutdown, timebased 등) 검증

---

## 3. Common writing rules (공통 작성 규칙)
	1)테스트 케이스명/Title은 동작+조건_기대결과가 드러나게 작성
		예) `REST API DELETE 호출 시 삭제 반영 확인`
	2)Expected(기대값)은 판정 가능한 기준으로 작성
		예) `HTTP 200/204`, `특정 로그 키워드 존재`, `예외명(ServiceTimeOutException)`
	3) Actual(결과값)은 요약만 작성하고 긴 내용은 Evidence로 분리  
	4) 민감정보(IP/계정/내부 서비스명)는 `{BASE_URL}`, `{APP}`, `{SERVICE}` 등 placeholder로 익명화  
	5)케이스는 “1 row = 1 검증 단위” (조건이 달라지면 케이스를 분리)

---

## 4. BVT (Basic Function List) Excel structure
> 실무에서 운영하던 기본 기능 리스트 형태의 표준 컬럼 정의

### 4.1 BVT Columns (추천 표준 컬럼)
| Column                | Meaning                                  |
| --------------------- | ---------------------------------------- |
| No.                   | 일련번호 (001~)                              |
| 대분류                   | 기능 큰 범주 (예: Protocol/DB/Timeout/Logging) |
| 중분류                   | 기능 중간 범주 (예: HTTP/WebSocket/Datasource)  |
| 소분류                   | 상세 범주 (예: RESTful/JSON Payload/Oracle)   |
| 테스트 케이스명(Title)       | 무엇을 검증하는지 한 줄                            |
| Priority              | P0(필수) / P1(권장) / P2(시간되면)               |
| Preconditions/Setup   | 사전 조건/설정/준비물                             |
| Steps/Request/Command | 실행 절차 (요청/커맨드 요약)                        |
| Expected (판정 기준)      | status/로그/예외/DB반영 등 “판정 가능”하게            |
| 실행결과(Status)          | PASS/FAIL/BLOCKED/SKIP                   |
| 결과값(Actual 요약)        | 실제 관찰 요약(짧게)                             |
| Evidence              | 로그/스크린샷/리포트 링크 또는 파일 경로                  |
| 자동화 여부                | Manual / Candidate / Automated           |
| 실행방식(Tool)            | Postman / WASP / HTML / CLI 등            |
| 비고                    | 제약/특이사항                                  |

### 4.2 BVT Example rows (anonymized)
| No. | 대분류 | 중분류 | 소분류 | 테스트 케이스명 | Priority | Preconditions/Setup | Steps/Request/Command | Expected (판정 기준) | Status | Actual(요약) | Evidence | Automation | Tool | 비고 |
|---:|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 001 | 프로토콜 | HTTP | RESTful | REST API DELETE 호출 시 삭제 반영 확인 | P0 | `{BASE_URL}` 접근 가능 | `DELETE /{app}/{sg}/empno/{id}` (Postman) | `200/204` + 삭제 반영 + error log 없음 |  |  |  | Manual | Postman |  |
| 006 | 프로토콜 | HTTP | JSON Payload | Content-Type: application/json 호출 성공 확인 | P0 | 헤더 설정 | `POST /{service}` body `{dto:{}}` | 응답 success + 로그 `Payload success` |  |  |  | Candidate | WASP |  |
| 015 | Timeout | 시스템 | SYSTEM_TIMEOUT | 서비스 타임아웃 시 예외 발생 확인 | P0 | timeout 값 설정 | 옵션 적용 후 지연 유도 | `ServiceTimeOutException` 발생 + 로그 확인 |  |  |  | Manual | WASP |  |
