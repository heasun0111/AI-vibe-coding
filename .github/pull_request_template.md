## 요약

<!-- 무엇을, 왜 바꿨는지 한두 문장으로 -->

## 변경 종류

- [ ] feat — 신규 기능/산출물
- [ ] fix — 버그 수정
- [ ] docs — 문서/스킬/CLAUDE.md 변경
- [ ] refactor — 동작 변경 없는 리팩터링
- [ ] test — 테스트만 추가/수정
- [ ] chore — 빌드/CI/설정 등

## 개발 생명주기 체크리스트

해당 단계에 관련된 항목만 체크한다.

- [ ] 요구사항 변경 시 `ENG-SWE1-001`/`002`(requirements-engineer)를 갱신했다
- [ ] 아키텍처 변경 시 `ENG-SWE2-001`/`002`(architecture-design-engineer)를 갱신했다
- [ ] 상세설계 변경 시 `ENG-SWE3-001`/`002`(detailed-design-engineer)를 갱신했다
- [ ] 구현 코드는 `tdd` 스킬(coding 서브에이전트)의 Red-Green-Refactor로 작성했고, **Branch 커버리지
      100%**를 달성했다
- [ ] 통합 대상 변경 시 `integration-test-engineer`로 통합 시험(Function/Call 커버리지 100%)을
      갱신했다
- [ ] 시스템 테스트 대상 변경 시 `sw-system-tester`로 `ENG-SWE6-001`/`002`를 갱신했다
- [ ] `ENG-TRC-001` 양방향 추적 매트릭스를 갱신했다
- [ ] CI(`lint-and-test`)가 통과한다

## 확인 필요/미해결 사항

<!-- 확인이 필요한 가정, 미해결 갈등이 있으면 여기에 남긴다. 없으면 "없음"이라고 쓴다. -->
