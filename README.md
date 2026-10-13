# GST-AI-Crew GitHub Repository Ruleset 운영 정책

| 항목 | 내용 |
|---|---|
| 문서명 | GitHub Repository Ruleset 운영 정책 |
| 적용 조직 | `GST-AI-Crew` |
| 관리 범위 | GitHub Enterprise `gst-cloud` |
| Ruleset | `DX - Naming & metadata` / `DX - Protected main/develop` |
| 상태 | `Active`
| 작성 기준일 | 2026-10-13 |
| 문서 상태 | 설정 내용 기반 초안 |

## 1. 목적

본 문서는 `GST-AI-Crew` 조직의 GitHub Repository 운영에 적용할 브랜치 관리, 커밋 메시지, Pull Request(PR), 코드 보호 및 예외 처리 기준을 정의한다.

주요 목적은 다음과 같다.

- 기본 브랜치에 대한 직접 변경을 통제하고 PR 중심의 변경 이력을 관리한다.
- 브랜치명과 커밋 메시지 형식을 표준화한다.
- 강제 푸시 및 브랜치 삭제로 인한 코드·이력 손상을 방지한다.
- 코드 검토 및 자동 검사 기준을 조직 전체에 일관되게 적용한다.

## 2. 브랜치 및 커밋 네이밍 표준

### 2.1 브랜치명 규칙

제공된 정규식:

```regex
^(main|develop|(feature|fix|hotfix|release)/[a-zA-Z0-9._-]+)$
```

허용되는 이름 예시:

- `main`
- `develop`
- `feature/login-api`
- `fix/token-expiry`
- `hotfix/payment-error`
- `release/v1.2.0`

허용되지 않는 이름 예시:

- `test`
- `feature/` (작업명 누락)
- `feature/login/api` (추가 `/` 포함)
- `Feature/login` (대문자 접두사)
- `bugfix/login` (허용되지 않는 접두사)

**운영 기준:** 일반 기능 개발은 `feature/`, 버그 수정은 `fix/`, 긴급 수정은 `hotfix/`, 배포 준비는 `release/`를 사용한다. `main`과 `develop`은 공유 브랜치로 관리한다.

### 2.2 커밋 메시지 규칙

제공된 정규식:

```regex
^(feat|fix|docs|refactor|test|chore)(\(.+\))?: .+
```

허용되는 메시지 예시:

```text
feat: 로그인 기능 추가
feat(auth): OAuth 로그인 구현
fix: 토큰 만료 오류 수정
docs: README 사용법 수정
refactor(api): 예외 처리 구조 개선
test: 사용자 서비스 단위 테스트 추가
chore: CI 설정 업데이트
```

허용되지 않는 메시지 예시:

```text
로그인 개발
add login
Feat: 로그인 기능 추가
feat 로그인 기능 추가
fix: 
```

사용 가능한 타입:

| 타입 | 목적 |
|---|---|
| `feat` | 신규 기능 |
| `fix` | 버그 수정 |
| `docs` | 문서 변경 |
| `refactor` | 기능 변경 없는 구조 개선 |
| `test` | 테스트 추가 또는 수정 |
| `chore` | 빌드·설정·유지보수 작업 |

> **참고:** 제공된 커밋 정규식은 끝에 `$`가 없으며, GitHub의 `must start with` 조건에 맞춘 패턴이다. 커밋 본문의 모든 추가 문자까지 엄격히 제한해야 한다면 검증 방식과 정규식을 별도로 검토한다. PR Merge 방식에 따라 자동 생성되는 Merge/Squash 커밋 메시지가 규칙을 만족하는지도 점검한다.

## 3. Pull Request 및 기본 브랜치 보호 정책

### 3.1 권장 개발 흐름

```text
main / develop
     ↓ 작업 브랜치 생성
feature/... 또는 fix/... 등
     ↓ 변경사항 Commit / Push
Pull Request 생성 → 리뷰·자동 검사(설정된 경우)
     ↓ 승인 및 조건 충족
main 또는 develop으로 Merge
```

권장 운영 방식:

1. 개발자는 기본 브랜치에서 직접 수정하지 않고 별도 작업 브랜치를 생성한다.
2. 커밋 메시지는 3.2의 표준에 따라 작성한다.
3. 대상 브랜치에 PR을 생성한다.
4. 적용된 리뷰 승인·대화 해결·상태 검사 조건을 충족한 뒤 병합한다.
5. 병합 완료 후 불필요한 작업 브랜치는 정리한다. 단, 삭제 제한 정책이 있다면 권한·정리 절차를 별도로 정한다.

## 4. Bypass 및 예외 관리

Bypass list는 비어 있다. 이는 명시적으로 등록된 예외 대상이 없음을 의미한다. 단, 실제 적용 범위와 사용자 권한, Ruleset 조합에 따른 효과는 테스트해야 한다.

- 원칙적으로 일반 개발자, 팀, 앱에 상시 Bypass를 부여하지 않는다.
- 긴급 장애 조치가 필요한 경우 Enterprise/Organization 운영 담당자가 필요성과 적용 범위를 검토한다.
- 예외가 꼭 필요하면 최소 범위로 부여하고, 사용 내역을 감사 로그로 확인한다.
- 상시 예외 권한이 필요한 CI/CD 앱은 목적·권한·담당자를 문서화한다.

## 5. 변경 관리

Ruleset 변경 시 다음 내용을 기록한다.

| 변경일 | 변경 대상 | 변경 내용 | 변경 사유 | 담당자 |
|---|---|---|---|---|
| 2026-10-13 | `DX - Protected main/develop` | main/develop 브랜치 보호 | 운영 표준 수립 | 노명욱 |
| 2026-10-13 | `DX - Naming & metadata` | commit/branch 명 강제 | 운영 표준 수립 | 노명욱 |

---

### 참조

- GitHub Enterprise: `https://github.com/enterprises/gst-cloud/settings/policies/code`
- GitHub 공식 문서: [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- GitHub 공식 문서: [Available rules for rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)

> 이 문서는 제공된 설정값에 기반한 운영 정책 **초안**이며, 활성 체크박스·Required approvals·Target repositories 값이 확인되면 확정본으로 갱신한다.
