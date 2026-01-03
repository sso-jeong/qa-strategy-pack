# Rollback Playbook (anonymized)

> 목적: 배포 실패/품질 기준 미달 시, 빠르고 안전하게 이전 버전으로 되돌린다.  
> 연결: Risk Matrix R2(배포 설정 변경) High 리스크 대응 문서

## 0. Basic Info
- Service:
- Current Release/Version:
- Target rollback version:
- Environment: STG / PROD
- Incident ID / Ticket:
- Decision maker (승인자):
- Executor (실행자):
- Start time / End time:

---

## 1. Rollback Trigger (언제 롤백하나)
아래 중 하나라도 만족하면 롤백 후보이며 상황에 따라 Go/No-Go 승인 필요
- Sev1 발생 (서비스 불가 / 데이터 정합성 훼손)
- Smoke 100% 실패 (Test Plan Exit Criteria 위반)
- 5xx 폭증/로그 에러 급증/핵심 플로우 중단
- 환경 설정 누락으로 기능 불능(R2)

---

## 2. Pre-rollback Checklist (되돌리기 전 확인)
| No | Item | Expected | Result | Evidence |
|---:|---|---|---|---|
| P1 | 장애 범위 파악 | 영향 사용자/기능 정리 |  |  |
| P2 | 데이터 영향 확인 | 데이터 마이그레이션/정합성 위험 여부 |  |  |
| P3 | 롤백 방식 확인 | 이전 artifact/image 준비됨 |  |  |
| P4 | 커뮤니케이션 | 채널/담당자/승인 확보 |  |  |

---

## 3. Rollback Steps (예시, 환경에 맞게 수정)
> 아래는 일반적인 순서 예시. 시스템에 맞게 명시해두면 됨

1) 배포 중지 / 트래픽 차단(필요 시)
- (예) canary 중단 / LB weight 0 / feature flag off

2) 이전 버전 배포 실행
- (예) Jenkins job: `deploy-rollback` 실행
- (예) 이미지 태그: `{previous_version}`

3) 설정/시크릿/환경변수 롤백
- 배포 설정 변경(R2)이 있었다면 반드시 함께 되돌리기

4) 서비스 재기동 및 헬스체크
- `GET /health` 확인
- 로그 에러 급증 여부 확인

5) Smoke 재실행(필수)
- `templates/smoke-checklist.md`의 Health + Core API 5개
- 모두 PASS 시 롤백 완료로 선언

---

## 4. Post-rollback Verification
| No | Check | Expected | Result | Evidence |
|---:|---|---|---|---|
| V1 | 헬스체크 | status UP |  |  |
| V2 | 핵심 API | 100% PASS |  |  |
| V3 | UI 핵심 화면 | 진입/로그인 정상 |  |  |
| V4 | 지표 안정화 | 5xx/latency 정상 범위 |  |  |

---

## 5. Documentation (사후 기록)
- 원인 요약(RCA 링크):
- 재발 방지 액션:
- 다음 배포 시 추가 검증 항목(Risk Matrix 업데이트):

