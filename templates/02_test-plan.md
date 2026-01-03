# Test Plan (Release Verification Plan)

>목적
>- 이번 릴리즈에서 **무엇을/어떻게/언제까지** 검증할지 합의하고 배포 판단을 위한 기준(Exit Criteria)을 명확히 한다.
>- 리스크 기반(Risk Matrix)으로 범위와 우선순위를 정하고 팀이 **같은 기준으로 Go/No-Go를 결정**할 수 있게 한다.
>
>연결
>- 입력: `Risk Matrix` (R1~Rn) → In Scope / Test Strategy / Smoke 대상 선정
>- 실행: `Test Case Structure` 규칙으로 케이스 작성 → BVT/Regression/옵션조합 테스트 수행
>- 출력:
>	배포 직후 검증은 `smoke-checklist.md`로 수행하고 Evidence를 남긴다.
	  Exit Criteria 충족 여부는 `release-go-no-go.md`에서 체크한다.
	  실패 시 대응은 `rollback-playbook.md` 기준으로 롤백/중단을 진행한다.

## 1. Basic Info
- Service:
- Release/Version:
- 기간:
- QA Owner:
- Environments: DEV / STG / PROD

## 2. Scope
### In Scope
- 이번 배포에서 변경된 기능/핵심 플로우
- (예) 로그인 토큰 갱신, 결제 생성 API

### Out of Scope
- 시간/환경/권한 때문에 이번에 제외하는 범위
- (예) 어드민 화면 UI 전체 회귀는 제외, 성능 테스트는 이번 배포에서는 미실시

## 3. Test Strategy
- Functional(기능 테스트): 정상/예외/에러코드 등 / API 중심 / 필요한 경우 UI
- Regression(회귀 테스트): 기존 기능이 깨졌는지 확인 / 핵심 시나리오 우선
- Smoke: 배포 직후 필수 / 스모크 자동 실행
- Performance(optional): 필요 시 부하/성능 측정 조건

## 4. Test Data / Environment
- 테스트 계정/권한:
- 의존 시스템: (예) 인증 서버, DB, 외부 API 등
- 환경 체크 포인트(설정/권한/DB스키마 등):

## 5. Defect Policy
- 버그를 어떤 기준으로 처리할 것인지
- Severity 기준:
	- Sev1: 서비스가 안 됨/데이터 망가짐(무조건 막음)
	- Sev2: 핵심 기능 심각 문제 (원칙적으로 막음)
	- Sev3: 불편/우회 가능 (배포 가능하되 기록)
- 재현 기준:
	- 재현 Steps
	- 기대/실제 결과
	- 로그/스크린샷/리포트
	- 환경정보(STG/버전/빌드번호)

## 6. Exit Criteria
- Sev1(서비스 불가/데이터 정합성) 0건
- Sev2(핵심 기능 영향) 오픈 시: 영향도/우회책 문서화 + 배포 승인 필요
- 배포 직후 Smoke(헬스체크 + 핵심 API 5개) 100% PASS
- 회귀 자동화(Newman) 핵심 시나리오 100% PASS / 전체 PASS율 95% 이상
- 롤백 절차 확인 및 담당자/커뮤니케이션 채널 확인 완료


