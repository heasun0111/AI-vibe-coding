# CLAUDE.md

이 파일은 이 저장소에서 작업하는 Claude Code(claude.ai/code)에게 제공하는 안내 문서다.

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

## 설계 단계 — 상세설계

- `.claude/skills/detailed-design-analyst/SKILL.md` — ISO 26262/A-SPICE 준수 SW 상세설계(SWE.3)
  분석·작성 방법론(초안). 함수 계약(사전/사후조건) 서술 패턴, UML(클래스·시퀀스·상태·호출관계) 다이어그램
  표현, ISO 26262-6 SW 단위 설계 원칙 점검, 단위시험 기반 실행 가능한 검증방안, 상위 요구사항/아키텍처와의
  양방향 추적성·일관성 확보 방안을 포함한다. 공식 산출물 템플릿은 `WP_Templates/Engineering/
  SoftwareDetailedDesignAndUnitConstruction/`(TPL-SWE3-001/002, TPL-SBOM-001)에 있으며, `TPL-SWE3-001/
  002`는 등록부 기준 v0.9(초안)이다. `requirements-analyst` 스킬이 만든 SW 요구사항·Use Case 및
  SWE.2 아키텍처 산출물을 입력으로 받는다는 전제로 동작하며, SWE.2(아키텍처) 전용 스킬/에이전트는 아직
  만들지 않았다.
- `.claude/agents/detailed-design-engineer.md` — 위 스킬을 사용해 상세설계 산출물을 작성하는 서브에이전트.

