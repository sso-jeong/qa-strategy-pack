# Release Go / No-Go Report (anonymized)

> 목적: 배포 전후 품질 상태를 종합 판단하여 배포 진행/중단을 결정한다.  
> 연결: Test Plan Exit Criteria + Risk Matrix High 항목 + Smoke 결과

## 0. Basic Info
- Service:
- Release/Version:
- Date/Time:
- Environment: STG / PROD
- QA Owner:
- Approver (승인자):

---

## 1. Change Summary (이번 변경 요약)
- 주요 변경:
  - (예) Auth 토큰 갱신 로직 변경(R1)
  - (예) 배포 설정 변경(R2)
  - (예) Web 라우팅 변경(R3)

---

## 2. Risk Summary (Risk Matrix 기반)
| Risk ID | Level | Risk | Test Coverage (어떻게 커버했나) | Result | Evidence |
|---|---|---|---|---|---|
| R1 | M | 인증/세션 만료 | 토큰 만료/갱신/에러코드/로그 |  |  |
| R2 | H | 배포 설정 누락 | smoke + rollback 시나리오 검증 |  |  |
| R3 | M | 라우팅/권한 | 핵심 화면 smoke + 네트워크/콘솔 |  |  |

---

## 3. Test Execution Summary
### 3.1 Smoke (필수)
- Result: PASS / FAIL
- Scope: health + 핵심 API 5개 (+ UI smoke if needed)
- Evidence: (링크/파일)

### 3.2 Regression (자동화/수동)
- Newman Regression:
  - Total: __ / Pass: __ / Fail: __ / Pass rate: __%
  - Evidence: HTML report / CI run link
- Manual Regression (있다면):
  - Scope: (예) 결제 생성/조회 등
  - Result: PASS / FAIL

### 3.3 Performance (optional)
- 실행 여부: Yes/No
- 조건: (예) __ RPS, __ min
- 결과: (p95 latency / error rate)

---

## 4. Defect Status (Sev 기준)
| Sev | Open | Fixed | Deferred | Notes |
|---:|---:|---:|---:|---|
| Sev1 | 0 |  |  | Sev1은 0건이어야 함 |
| Sev2 |  |  |  | 오픈 시 승인 필요 |
| Sev3 |  |  |  | 배포 가능하나 기록 |

- 핵심 오픈 이슈 요약(있다면):
  - (예) Sev2: 특정 환경에서 간헐적 500, 우회책 __, 영향도 __

---

## 5. Exit Criteria Check (Test Plan 기준)
| Criteria | Target | Actual | Pass/Fail | Evidence |
|---|---|---|---|---|
| Sev1 0건 | 0 |  |  |  |
| Sev2 오픈 시 승인 | 승인 필요 |  |  |  |
| Smoke 100% PASS | 100% |  |  |  |
| Newman 핵심 100% PASS | 100% |  |  |  |
| 전체 Pass율 | ≥95% |  |  |  |
| 롤백 준비 완료 | Yes |  |  |  |

---

## 6. Decision
- Decision: **GO / NO-GO**
- Rationale (왜 그렇게 판단했나):
  - (예) Exit Criteria 모두 충족 → GO
  - (예) Sev1 발생 / Smoke 실패 → NO-GO 및 롤백 진행

## 7. Action Items
- 배포 후 모니터링 포인트:
- 추가 검증 필요 항목:
- 다음 릴리즈 개선점:
