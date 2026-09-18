# CLAUDE.md

이 파일은 이 저장소에서 작업하는 Claude Code(claude.ai/code)에게 제공하는 안내 문서다.

## 소프트웨어 개발 정보
이 소프트웨어 개발은 다음의 항목을 이용해서 개발한다.
- Python 3.14를 이용해서 개발한다.
- 코드 테스트는 unittest를 활용한다.
- 순환복잡도, 함수라인수 등 측정 지표는 오픈소스 도구를 이용한다.

## 프로젝트 개발 정책

### 개발 생명 주기

- 이 프로젝트는 **반드시** 분석, 설계, 구현, 테스트의 순서로 개발을 진행한다.
- 각 단계가 완료되었을 때, 지정된 템플릿을 이용한 산출물이 생성되어야한다.

## 분석 단계 — 요구사항 분석

- `.claude/skills/requirements-analyst/SKILL.md` — ISO 26262/A-SPICE 준수 요구사항(기능/비기능)
  분석 방법론. EARS 작성 패턴, UML/SysML 다이어그램 표현, ISO/IEC 25010 기반 비기능 요구사항과
  실행 가능한 검증방안, 양방향 추적성·일관성 확보 방안을 포함한다. 공식 요구사항 산출물 템플릿은
  `WP_Templates/`(SWE.1: `Engineering/SoftwareRequirementsAnalysis/`, 추적성: `Engineering/
  Traceability/TPL-TRC-001`)에 확정되어 있으며, 스킬의 "0. 산출물 템플릿" 절에 매핑·작성 절차가
  정리되어 있다. 템플릿 등록부는 `WP_Templates/PRC-TPL-001_표준 산출물 양식 등록부.xlsx`. 템플릿이
  개정되면(등록부 버전 상승) 그 즉시 스킬 0절을 새 버전에 맞춰 갱신한다.
- `.claude/agents/requirements-engineer.md` — 위 스킬을 사용해 요구사항 산출물을 작성하는 서브에이전트.

## 설계 단계 — 아키텍처 설계

- `.claude/skills/architecture-analyst/SKILL.md` — ISO 26262/A-SPICE 준수 SW 아키텍처 설계(SWE.2)
  분석·작성 방법론. 요소·인터페이스 계약 서술 패턴, UML/SysML(컴포넌트·BDD/IBD·시퀀스·상태) 다이어그램
  표현, ISO/IEC 25010 품질속성 분석, ISO 26262-6 아키텍처 수준 안전 설계 원칙(간섭으로부터의 자유, 오류
  검출/격리) 점검, 통합시험 기반 실행 가능한 검증방안, 상위 요구사항/하위 상세설계와의 양방향 추적성·
  일관성 확보 방안을 포함한다. 공식 산출물 템플릿은 `WP_Templates/Engineering/
  SoftwareArchitecturalDesign/`(TPL-SWE2-001/002, 등록부 기준 v1.0)에 있다. `requirements-analyst`
  스킬이 만든 SW 요구사항·Use Case를 입력으로 받고, 산출물은 `detailed-design-analyst` 스킬의 상위
  입력이 된다.
- `.claude/agents/architecture-design-engineer.md` — 위 스킬을 사용해 아키텍처 산출물을 작성하는
  서브에이전트.

## 설계 단계 — 상세설계

- `.claude/skills/detailed-design-analyst/SKILL.md` — ISO 26262/A-SPICE 준수 SW 상세설계(SWE.3)
  분석·작성 방법론(초안). 함수 계약(사전/사후조건) 서술 패턴, UML(클래스·시퀀스·상태·호출관계) 다이어그램
  표현, ISO 26262-6 SW 단위 설계 원칙 점검, 단위시험 기반 실행 가능한 검증방안, 상위 요구사항/아키텍처와의
  양방향 추적성·일관성 확보 방안을 포함한다. 공식 산출물 템플릿은 `WP_Templates/Engineering/
  SoftwareDetailedDesignAndUnitConstruction/`(TPL-SWE3-001/002, TPL-SBOM-001)에 있으며, `TPL-SWE3-001/
  002`는 등록부 기준 v0.9(초안)이다. `requirements-analyst` 스킬의 SW 요구사항·Use Case 및
  `architecture-analyst` 스킬(SWE.2)의 아키텍처 산출물을 입력으로 받는다는 전제로 동작한다.
- `.claude/agents/detailed-design-engineer.md` — 위 스킬을 사용해 상세설계 산출물을 작성하는 서브에이전트.

## 구현 단계 — 코딩

