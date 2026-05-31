---
name: j-token-workflow
description: j-token-workflow-kit의 워크플로우를 명시적으로 시작하는 진입점입니다. Claude Code 기본 "워크플로우(Workflow 도구)"와 단어가 겹쳐 혼동될 때, 이 키트의 요구사항·디버깅·Figma/UI·PRD·스펙 워크플로우를 분명하게 지시하기 위해 사용합니다. 예시 지시) /j-token-workflow, j-token 워크플로우로 정리해줘, j-token 워크플로우 시작

---

# j-token 워크플로우 진입점

## 목적

이 스킬은 `j-token-workflow-kit`의 워크플로우를 **명시적으로** 시작하기 위한 진입점입니다.

"워크플로우"라는 단어는 Claude Code 기본 `Workflow` 도구(서브에이전트 오케스트레이션)와 겹칩니다. 그래서 사용자가 "워크플로우로 정리해줘"라고만 말하면 Claude가 어느 쪽을 의도했는지 헷갈릴 수 있습니다.

이 혼동을 피하기 위해, 사용자는 `/j-token-workflow`를 호출하거나 "j-token 워크플로우"라고 명시해서 **이 키트의 문서 수렴 워크플로우**를 분명하게 지시할 수 있습니다.

이 스킬이 호출되면 Claude Code 기본 `Workflow` 도구(서브에이전트 오케스트레이션)를 사용하지 않습니다. 대신 아래의 키트 내부 워크플로우 모듈로 라우팅합니다.

## 동작

이 스킬은 단일 작업을 강제하지 않고, 요청 안의 작업 단위를 감지해 키트 내부의 적합한 모듈로 연결합니다. 실제 라우팅과 진행 규칙은 `workflow-composer` 스킬과 동일합니다.

| 단서 | 연결 모듈 |
|---|---|
| 배경, 원하는 결과, 거친 기능, 정책, 스펙 요청 | `requirements-to-spec` |
| 제품/SDK/CLI/런타임/개발자 도구 기획, "왜·무엇" | `prd-writer` |
| API, 프로토콜, 경계, 테스트 등 "어떻게"와 계약 | `technical-spec-writer` |
| 증상, 오류, 스크린샷, 로그, 재현, "안 된다" | `bug-report-to-fix` |
| Figma 링크, 화면, 디자인, UI 흐름, 시각 자료 | `figma-flow-to-implementation` |
| 위 작업이 한 요청에 섞여 있을 때 | `workflow-composer` |

여러 모듈이 동시에 일치하면 `workflow-composer`의 활성화·질문 순서·임시 문서 규칙을 그대로 따릅니다.

## 항상 함께 적용하는 보조 스킬

- `cognitive-writing`: 산출 문서를 읽기 쉽게 구조화합니다.
- `branch-rule`: 브랜치 생성 시 의미 접두사를 사용합니다.
- `commit-rule`: 사용자가 명시적으로 커밋을 요청했을 때만 적용합니다.
- `pr-rule`: 사용자가 명시적으로 PR을 요청했을 때만 적용합니다.
- `git-push-safety`: 모든 push 전에 적용합니다.

## 핵심 원칙

- 이 스킬은 "이 키트의 워크플로우를 쓰겠다"는 명시적 신호입니다. 기본 `Workflow` 도구로 우회하지 않습니다.
- 사용자가 말하지 않은 플랫폼, 원인, 화면 순서를 단정하지 않습니다.
- 관련 문서가 확정되었거나 사용자가 명시적으로 구현을 요청한 뒤에만 구현을 시작합니다.
- 구체적인 진행·문서·plan 모드 규칙은 `workflow-composer`를 따릅니다.
