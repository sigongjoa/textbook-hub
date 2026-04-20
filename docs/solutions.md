# 해결책 및 MVP 피벗

> [`critique.md`](critique.md)에서 제기한 문제별 해결책. 이 프로젝트 구조(블록/프로세스/검증) 위에서 실행 가능한 수준으로 구체화.

## 문제 → 해결 매핑

### 1. 블록 단위 Fork가 실무와 어긋남
**해결: 이중 단위 — `Lesson` (차시, Fork 단위) + `Block` (Lesson 내부 구성요소).**

- Fork·공유·검색·판매 단위 = **Lesson** (예: "이차함수 도입 45분, 개념+예제2+연습5")
- Block은 Lesson 내부 편집 단위로만 사용
- 사용자는 맥락 유지한 채 가져가고, 내부만 블록 프로세스로 변형
- [`blocks.md`](blocks.md)의 계층에 Lesson을 한 층 추가. 교재 = Lesson의 ordered list.

### 2. 공유 인센티브 공백
**해결: 3-tier 공개 모델 + 경제 레이어.**

| Tier | 설명 | 보상 |
|---|---|---|
| Free (CC-BY) | 누구나 사용·Fork | 크레딧·랭킹·학원 브랜딩 (프로필 노출) |
| Paid (1회 구매) | Lesson 단위 500~3000원 | 작성자 70% / 플랫폼 30% |
| Private | 본인 전용 | — |

핵심: 학원장 페르소나에겐 **"내 학원 원생 50명이 쓰는 교재가 다른 학원장에게 월 N건 팔리는 부수입"**이 유일하게 작동하는 동기. "사용됨 카운트"는 장식.

### 3. 저작권 지뢰
**해결: 출처 플래그 필수 + tier 제한.**

- 블록/Lesson 생성 시 `source_type` 필수: `original` / `inspired_by` / `derived_from_commercial`
- `derived_from_commercial`는 Private tier만 허용 (공개·판매 불가)
- `inspired_by`는 원출처 명기 + "30% 이상 변형" 자가진단 체크
- 신고 기반 takedown + 반복 위반자 차단

### 4. 한정 프로세스 = 함정
**해결: 프로세스 초안 → 자유편집 fine-tune → diff 기록. 하이브리드.**

- 한정 프로세스는 "80% 초안 생성기", 자유편집 허용
- 자유편집도 `applied_ops: ["manual_edit", diff_hash]`로 기록
- "한정"의 철학은 **"무엇이 바뀌었는지 추적 가능"**이지 "자유편집 금지"가 아님
- [`processes.md`](processes.md)의 "Manual" 실행 방식을 1급 시민으로 격상

### 5. AI 역할 미정
**해결: 3-layer 고정.**

1. **실행 엔진**: 각 프로세스의 LLM 호출 (Claude Sonnet 4.6 기본)
2. **검증 보조**: math-verify + prose-verify 기 탑재. 여기에 **"교육학적 타당성"** 한 층 추가 — 학년별 어휘·문장길이 기준 통과 여부
3. **추천**: 학생 프로필 기반 "이 학생에겐 이 Lesson + 이 프로세스 조합" 제안

README의 "AI 역할 미정" 자리에 이 3개를 박아넣으면 결정 완료.

### 6. 기존 솔루션 대비 차별화
**해결: 두 개로 압축.**

- **"검증된 변형"**: math-verify/prose-verify 통과본만 유통. iScream·카페는 공유는 되지만 품질 검증 없음.
- **"학생별 개별화 자동화"** (UC-07): 한 Lesson을 학생 N명 프로필에 맞춰 N개 Fork 자동 생성.

이 두 개가 유일한 moat 재료. 나머지는 복제 가능. (자세한 moat 분석은 [`moat.md`](moat.md))

---

## MVP 재정의 (6주 스프린트 × 3)

공유 커뮤니티는 **MVP에 없음**. 개인 선생님이 혼자 써도 가치 있는 도구가 먼저.

### Sprint 1 — 개인 라이브러리 (4주)
- Lesson CRUD + Block 에디터 (Typst 기반, 기존 `book-kit` 재사용)
- math-verify / prose-verify 통합
- 본인 Lesson → 학생별 Fork + 프로세스 2개 (`problem.difficulty`, `problem.numbers`)

### Sprint 2 — Fork + 프로세스 카탈로그 (4주)
- 한정 프로세스 5개 (`grade.shift`, `problem.difficulty`, `problem.numbers`, `concept.simplify`, `solution.expand`)
- 자유편집 + diff 기록
- 계보 뷰 (UC-06)

### Sprint 3 — 공유·유료 (4주)
- Free / Paid / Private 3 tier
- 구매·결제 (토스페이먼츠)
- 작성자 정산 대시보드

공유는 Sprint 3 이후 fast-follow. 유저 20명 쌓인 뒤 오픈.

---

## 성공 지표 (MVP 90일)

| 지표 | 목표 | 실패 시 의미 |
|---|---|---|
| 주 1회 이상 Fork 실행 유저 | 15명 | 한정 프로세스가 실제 needs 아님 |
| Lesson당 평균 Fork 수 | 1.5+ | 변형이 한 번에 끝남 = 개별화 수요 약함 |
| 유료 Lesson 구매 | ≥5건/월 | 경제 인센티브 공백 미해결 |
| math-verify 통과율 | >85% | AI 변형 품질 미흡 |

**셋 이상 달성 실패 시 개인용 도구로 피벗** 결정 (공유 플랫폼 포기).
