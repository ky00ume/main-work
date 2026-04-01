# main-work

AI 어시스턴트(Claude, GPT 등)를 활용한 작업을 위한 프롬프트, 가이드, 스킬 자료 모음입니다.

## 폴더 구조

- `claude-desktop-prompts/` — Claude Desktop 바이브 코딩용 맞춤 MD 프롬프트
- `superpowers-adapted/` — obra/superpowers 기반 Claude 스킬 모음
- `dist/` / `release/` — 빌드 결과물

## claude-desktop-prompts 사용법

`claude-desktop-prompts/` 폴더의 파일들은 `ky00ume/BOT` (비전 타운 RPG 봇) 프로젝트를 Claude Desktop에서 작업할 때 사용하는 프롬프트 모음입니다.

| 파일 | 용도 | 사용 시점 |
|------|------|-----------|
| `CLAUDE.md` | BOT 프로젝트 전체 컨텍스트 | 새 대화 시작 시 항상 첨부 |
| `context-prime.md` | 빠른 컨텍스트 주입 | 짧은 작업 세션 시작 시 |
| `create-prd.md` | 신기능 기획 명세서 작성 | 새 기능 개발 전 |
| `optimize.md` | 코드 최적화 요청 | 특정 함수/모듈 개선 시 |
| `todo.md` | 작업 계획 수립 | 여러 파일 수정이 필요한 작업 전 |
| `update-docs.md` | 문서 업데이트 | 코드 변경 후 패치노트 작성 시 |
| `evaluate-repository.md` | 외부 코드 안전성 검토 | 외부 라이브러리/코드 도입 전 |

## 기타 파일

- `CLAUDE.md` — main-work 레포 전용 Claude 작업 지침
- `superpowers-adaptation-guide.md` — superpowers 스킬 적용 가이드
- `# PROMPT ENGINEERING MASTER GUIDE.txt` — 프롬프트 엔지니어링 종합 가이드
- `Claude Skill 작성 모범 사례.txt` — Claude 스킬 작성 가이드
- `GPT5.4 프롬프팅 가이드.txt` — GPT 프롬프팅 가이드
- `Gemini 프롬프트 가이드.txt` — Gemini 프롬프트 가이드