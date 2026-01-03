# Sample Release Go / No-Go Report (anonymized)

- Service: {RUNTIME_SERVICE}
- Release/Version: 21.2.0.0.31
- Environment: STG
- Date/Time: 2026-01-03 14:00
- QA Owner: {YOU}
- Approver: {LEAD/PM}

---

## 1) Change Summary
- R1: Auth 토큰 만료/갱신 로직 변경 및 에러코드 정리
- R2: 배포 설정 키 변경 및 환경변수 추가/삭제
- R3: Web UI 라우팅/권한 체크 수정

---

## 2) Risk Summary (Risk Matrix 기반)
| Risk ID | Level | Risk | Test Coverage | Result | Evidence |
|---|---|---|---|---|---|
| R1 | M | 인증/세션 만료 | 토큰 발급/만료/갱신/401/로그 확인 | PASS | evidence/auth-refresh.log |
| R2 | H | 배포 설정 누락 | config 체크 + health + core API smoke + rollback trigger 확인 | PASS | evidence/smoke-stg.md |
| R3 | M | 라우팅/권한 | 핵심 화면 진입/404/권한/콘솔 확인 | PASS | evidence/ui-smoke.png |

---

## 3) Test Execution Summary

### 3.1 Smoke (필수)
- Scope: Health + Core API 5개 + (R3 포함 시 UI Smoke)
- Result: PASS
- Evidence: evidence/smoke-stg.md

### 3.2 Regression
- Newman Regression: 미실시(샘플) / 또는 실행했다면 아래처럼
  - Total: 20 / Pass: 19 / Fail: 1 / Pass rate: 95%
  - Evidence: evidence/newman-report.html

---

## 4) Defect Status
| Sev | Open | Fixed | Deferred | Notes |
|---:|---:|---:|---:|---|
| Sev1 | 0 | 0 | 0 | Sev1은 0건 유지 |
| Sev2 | 0 | 1 | 0 | 수정 완료 |
| Sev3 | 2 | 0 | 2 | UI 문구/경미한 UX |

- 오픈 이슈(예):
  - Sev3: 특정 메시지 문구 오타(우회 가능, 다음 배포 반영)

---

## 5) Exit Criteria Check
| Criteria | Target | Actual | Pass/Fail | Evidence |
|---|---|---|---|---|
| Sev1 0건 | 0 | 0 | PASS | defect-summary |
| Smoke 100% PASS | 100% | 100% | PASS | evidence/smoke-stg.md |
| (선택) Newman 핵심 100% | 100% | 100% | PASS | evidence/newman-report.html |
| 전체 Pass율 | ≥95% | 95% | PASS | evidence/newman-report.html |
| 롤백 준비 완료 | Yes | Yes | PASS | rollback-playbook 확인 |

---

## 6) Decision
- Decision: **GO**
- Rationale:
  - Exit Criteria 충족
  - High Risk(R2) 영역 Smoke 및 설정 점검 통과
  - Sev1/Sev2 오픈 없음

## 7) Action Items
- 배포 후 모니터링:
  - Auth 관련 401/refresh 실패율
  - 5xx 및 latency(p95) 추이
- 다음 배포 개선:
  - Newman 회귀 자동화 범위 확대(핵심 시나리오 우선)

