# textbook-hub (가칭)

선생님용 교재 버전관리·공유 시스템 구상.

## 포지셔닝 (2026-04-20 재정의)

> **이전**: 교재 공유 커뮤니티 웹 SaaS
> **현재**: 학원 OS 안의 교재 모듈 (`EducationCoreArchitecture`의 콘텐츠 레이어)

독립 SaaS로 분리하지 않고, `class-log` / `homework` / `teach-note` / `task-log` 와 통합된 학원 운영 시스템의 일부로 설계. 이유는 [`docs/moat.md`](docs/moat.md) 참조.

## 확정된 방향

- **Fork 단위**: **Lesson** (차시) — 내부 편집은 Block 단위
- **편집 방식**: 한정 프로세스 초안 + 자유편집 fine-tune (diff 기록)
- **AI 역할**: ①실행 엔진 (LLM) ②검증 보조 (math-verify/prose-verify + 교육학적 타당성) ③학생별 추천
- **공개 모델**: Free(CC-BY) / Paid(500~3000원, 70-30 분배) / Private 3-tier
- **저작권**: `source_type` 필수 (`original`/`inspired_by`/`derived_from_commercial`), 상업 파생은 Private 전용
- **1차 타깃**: 소형 학원장 (월매출 500~2000)

## 핵심 시나리오

"A 선생님의 Lesson을 가져와서, 내 학생 프로필에 맞게 문제 난이도·수치만 변형해서 배포한다."

→ **Fork(Lesson)** + **한정 프로세스 + 자유편집 hybrid** 조합으로 달성.

## 문서 구조

### 구상
- [`docs/usecases.md`](docs/usecases.md) — 유즈케이스 (UC-01 ~ UC-08)
- [`docs/blocks.md`](docs/blocks.md) — 콘텐츠 구조 (Lesson + Block)
- [`docs/processes.md`](docs/processes.md) — 한정 프로세스 카탈로그

### 전략 검토
- [`docs/critique.md`](docs/critique.md) — 비판적 검토 (교육 현장 관점)
- [`docs/solutions.md`](docs/solutions.md) — 해결책 + MVP 피벗
- [`docs/moat.md`](docs/moat.md) — moat 분석 + 포지셔닝

### 목업 & 피치
- [`mockup/index.html`](mockup/index.html) — Toss/Karrot 톤 웹 목업
- [`deck/index.html`](deck/index.html) — Pitch Deck (17 slides)
- Live: https://sigongjoa.github.io/textbook-hub/ · Deck: https://sigongjoa.github.io/textbook-hub/deck/

## MVP (6주 × 3 스프린트)

1. **개인 라이브러리** — Lesson CRUD + 학생별 Fork + 프로세스 2개
2. **Fork 카탈로그** — 프로세스 5개 + 자유편집 + 계보 뷰
3. **공유·유료** — 3 tier + 결제 + 정산

공유 커뮤니티는 MVP 이후. 자세한 내용은 [`docs/solutions.md`](docs/solutions.md).

## 미정 항목

- [ ] 브랜드명 (textbook-hub은 가칭)
- [ ] `EducationCoreArchitecture`와의 통합 시점 (초기부터 vs 개인 라이브러리 후)
- [ ] 학생 프로필 데이터 스키마 (`class-log` 기존 스키마 확장 여부)
