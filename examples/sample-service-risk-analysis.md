# Sample Service Risk Analysis (anonymized)

- Service: {RUNTIME_SERVICE}
- Release/Version: 21.2.0.0.31
- Environment: STG
- Date: 2026-01-03
- QA Owner: {YOU}

## Change Summary (이번 변경점)
- Auth/API: 토큰 만료/갱신 로직 변경 및 에러코드 정리
- Deploy config: 환경변수/설정 키 변경(추가/삭제 포함)
- Web UI: 라우팅 경로 변경 및 권한 체크 로직 수정

## Risk Matrix
Scoring: Impact(1~3), Likelihood(1~3), Score=Impact*Likelihood

| ID | Change/Area | Failure Mode | Impact | Likelihood | Score | Level | Test Focus |
|---|---|---|---:|---:|---:|---|---|
| R1 | Auth/API 변경 | 인증 실패 / 세션 만료 시 무한 재시도 | 3 | 2 | 6 | M | 만료/갱신/에러코드/로그 |
| R2 | 배포 설정 변경 | 환경별 설정 누락으로 기능 불능/500 발생 | 3 | 3 | 9 | H | Smoke + 롤백 준비 검증 |
| R3 | Web UI 라우팅/권한 변경 | 핵심 화면 진입 불가(404/403) | 2 | 2 | 4 | M | 핵심 화면 Smoke + 콘솔/네트워크 |

## Mapping (Risk → Test)
- R1 → BVT 케이스: `AUTH-REFRESH`, `AUTH-EXPIRED`, `AUTH-UNAUTHORIZED`
- R2 → Smoke: `Config/Secret`, `Health`, `Core API 5개` + Rollback Trigger 점검
- R3 → UI Smoke: `Main 진입`, `Login`, `권한 페이지`, `404 처리`, `콘솔 에러`

## Notes
- 본 샘플은 실무에서 Excel/Jira로 관리하던 방식의 문서를 익명화하여 재구성한 예시이다.

