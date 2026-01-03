# Smoke Checklist (anonymized)

> 목적: 배포 직후 서비스 살아있음 + 핵심 플로우 동작을 빠르게 확인한다.  
> 원칙: High 리스크(Risk Matrix) 항목과 핵심 API/화면을 우선 검증하고 Evidence를 남긴다.

## 0. Basic Info
- Service:
- Release/Version:
- Date/Time:
- QA Owner:
- Environment: DEV / STG / PROD
- Deploy type: (예) rolling / blue-green / canary / manual
- Related Risk IDs: (예) R2, R3

---

## 1. Pre-check (배포 전/직후 점검)
| No | Check | Method | Expected | Result | Evidence |
|---:|---|---|---|---|---|
| P1 | 배포 대상/버전 확인 | 배포 공지/Jenkins/릴리즈 노트 | 대상 버전 일치 |  |  |
| P2 | 환경 설정 반영 확인(R2) | config/secret/env var 확인 | 누락 없음 |  |  |
| P3 | 의존 시스템 상태 | DB/Redis/Auth/외부 API ping | 정상 응답 |  |  |

---

## 2. Health Check (필수)
| No | Check | Request/Steps | Expected | Result | Evidence |
|---:|---|---|---|---|---|
| H1 | 헬스 엔드포인트 | `GET /health` | 200 + status UP |  |  |
| H2 | 로그 에러 급증 확인 | 로그 검색 | ERROR 폭증 없음 |  |  |
| H3 | 장애 지표 확인(가능 시) | 모니터링/대시보드 | 5xx 급증 없음 |  |  |

---

## 3. Core API Smoke (핵심 API 5개)
> Test Plan Exit Criteria와 연결: 핵심 API 5개 100% PASS

| No | API | Request | Expected (판정 기준) | Result | Evidence |
|---:|---|---|---|---|---|
| A1 | Auth 토큰 발급/갱신 (R1) | `POST /auth/token` / `POST /auth/refresh` | 200 + 토큰 재발급 + 만료/에러코드 정상 |  |  |
| A2 | 핵심 조회 API | `GET /resource/{id}` | 200 + 응답 스키마 정상 |  |  |
| A3 | 생성/변경 API | `POST /resource` | 201/200 + DB 반영 |  |  |
| A4 | 권한/인가 확인 | 권한 없는 계정 호출 | 401/403 정상 |  |  |
| A5 | 에러 처리(음수 케이스) | 필수값 누락 | 400 + 에러코드/메시지 규격 |  |  |

---

## 4. UI Smoke (선택, R3 있을 때 필수)
| No | Screen | Steps | Expected | Result | Evidence |
|---:|---|---|---|---|---|
| U1 | 메인/진입 | URL 접속 | 200 + 화면 렌더링 |  |  |
| U2 | 라우팅/404 | 존재하지 않는 경로 | 404 페이지 정상 |  |  |
| U3 | 로그인 | 로그인 시도 | 성공/실패 처리 정상 |  |  |
| U4 | 권한 페이지 | 권한 없는 접근 | 접근 차단/안내 정상 |  |  |
| U5 | 콘솔/네트워크 에러 | devtools 확인 | 치명적 에러 없음 |  |  |

---

## 5. Post-check (최종)
- Smoke 결과: PASS / FAIL
- Fail 시 조치:
  - 즉시 공유 채널에 알림(아래 커뮤니케이션 섹션 참조)
  - 롤백 필요 여부 판단 → `rollback-playbook.md` 실행

## 6. Communication
- Release channel:
- On-call / 담당자:
- Rollback 승인자: