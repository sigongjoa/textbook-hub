# 블록 단위 콘텐츠 구조

> Fork 단위 = 블록. 이 문서는 블록이 정확히 무엇인지, 어떤 타입이 있는지, 어떻게 모여서 교재가 되는지 정의.

## 계층 구조

```
📚 Book (교재)
 └─ 📖 Chapter (단원)
     └─ 📑 Section (소단원, 선택적)
         └─ 🧩 Block (블록) ← Fork 단위
             ├─ 타입, 메타데이터, 내용
             └─ 버전 이력
```

## 블록 타입 (8종)

| 타입 | 용도 | 예시 |
|---|---|---|
| **concept** | 개념 설명 | "이차함수의 정의" 해설 |
| **example** | 풀이 예제 | 예제 1: y=x²-4x+3 그래프 |
| **problem** | 문제 | "다음 함수의 최솟값을 구하시오" |
| **figure** | 시각자료 | CeTZ 그래프, Venn 다이어그램 |
| **solution** | 풀이 | 위 문제의 풀이 + 정답 |
| **drill** | 반복연습 세트 | 10개 유사 문제 묶음 |
| **summary** | 요약/정리 | 단원 끝 한 장 요약 |
| **prerequisite** | 선수학습 | "이 단원 전에 알아야 할 것" |

## 블록 구조 (JSON 스키마 초안)

```json
{
  "id": "blk_a7f3d9e1",
  "type": "concept",
  "version": 2,
  "author": "seojaeyong",
  "parent_id": "blk_9c2a1b8f",
  "lineage": ["blk_9c2a1b8f", "blk_0fd1..."],
  "applied_ops": ["학년변환:고1→중3", "난이도↓"],
  
  "metadata": {
    "subject": "수학",
    "grade": "고1",
    "chapter": "이차함수",
    "section": "이차함수의 그래프",
    "difficulty": 2,
    "tags": ["그래프", "평행이동"],
    "language": "ko"
  },
  
  "content": {
    "format": "typst",
    "body": "이차함수 $y = a(x-p)^2 + q$ 의 그래프는 ...",
    "assets": ["img_xyz.png"]
  },
  
  "relations": {
    "depends_on": ["blk_prereq_..."],
    "used_in_books": ["book_..."],
    "forked_by": ["blk_fork1_...", "blk_fork2_..."]
  },
  
  "verification": {
    "math_verify": "PASS",
    "prose_verify": "PASS",
    "last_verified_at": "2026-04-20T10:00:00Z"
  },
  
  "stats": {
    "use_count": 42,
    "fork_count": 7,
    "rating": 4.6
  }
}
```

## 교재(Book)는 블록 참조의 모음

```json
{
  "id": "book_iruda_quad_1",
  "title": "이루다용 이차함수 기초",
  "author": "seojaeyong",
  "visibility": "private",
  "chapters": [
    {
      "title": "이차함수의 정의",
      "blocks": [
        {"ref": "blk_a7f3d9e1", "version": "latest"},
        {"ref": "blk_example_1", "version": 3, "pinned": true},
        {"ref": "blk_problem_a", "version": "latest"}
      ]
    }
  ]
}
```

**핵심 설계**: 교재는 블록을 **참조(reference)**만 함. 블록 자체는 플랫폼의 공용 저장소에 존재. 교재 소유자가 바꾸는 것은 "어떤 블록을 어떤 순서로"뿐.

## 블록이 불변(immutable)인가 가변(mutable)인가?

| 선택 | 설명 | 트레이드오프 |
|---|---|---|
| **A. 불변 + 버전** | 한 번 발행한 블록은 수정 불가, 수정하려면 v2 발행 | 참조 안정성 ★, 작성자 번거로움 |
| **B. 가변 + 히스토리** | 같은 블록 id를 수정, 내부 히스토리만 유지 | 작성자 편의 ★, 참조 깨질 위험 |

**→ A 권장** (git commit과 동일한 철학). 블록 id는 특정 상태의 fingerprint. "latest" 참조는 별칭.

## Fork의 데이터 의미

Fork = 새 블록 id 생성 + parent_id 지정. 원본과 독립. 단 lineage를 따라 올라가면 원본 확인 가능.

```
blk_9c2a1b8f (원본, 고1 개념설명)
    ▼ Fork + [학년변환: 고1→중3]
blk_a7f3d9e1 (중3 개념설명, v1)
    ▼ UC-05 버전 업데이트
blk_a7f3d9e1 (중3 개념설명, v2)
```

## 열린 질문

- [ ] 블록 크기 상한은? (너무 크면 fork 의미 희석, 너무 작으면 조합 부담)
- [ ] 블록 간 의존성(depends_on)은 hard-require인가 힌트인가?
- [ ] 비공개(private) 블록을 공개 교재에서 참조할 수 있나?
- [ ] 블록 삭제는 가능한가? 참조하는 교재가 있으면 어떻게?
- [ ] Diff를 어떻게 보여줄 것인가? (Typst 소스 diff? 렌더 후 시각 diff?)
