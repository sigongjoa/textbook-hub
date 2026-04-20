# 한정된 변형 프로세스

> "자유 편집이 아니라 정해진 오퍼레이션 안에서 바꾼다"는 제품의 핵심 차별점.

## 왜 한정된 프로세스인가?

자유 편집의 문제:
- 변형 의도가 기록 안 됨 → 파생본의 의미 추적 불가
- 품질 검증이 어려움 (무엇이 바뀌었는지 모름)
- AI 자동화의 조립 지점이 불명확
- 커뮤니티가 "어떻게 변형하면 좋은지" 학습 불가

한정된 프로세스의 이점:
- 각 파생본에 **"이 프로세스 적용됨"** 라벨 자동 부착
- 검증 pipeline이 프로세스별로 최적화 가능
- AI가 개입할 지점이 명확 (각 프로세스의 실행 엔진)
- "난이도↓ 변형이 가장 인기" 같은 메타 통계 가능
- 초보 선생님도 "뭘 할 수 있는지" 목록으로 학습

## 프로세스 카탈로그 (초안)

> 블록 타입별로 적용 가능한 프로세스가 다름.

### 공통 프로세스 (모든 블록 타입)

| 프로세스 | 파라미터 | 설명 |
|---|---|---|
| `grade.shift` | from, to | 학년 변환 (고1→중3 등) |
| `tone.shift` | from, to | 어투 변환 (격식↔친근) |
| `length.adjust` | ratio | 길이 조절 (0.5x, 2x 등) |
| `language.translate` | to | 언어 변환 (ko→en) |

### problem 블록 전용

| 프로세스 | 파라미터 | 설명 |
|---|---|---|
| `problem.difficulty` | delta | 난이도 조정 (+1 / -1 / -2) |
| `problem.numbers` | seed | 수치만 교체 (같은 구조, 다른 숫자) |
| `problem.context` | new_context | 문맥 교체 ("사과"→"축구공") |
| `problem.type_shift` | from, to | 유형 변환 (서술형→MCQ 등) |
| `problem.mcq_choices` | n, distractor_level | MCQ 오답 선지 재생성 |
| `problem.multiply` | n | 유사 문제 N개 생성 (drill 블록으로 변환) |

### concept 블록 전용

| 프로세스 | 파라미터 | 설명 |
|---|---|---|
| `concept.simplify` | target_level | 더 쉽게 설명 |
| `concept.deepen` | target_level | 더 깊이/엄밀하게 |
| `concept.add_example` | n | 예시 N개 추가 |
| `concept.add_visual` | - | 시각자료 추가 지점 표시 |

### figure 블록 전용

| 프로세스 | 파라미터 | 설명 |
|---|---|---|
| `figure.recolor` | palette | 색 팔레트 변경 |
| `figure.reparameterize` | params | CeTZ 파라미터 변경 |
| `figure.add_annotation` | labels | 주석/라벨 추가 |

### solution 블록 전용

| 프로세스 | 파라미터 | 설명 |
|---|---|---|
| `solution.expand` | detail_level | 풀이 더 자세하게 |
| `solution.abbreviate` | - | 풀이 요약 |
| `solution.add_why` | steps | 각 단계에 "왜" 설명 추가 |
| `solution.alt_method` | - | 다른 풀이 방법 추가 |

## 프로세스 파이프라인

프로세스는 **체인 가능**. 한 블록에 여러 개 순차 적용.

```
blk_original (고1 개념설명, 격식체)
    ▼ grade.shift(from=고1, to=중3)
    ▼ tone.shift(from=격식, to=친근)
    ▼ concept.add_example(n=2)
blk_forked (중3 개념설명, 친근체, 예시 2개 추가)
```

적용된 프로세스 체인이 `applied_ops`에 기록되어 계보 추적 가능.

## 프로세스 실행 엔진

각 프로세스는 다음 중 하나의 방식으로 실행:

| 방식 | 예시 | 설명 |
|---|---|---|
| **Pure (결정론적)** | `problem.numbers(seed=42)` | AI 없이 룰 기반, 같은 입력→같은 출력 |
| **AI-assisted** | `concept.simplify` | LLM 호출 (Claude 등), 프롬프트는 프로세스가 소유 |
| **Hybrid** | `problem.difficulty` | AI로 변형 후 math-verify로 정합성 체크 |
| **Manual** | `figure.add_annotation` | 프로세스는 "입력 칸만 제공", 사람이 직접 채움 |

## 검증 게이트 (공통)

모든 프로세스 실행 후 자동 검증:

1. **math-verify** (math 블록 대상) → 오답/모순 없음 확인
2. **prose-verify** → 어투·가독성·어휘 확인
3. 실패 시 롤백 또는 사용자에게 수정 요청

## 커스텀 프로세스

작성자/관리자가 새 프로세스를 정의 가능 (플러그인).
- 이름, 파라미터 스키마, 실행 엔진, 검증 조건을 JSON으로 등록
- 커뮤니티 검토 후 카탈로그 등재

## 열린 질문

- [ ] 프로세스는 되돌릴(undo) 수 있나? (git revert 같은 개념)
- [ ] 프로세스의 "성공 기준"은 누가 정하나? 플랫폼? 작성자?
- [ ] AI-assisted 프로세스의 AI 호출 비용은 누가 부담? (플랫폼/사용자/Author)
- [ ] 프로세스 조합에 "금지된 체인"이 있나? (예: grade.shift 두 번 연속)
- [ ] 프로세스 자체의 버전관리는? (같은 `problem.difficulty`라도 알고리즘 개선 시 v2)