- `.claude/skills/tdd/SKILL.md` — [obra/superpowers](https://github.com/obra/superpowers)의
  `skills/test-driven-development`(RED-GREEN-REFACTOR, Iron Law, 테스트 품질 원칙)를 기반으로 이
  프로젝트에 맞게 각색한 구현 단계 방법론. `CLAUDE.md`의 "구현 지침"을 반영해 Python 3.14 +
  `unittest` 기반 Red-Green-Refactor 사이클, 모든 테스트 함수에 긍정/부정 케이스 구분(`@case
  Positive|Negative`)과 Doxygen 형식 목적 서술(`@brief`/`@given`/`@when`/`@then`/`@trace`) 의무화,
  함수 라인수(≤50)·순환복잡도(≤10)·중복코드(≤7라인)·Doxygen 주석 비율(≥20%)·네이밍 규칙(3글자 이상,
  낙타 표기법) 품질 지표, 오픈소스 측정 도구(radon/pylint/coverage 등) 활용 원칙,
  `detailed-design-analyst` 스킬(SWE.3) 산출물과의 추적성을 다룬다. 원본의 mock/테스트 품질 원칙은
  같은 폴더의 `writing-good-tests.md`에 있다. 공식 워드/엑셀 템플릿은 없으며, 소스코드·단위테스트·
  측정 리포트가 산출물이다.
- `.claude/agents/coding.md` — 위 스킬을 사용해 실제로 코드를 구현하고 테스트를 실행하는 서브에이전트
  (Bash 도구로 테스트·측정 명령을 직접 실행한다).

## 테스트 단계 — 통합 테스트

- `.claude/skills/integration-test-analyst/SKILL.md` — ISO 26262 Part 6/A-SPICE(SWE.5) 준수 SW
  통합 테스트 방법론. **테스트 베이시스는 `architecture-analyst` 스킬(SWE.2) 아키텍처 설계서의
  인터페이스 명세(6장)와 통합 순서(11장)** — 임의로 인터페이스 동작·순서를 지어내지 않는다. ISO
  26262-6 시험기법(요구사항기반/인터페이스/결함주입/자원사용/백투백/경계값/오류추정) 선정, 함수
  커버리지·Call 커버리지 **100%**를 종료 기준으로 삼는 측정 절차(실제 설치된 도구만 사용, 미달 시
  원인 분류), 결함·회귀 처리, `ENG-TRC-001`(`Coverage` 열 포함) 양방향 추적성·일관성 확보 방안을
  포함한다. 공식 산출물 템플릿은 `WP_Templates/Engineering/
  SoftwareComponentVerificationAndIntegrationVerification/`(TPL-SWE5-001/002/003, 등록부 기준
  v1.0)에 있다.
- `.claude/agents/integration-test-engineer.md` — 위 스킬을 사용해 통합 시험 케이스를 작성하고 실제로
  실행·커버리지 측정까지 수행하는 서브에이전트(Bash 도구 사용).

## 테스트 단계 — 시스템 테스트(SW 검증)

- `.claude/skills/sw-system-test/SKILL.md` — A-SPICE SWE.6(소프트웨어 검증) 준수 SW 시스템 테스트
  방법론. **완성된 SW 전체를 `requirements-engineer`가 만든 SW 요구사항 명세서(`ENG-SWE1-001`)
  기준으로 블랙박스 검증한다** — 컴포넌트 간 인터페이스(SWE.5, `integration-test-analyst`)나 함수
  내부 분기(단위, `tdd`)는 다시 다루지 않는다. ISO 26262 Part 6·ISO 29119/ISTQB 기반 기법(경계값/
  동치클래스/조합/페어와이즈/엣지케이스/경험기반, 페어와이즈·전수조합은 PICT 사용), ISO/IEC
  25000(SQuaRE) 품질 특성별 비기능 케이스 분류를 다룬다. 공식 산출물 템플릿은
  `WP_Templates/Engineering/SoftwareVerification/`(TPL-SWE6-001/002, 등록부 기준 v1.0)에 있으며,
  스킬의 "공식 산출물과 등록부" 절에 매핑·작성 절차가 정리되어 있다.
- `.claude/agents/sw-system-tester.md` — 위 스킬을 사용해 시스템 테스트 케이스를 작성·실행하는
  서브에이전트.

## 서브에이전트·스킬 경계 요약

각 서브에이전트는 정확히 자신의 스킬만 근거로 삼고, 아래 "범위 밖" 항목은 다른 서브에이전트에게
넘긴다(같은 내용을 여러 스킬에 중복 정의하지 않는다).

| 단계 | A-SPICE | 서브에이전트 | 사용 스킬 | 테스트 베이시스/입력 | 주요 산출물 | 범위 밖 → 담당 |
|---|---|---|---|---|---|---|
| 분석 | SYS.2/SWE.1 | requirements-engineer | requirements-analyst | 이해관계자 요구·안전목표·OEM 입력 | ENG-SWE1-001/002 | 아키텍처 → architecture-design-engineer |
| 설계(아키텍처) | SWE.2 | architecture-design-engineer | architecture-analyst | ENG-SWE1-001/002 | ENG-SWE2-001/002 | 함수 단위 상세설계 → detailed-design-engineer |
| 설계(상세) | SWE.3 | detailed-design-engineer | detailed-design-analyst | ENG-SWE2-001/002, ENG-SWE1-001 | ENG-SWE3-001/002, SBOM | 실제 구현·단위테스트 → coding |
| 구현·단위 테스트 | SWE.3 단위구현 | coding | tdd | ENG-SWE3-001/002 함수 계약 | 소스코드, 단위테스트(Branch 커버리지 100%) | 컴포넌트 간 통합 → integration-test-engineer |
| 통합 테스트 | SWE.5 | integration-test-engineer | integration-test-analyst | ENG-SWE2-001 인터페이스·통합순서 | ENG-SWE5-001/002/003(Function/Call 커버리지 100%) | SW 전체 요구사항 기반 블랙박스 검증 → sw-system-tester |
| 시스템 테스트 | SWE.6 | sw-system-tester | sw-system-test | ENG-SWE1-001 SW 요구사항 | ENG-SWE6-001/002 | 컴포넌트 인터페이스 화이트박스 검증 → integration-test-engineer |
| 감사(전 단계 공통) | PA2.1/2.2 (CL2) | aspice-cl2-auditor | aspice-auditor | 위 모든 산출물 | 심사 리포트 | 산출물 작성 자체는 하지 않음(감사만) |

### 분석 지침
- 요구사항 분석 단계 수행은 requirements-engineer 서브에이전트가 담당한다.

### 아키텍쳐 설계 지침
- 아키텍쳐 설계 단계 수행은 architecture-design-engineer 서브에이전트가 담당한다.

### 상세설계 지침
- 상세설계 단계 수행은 detailed-design-engineer 서브에이전트가 담당한다.

### 구현 지침
- 구현 단계 수행은 coding 서브에이전트가 담당한다.
- TDD 방식으로 진행하고, TDD 스킬을 사용해야 한다.
- 다음의 품질 지표를 **반드시** 준수해야한다.
  - 함수 라인수는 순수코드라인 50라인 이하여야 한다.
  - 함수 순환복잡도는 10이하여야 한다.
  - 중복 코드는 7라인까지 허용한다.
  - 주석은 Doxygen 방식으로 작성하며, 20% 이상 작성해야한다.
- 함수명, 변수명은 3글자 이상 사용하고, 낙타 표기법을 활용한다.

### 단위 테스트 지침
- 단위 테스트는 TDD로 대체하며, coding 서브에이전트가 담당한다.
- 단위 테스트는 Branch 커버리지 100%를 달성해야 한다.
- 테스트 성공률은 100%여야 한다.

### 통합 테스트 지침
- 통합 테스트 단계 수행은 integration-test-engineer 서브에이전트가 담당한다.
- 시험 기법은 ISO 26262 Part 6에 근거한다.
- 함수 커버리지와 Call 커버리지는 **반드시** 100%를 달성해야 한다.
- 테스트 베이시스는 아키텍처 설계서(`ENG-SWE2-001`)의 인터페이스 명세와 통합 순서다.
- 테스트 성공률은 100%여야 한다.

### 시스템 테스트 지침
- 시스템 테스트(SW 검증, SWE.6) 단계 수행은 sw-system-tester 서브에이전트가 담당한다.
- 완성된 SW 전체를 SW 요구사항 명세서(`ENG-SWE1-001`) 기준으로 블랙박스 검증한다 — 컴포넌트 간
  인터페이스(통합 테스트)나 함수 내부 분기(단위 테스트)를 다시 검증하지 않는다.
- 시험 기법은 ISO 26262 Part 6, ISO 29119/ISTQB에 근거하고, 비기능 요구사항은 ISO/IEC 25000(SQuaRE)
  품질 특성별로 구분한다.
- 테스트 성공률은 100%여야 한다.
