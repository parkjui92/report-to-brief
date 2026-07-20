# 왜 만들었나 · 상세 사용법

**한국어** · [English](#english)

README에서 덜어낸 배경과 사용 시나리오를 여기 둡니다.

---

## 왜 만들었나

정책·연구 현장에서 가장 자주 오는 요구 중 하나가 이겁니다. **"이 보고서, 4쪽으로 줄여 주세요."**

200쪽을 쓴 사람에게는 야박한 요구지만, 받는 쪽 사정도 분명합니다. **의사결정자는 원본을 읽지 않습니다.** 성의가 없어서가 아니라 책상에 올라온 보고서가 그것 하나가 아니기 때문입니다. 그래서 실제로 유통되고, 인용되고, 결정의 근거가 되는 문서는 200쪽이 아니라 **그 4쪽**입니다. 원본보다 요약본이 더 중요한 문서인 셈입니다.

그런데 LLM에게 "요약해 줘"라고 하면 나오는 건 **요약이지 브리프가 아닙니다.** 둘은 생각보다 많이 다릅니다.

- **요약은 원본의 순서를 따라 줄입니다.** 서론에서 시작해 결론에서 끝납니다. 브리프는 반대여야 합니다 — **결론이 맨 앞**에 와야 하고, 읽는 사람이 첫 문단만 읽고도 판단할 수 있어야 합니다. 결정권자가 3쪽까지 읽어 줄 것이라는 가정은 대체로 틀립니다.
- **요약은 원본에 기대는 문장을 씁니다.** "본 보고서는 ~을 다룬다", "3장에서 살펴본 바와 같이". 브리프에는 **그것만 읽어도 이해되는 자립성**이 필요합니다. 독자가 바로 그 원본을 안 읽은 사람이기 때문입니다.

여기까지는 형식의 문제라 고치기 쉽습니다. 진짜 문제는 그다음입니다.

**압축 과정에서는 조용히 사고가 납니다.** 분량을 맞추다 보면

- 핵심 수치가 통째로 빠지고,
- 수치는 남았는데 **출처만 떨어져 나가고**,
- 원문의 조건부 주장이 **단정으로 바뀝니다.** "일부 조건에서는", "단기적으로는", "상관관계일 뿐 인과로 단정하기 어렵다" 같은 단서절은 분량을 줄이는 사람 눈에 가장 먼저 군더더기로 보입니다. 그런데 그 단서절이 문장의 절반입니다. 떼어내는 순간 원문이 하지 않은 말이 됩니다.

이 손상의 고약한 점은 **원본을 아는 사람만 알아챌 수 있다**는 것입니다. 브리프 자체는 멀쩡해 보입니다. 문장이 매끄럽고, 수치가 구체적이고, 결론이 선명합니다. 원본과 나란히 놓고 대조하지 않으면 드러나지 않습니다. 그리고 브리프를 받아 읽는 사람은 정의상 원본을 안 읽은 사람입니다. **문서 안에 걸러낼 단서가 없습니다.**

그래서 이 스킬은 압축을 하고 끝내지 않습니다. 압축한 뒤 **원본과 다시 대조합니다.** 브리프의 모든 진술이 원본에 실재하는지(축1), 원본을 모르는 독자에게 성립하는지(축2), 수치가 출처를 데리고 왔는지(축3) — 이 **3축 압축검수**가 이 스킬의 본체입니다. 짧게 만드는 건 어렵지 않습니다. **짧게 만들면서 거짓말을 하지 않는 것**이 어렵습니다.

### 스킬 자신의 급소도 한 번 걸렸습니다

공개 전에 합성 보고서로 스모크 테스트를 돌리고, 실행 이력을 모르는 별도 세션에 적대적 검증을 맡겼습니다. 거기서 이 스킬 자신의 급소가 나왔습니다. **원문에 처음부터 출처가 없던 수치**를 만났을 때입니다. 검수는 "출처가 없다"고 잡아내는데, 정작 조치가 '출처를 창작하지 않는다'는 원칙과 교착돼 아무것도 못 하는 상태였습니다.

지금은 분기가 있습니다. 그건 브리프의 잘못이 아니라 **원본에 물어야 할 사항**이므로, 🔴(환각)이 아니라 `⚠️[원문 출처부재]`로 분류하고 "저자에게 원 출처 확인 요망"을 남깁니다. 없는 출처를 그럴듯하게 붙이는 것이 가장 나쁜 처리입니다. 같은 검증에서 분량 단위의 자기모순(마크다운에는 "2쪽"이 존재하지 않는데 압축비를 쪽 기준으로 산정하던 문제)도 함께 잡혔습니다. 전체 내역은 [CHANGELOG.md](../CHANGELOG.md)에 있습니다.

---

## 압축에서 흔한 사고와 이 스킬의 방어

| 압축에서 흔한 사고 | 이 스킬의 방어 |
|---|---|
| 압축하다 **없는 내용을 지어냄**(환각) | **축1** — 브리프의 모든 진술을 원본과 대조. 원본에 없으면 🔴 |
| 수치는 남고 **출처가 떨어져 나감** | **축3** — 핵심 수치에 출처 동반 강제 |
| 조건부 주장이 **과잉 단정**으로 바뀜 | **축1** — 단서절 보존 점검. 애초에 집필 단계에서 "뉘앙스·조건·출처는 압축 대상이 아니다"로 방어 전진배치 |
| **약어만 살아남고 정의가 탈락** | **Phase 1** — 약어-정의 쌍을 필수 자산으로 고정(정의가 서론·배경에 있어도 버리지 않음) |
| 원본을 알아야만 이해되는 **반쪽 요약** | **축2** — 자립성·두괄식 점검 |
| 원문에 **애초에 출처가 없던** 수치 | **축3 분기** — 환각이 아니므로 🔴 아님. `⚠️[원문 출처부재]` + 저자 확인 요망. **출처를 지어내지 않음** |

여기서 나온 "요약 vs 브리프"의 차이를 표로 정리하면 이렇습니다.

|  | 요약 | 브리프 |
|---|---|---|
| 순서 | 원본을 따라 서론 → 결론 | **결론이 맨 앞**(두괄식) |
| 상정 독자 | 원본을 읽은 사람 | **원본을 안 읽은 사람** |
| 문장 | "본 보고서는 ~을 다룬다" | 그 자체로 성립하는 진술 |
| 수치 | 남기도 하고 빠지기도 함 | **출처와 함께** 남김 |
| 목적 | 내용 전달 | **판단 근거 제공** |

---

## 작동 방식 — 4단계

```
[원본 보고서]
   │
   ├─ Phase 1  해부 — 절대 버리지 않을 핵심 자산 추출 + 버릴 것 표시
   │            └ ★경량 게이트: "이렇게 압축할까요?"  ← 여기서 개입합니다
   │
   ├─ Phase 2  집필 — 선택 유형 골격으로 재구성(두괄식 우선)
   │
   ├─ Phase 3  3축 압축검수 — 원본과 대조  ← 이 스킬의 본체
   │            축1 원본 충실성 · 축2 자립성·두괄식 · 축3 근거·출처 보존
   │
   └─ Phase 4  출력 — 브리프(.md) + 압축 요약 리포트 [+ .hwpx / .docx]
```

1. **해부** — 결론·쟁점·제언, 수치와 그 출처, 약어-정의 쌍, 남길 표를 "핵심 자산"으로 고정하고 버릴 것을 표시합니다. 이때 각 수치가 **원문에 출처를 갖고 있는지**도 함께 태깅합니다. 자산 개수를 "결론 5개" 같은 고정 틀에 억지로 맞추지 않고 보고서 구조를 따릅니다.
2. **집필** — `policy_brief`(핵심 메시지 → 배경 → 쟁점 → 근거 → 제언) / `exec_summary` / `one_pager` / `issue_paper` 중 선택한 골격으로 재구성합니다. **골격의 섹션 순서보다 두괄식 원칙이 우선**합니다.
3. **3축 검수** — 원본과 대조해 충실성·자립성·근거보존을 점검하고, 🔴·⚠️는 위치를 명시해 고칩니다.
4. **출력** — 마크다운 브리프와 압축 요약 리포트. 요청하면 `.hwpx`/`.docx`로 변환합니다.

전체 규격은 [SKILL.md](../SKILL.md)에, 4단계를 처음부터 끝까지 밟은 예시는 [examples/example-brief.md](../examples/example-brief.md)에 있습니다(가상의 40쪽 보고서를 2쪽으로 압축한 것입니다).

---

## 상세 사용법

설치 후 Claude Code를 재시작하면 "브리프로 만들어줘", "요약본", "한 장으로", "executive summary" 같은 요청에 자동으로 반응합니다.

### 시나리오 1 — 긴 보고서를 정책브리프로

가장 기본적인 쓰임입니다. 파일을 첨부하고 이렇게 말하면 됩니다.

```
첨부한 정책연구보고서를 4쪽짜리 정책브리프로 압축해줘.
청중은 국장급 의사결정자야.
```

분량과 청중을 함께 지정하는 것이 좋습니다. 청중이 정해지면 남길 것과 버릴 것의 기준이 서기 때문입니다 — 실무자용이라면 방법론과 절차가 살아남지만, 의사결정자용이라면 그 자리는 제언과 리스크가 차지합니다.

### 시나리오 2 — 결정 회의에 들고 갈 한 장

```
이 보고서 executive summary로 만들어줘. 결론·권고랑 리스크만.
```

```
한 장으로. 핵심 메시지 3~5개랑 근거 박스만 있으면 돼.
```

앞은 `exec_summary`(결론·권고 → 핵심 근거 → 리스크·다음 단계), 뒤는 `one_pager` 골격을 씁니다. 유형을 이름으로 부르지 않아도 문장에서 알아듣습니다.

### 시나리오 3 — 수치와 출처를 반드시 살려야 할 때

```
이슈페이퍼로 요약하되 수치랑 출처는 다 살려줘.
```

이 요구는 사실 기본값입니다. 스킬은 원래 **수치를 남기면 출처도 함께 남기도록** 설계돼 있고, 뉘앙스·조건절·출처는 목표 분량을 넘기더라도 삭제하지 않습니다. 분량은 부차 사례와 중복 설명에서 줄입니다. 그래도 명시해 두면 검수 보고가 더 촘촘해집니다.

### 시나리오 4 — 이미 다 쓴 보고서를 나중에 줄일 때

자매 저장소인 [policy-research-kit](https://github.com/parkjui92/policy-research-kit)으로 50쪽짜리 정책연구보고서를 만들었다면, 그 결과물을 이 스킬에 그대로 넘기면 됩니다.

```
방금 만든 보고서 hwpx를 4쪽 브리프로 줄여줘
```

**킷의 브리프 모드와는 쓰임이 다릅니다.** 킷의 브리프 모드는 처음부터 브리프를 연구 산출물로 쓰는 것이고, 이 스킬은 **이미 완성된 문서를 나중에 압축**하는 것입니다. 원본이 이미 있고 그것을 줄여야 한다면 이쪽입니다. 반대로 백지에서 쓰는 일이라면 이 스킬이 아니라 집필 스킬의 몫입니다.

시리즈를 이어 붙이면 이렇게 씁니다 — **킷으로 보고서를 쓰고 → report-to-brief로 줄이고 → form-tailor로 기관 양식에 맞추고 → fact-verify로 출처를 확인합니다.**

### 분량은 이렇게 지정합니다

마크다운에는 고정된 "쪽"이 없습니다. 그래서 스킬은 **글자 수로 환산해** 분량을 관리하고, 압축비도 글자 수 기준으로 계산합니다. 실제 쪽수는 `.hwpx`/`.docx`로 변환한 뒤에 병기됩니다.

| 목표 | 국문 기준 본문 분량 | 어울리는 유형 |
|------|--------------------|--------------|
| `1p` | 약 1,600자 | one_pager |
| `2p` | 약 3,200자 | policy_brief 표준(기본값) |
| `4p` | 약 6,400자 | |
| `8p` | 약 12,800자 | issue_paper 상한 |

`"원본의 1/5로"`처럼 비율로 말해도 됩니다. 다만 압축비가 과격해지면(1/20 이하) 살아남을 수 있는 것은 결론과 제언뿐입니다. 그럴 때는 분량을 늘리기보다 **"이번 브리프의 목적"을 좁혀 주는 편**이 결과가 낫습니다.

### ★ 압축 설계 게이트에서 개입하는 법

Phase 1이 끝나면 스킬이 멈춰 서서 **"이 자산을 살리고 이건 버리겠습니다"**를 먼저 보여 줍니다. 여기가 **가장 값싸게 방향을 바꿀 수 있는 지점**입니다. 다 쓰고 나서 "그게 빠졌네"를 발견하는 것보다 훨씬 낫습니다. 이렇게 말하면 됩니다.

```
방법론은 다 빼도 되는데 해외사례는 일본 건만 살려줘
표는 수급 전망 하나만 남겨
제언 3개는 그대로 두고 배경을 절반으로 줄여
2장 쟁점이 이 브리프의 핵심이야. 거기에 분량을 몰아줘
```

무엇을 살리고 버릴지에 대한 합의가 압축 품질을 좌우합니다. 브리프가 마음에 안 드는 경우의 상당수는 집필이 아니라 이 단계에서 갈립니다.

> 배치 실행이나 서브에이전트처럼 되묻기가 불가능한 환경에서는 멈추지 않고, 적용한 압축 설계와 가정을 **요약 리포트에 명시한 뒤** 진행합니다.

### 3축 검수 결과 읽는 법

브리프와 함께 이런 리포트가 나옵니다. 이게 이 스킬을 쓰는 이유의 절반입니다.

```
## 압축 요약
- 원본: (파일명), 약 M자 → 브리프: K자 (압축비 1/x, 글자 수 기준)
  · 쪽수는 hwpx/docx 변환 후 병기
- 유형: policy_brief | 청중: 정책결정자
- 살린 핵심 자산: 결론·쟁점·제언 n / 핵심 수치 n(출처 보존 n, [원문 출처부재] n) / 약어정의 n / 표 n
- 3축 검수: 충실성 ✅ | 자립성 ✅ | 근거보존 ⚠️ 1건([원문 출처부재] → 저자 확인 요망)
- 의도적으로 제외: 방법론 상술, 해외사례 3건 중 2건, 부록
```

**먼저 읽어야 할 줄은 맨 아래 "의도적으로 제외"입니다.** 3축이 전부 ✅여도, 버려진 것 중에 당신이 꼭 필요했던 것이 있으면 그 브리프는 실패입니다. 검수는 "빠뜨렸는가"를 보지 "빠뜨려도 되는 것이었나"를 보지 못합니다. 그 판단은 사람 몫입니다.

판정 기호는 이렇게 읽습니다.

| 기호 | 뜻 | 당신이 할 일 |
|---|---|---|
| ✅ | 해당 축에서 문제 없음 | — |
| 🔴 | 원본에 없는 진술이 브리프에 있음 = **압축 중 환각** | 반드시 고칩니다. 스킬이 위치를 짚어 줍니다 |
| ⚠️ `[원문 출처부재]` | **원문 자체에** 출처가 없던 수치 | 브리프의 결함이 아니라 **원본에 대한 질의**입니다. 원저자에게 출처 확인을 요청하세요 |
| `원문 수치 재배열` | 원본에 없던 표를 원문 수치로 재구성 | 새 사실이 더해진 게 아닙니다. 그대로 두셔도 됩니다 |

축3에서 특히 헷갈리는 두 가지를 스킬이 구분해 보고합니다. **처리가 정반대**입니다.

- **압축 손실형** — 원문에는 출처가 있었는데 브리프에서 떨어져 나간 경우. → **원문의 출처를 복원**합니다. 정당한 처리입니다.
- **원문 출처부재형** — 원문에 애초에 출처가 없는 수치. → 브리프에 "(원문 출처 미표기)"를 달고 저자 확인을 요청합니다. **출처를 지어내지 않습니다.** '저자 추계' 같은 근거 없는 귀속도 금지입니다.

검수 결과가 미덥지 않으면 되물으면 됩니다.

```
축1 다시 봐줘. 3번 문단이 원문보다 세게 나간 것 같은데
이 수치 원본 몇 쪽에서 나온 건지 알려줘
```

### 입력은 어떤 형식이 되나

| 입력 | 처리 | 도구가 없으면 |
|---|---|---|
| `.hwp` / `.hwpx` | [kordoc](https://github.com/chrisryugj/kordoc) MCP로 텍스트·표 추출 | 본문을 붙여넣으면 그대로 진행 |
| `.docx` | pandoc 또는 python-docx | 〃 |
| `.pdf` | pdf 스킬 | 〃 |
| `.md` · 붙여넣은 본문 | 그대로 사용 | — |

> `.hwp`에서 텍스트를 추출하면 표·그림이 평탄화될 수 있습니다. 원본에 표가 많다면 추출 결과를 한 번 확인하고 시작하는 편이 안전합니다. 살려야 할 표가 있으면 압축 설계 게이트에서 지목해 주세요.

**출처의 실재까지 확인하고 싶다면** [fact-verify](https://github.com/parkjui92/fact-verify)를 함께 설치하면 연계됩니다. 이 스킬의 축3은 "원본과 일치하는가"를 보지, "그 출처가 실제로 존재하고 그 수치를 담고 있는가"까지는 보지 않습니다.

---

## 옵션과 산출물

| 옵션 | 기본값 | 설명 |
|------|--------|------|
| 분량 | `2p` | `1p` / `2p` / `4p` / `8p` 또는 "원본의 1/N" |
| 유형 | `policy_brief` | `policy_brief` / `exec_summary` / `one_pager` / `issue_paper` |
| 청중 | 정책결정자 | 의사결정자 / 실무자 / 일반 |

| 산출물 | 내용 |
|---|---|
| 브리프 본문 (`.md`) | 선택한 유형·분량·청중에 맞춘 두괄식 브리프 |
| 압축 요약 리포트 | 압축비, 살린 자산, **3축 검수 결과**, 의도적으로 제외한 것 |
| (요청 시) `.hwpx` / `.docx` | 배포용 변환본. 기관 양식이 필요하면 [form-tailor](https://github.com/parkjui92/form-tailor)와 연계 |

브리프의 각 핵심 항목이 **원본 어디서 왔는지 추적 가능**하게 만드는 것이 원칙입니다. 회의에서 "이 수치 어디 거냐"는 질문이 나왔을 때 답할 수 있다는 뜻입니다.

**원본은 수정하지 않습니다.** 항상 새 파일로 냅니다.

---
---

<a name="english"></a>

# Why I built this · Detailed usage

[한국어](#왜-만들었나--상세-사용법) · **English**

Background and usage scenarios trimmed out of the README.

---

## Why I built this

One of the most common requests in policy and research work is this: **"Can you cut this report down to four pages?"**

It feels ungenerous to whoever wrote the 200 pages, but the person asking has a point. **Decision-makers do not read the original.** Not out of indifference — there is more than one report on the desk. So the document that actually circulates, gets cited, and ends up justifying a decision is not the 200 pages. It is **those four pages**. The condensed version is the more consequential document.

But when you ask an LLM to "summarize this," what comes back is **a summary, not a brief.** The two differ more than they sound like they should.

- **A summary shrinks along the original's order.** It starts at the introduction and ends at the conclusion. A brief has to do the opposite — **the conclusion goes first**, and the reader has to be able to act on the opening paragraph alone. The assumption that a decision-maker will read as far as page three is usually wrong.
- **A summary writes sentences that lean on the original.** "This report examines…", "As discussed in Chapter 3…". A brief needs to be **self-contained**, because its reader is precisely the person who did not read that original.

Those are formatting problems, and formatting problems are easy to fix. The real problem is what comes next.

**Compression causes accidents quietly.** When you squeeze to fit a page target:

- key figures drop out entirely,
- figures survive but **their sources fall off**, and
- conditional claims harden into **flat assertions.** Hedges like "under certain conditions," "in the short term," or "this is a correlation and cannot be read as causal" are the first things that look like padding to whoever is cutting length. But that hedge is half the sentence. Remove it and the brief now says something the original never said.

The nasty part is that **only someone who knows the original can catch this.** The brief itself looks fine. The prose is smooth, the numbers are specific, the conclusion is crisp. Nothing surfaces unless you lay it next to the source. And the person reading the brief is, by definition, the person who did not read the source. **There is no signal inside the document telling them to check.**

So this skill doesn't stop at compressing. After compressing, it **goes back and compares against the original.** Does every statement in the brief actually exist in the source (axis 1)? Does it hold up for a reader who doesn't know the source (axis 2)? Did the figures bring their citations with them (axis 3)? That **three-axis compression review** is the substance of this skill. Making a document shorter is not hard. **Making it shorter without lying** is.

### The skill's own blind spot got caught once, too

Before release I ran a smoke test on a synthetic report and handed the output to a separate session — one with no knowledge of how it had been produced — for adversarial verification. That surfaced a blind spot in the skill itself: **figures that had no source in the original to begin with.** The review would flag "no source," and then deadlock against the rule that says never invent a citation. It caught the problem and had no move to make.

There is now a branch for it. That case is not the brief's fault — it is **a question to put back to the original**, so it is classified not as 🔴 (hallucination) but as `⚠️[no source in original]`, with a note asking the author to confirm where the figure came from. Attaching a plausible-looking citation would be the worst possible handling. The same round of verification also caught an internal contradiction in how length was measured (Markdown has no such thing as "page 2," yet compression ratios were being computed in pages). Full details are in [CHANGELOG.md](../CHANGELOG.md).

---

## Common compression failures and how this skill defends

| Common compression failure | How this skill defends |
|---|---|
| **Inventing content** while compressing (hallucination) | **Axis 1** — every statement in the brief is checked against the original. Not there → 🔴 |
| Figures survive but **sources fall off** | **Axis 3** — key figures must carry their citation |
| Conditional claims become **overstated assertions** | **Axis 1** — hedges are checked for survival. The defense is also moved forward into drafting: "nuance, conditions, and sources are not compressible" |
| **Acronyms survive, definitions don't** | **Phase 1** — acronym/definition pairs are locked in as required assets (kept even when the definition sits in the introduction) |
| A **half-summary** that only makes sense if you know the original | **Axis 2** — self-containment and conclusion-first structure |
| Figures that **never had a source in the original** | **Axis 3 branch** — not a hallucination, so not 🔴. Flagged `⚠️[no source in original]` + author query. **No citation is invented** |

The summary/brief distinction that follows from this, as a table:

|  | Summary | Brief |
|---|---|---|
| Order | Follows the original: intro → conclusion | **Conclusion first** |
| Assumed reader | Someone who read the original | **Someone who did not** |
| Sentences | "This report examines…" | Statements that stand on their own |
| Figures | Sometimes kept, sometimes cut | Kept **with their sources** |
| Purpose | Convey content | **Support a decision** |

---

## How it works — the four phases

```
[Source report]
   │
   ├─ Phase 1  Dissect — extract the assets that must survive, mark what goes
   │            └ ★ Lightweight gate: "compress it this way?"  ← you intervene here
   │
   ├─ Phase 2  Draft — rebuild on the chosen skeleton (conclusion-first wins)
   │
   ├─ Phase 3  Three-axis compression review — check against the original  ← the substance
   │            axis 1 fidelity · axis 2 self-containment · axis 3 evidence preservation
   │
   └─ Phase 4  Output — brief (.md) + compression report [+ .hwpx / .docx]
```

1. **Dissect** — conclusions, contested issues, recommendations, figures and their sources, acronym/definition pairs, and tables worth keeping are locked in as "core assets," and everything else is marked for cutting. Each figure is also tagged with **whether the original gave it a source**. Asset counts follow the report's actual structure rather than being forced into a fixed template ("five conclusions").
2. **Draft** — rebuilt on one of four skeletons: `policy_brief` (key message → background → issues → evidence → recommendations), `exec_summary`, `one_pager`, `issue_paper`. **Conclusion-first takes precedence over the skeleton's section order.**
3. **Three-axis review** — fidelity, self-containment, and evidence preservation are checked against the original; 🔴 and ⚠️ items are located and fixed.
4. **Output** — a Markdown brief plus a compression report. On request, converted to `.hwpx` or `.docx`.

The full specification is in [SKILL.md](../SKILL.md). A worked example running all four phases end to end is in [examples/example-brief.md](../examples/example-brief.md) — a hypothetical 40-page report compressed to two pages.

> **A note on the Korean context.** The default output shape is a *정책브리프* (policy brief) — in Korean policy practice, a short standalone document circulated to decision-makers ahead of a meeting, and often the only version anyone reads. Its longer sibling, the *이슈페이퍼* (issue paper), runs to roughly eight pages and adds analysis. Neither is exotic: they map closely onto the policy brief and executive summary formats used everywhere else, which is why the skill's four skeletons are named in those terms.

---

## Detailed usage

Restart Claude Code after installing and it will pick up requests like "turn this into a brief," "executive summary," "get it down to one page," or the Korean equivalents. Prompts work in either language.

### Scenario 1 — A long report into a policy brief

The basic case. Attach the file and say:

```
Compress the attached policy research report into a 4-page policy brief.
Audience is director-level decision-makers.
```

Give it both length *and* audience. Once the audience is fixed, the keep/cut boundary has a criterion — for a practitioner audience, methodology and process survive; for decision-makers, that space goes to recommendations and risks instead.

### Scenario 2 — One page to carry into the room

```
Make this an executive summary. Conclusions, recommendations, and risks only.
```

```
One page. Three to five key messages and an evidence box, that's it.
```

The first uses the `exec_summary` skeleton (conclusions and recommendations → key evidence → risks and next steps), the second `one_pager`. You don't have to name the type — it reads the intent from the request.

### Scenario 3 — When figures and sources must survive

```
Make it an issue paper, but keep every figure and its source.
```

This is actually the default behavior. The skill is built so that **if a figure survives, its citation survives with it**, and hedges, conditions, and sources are never deleted to hit a length target — length comes out of secondary examples and repeated explanation instead. Saying it explicitly still makes the review report more granular.

### Scenario 4 — Compressing a report you already finished

If you produced a 50-page policy research report with the sister repo [policy-research-kit](https://github.com/parkjui92/policy-research-kit), hand the result straight to this skill.

```
Cut the report I just produced down to a 4-page brief
```

**This is a different job from the kit's own brief mode.** The kit's brief mode writes a brief *as the research deliverable* from the start. This skill **compresses a document that already exists**. If you have a finished original and need it shorter, this is the right tool; if you are writing from a blank page, it isn't.

Chained across the series: **write the report with a kit → compress it with report-to-brief → fit it to an institutional template with form-tailor → check the sources with fact-verify.**

### Specifying length

Markdown has no fixed "page." So the skill manages length **by character count** and computes the compression ratio on the same basis. Actual page counts are reported only after conversion to `.hwpx` or `.docx`.

| Target | Body length (Korean characters) | Fitting type |
|--------|--------------------------------|--------------|
| `1p` | ~1,600 | one_pager |
| `2p` | ~3,200 | policy_brief standard (default) |
| `4p` | ~6,400 | |
| `8p` | ~12,800 | issue_paper ceiling |

This table is calibrated for **Korean-language body text**. Working in another language, express the target as a ratio instead — `"one-fifth of the original"` is a supported way to ask.

Either way, when the ratio gets aggressive (below about 1/20), the only things that can survive are conclusions and recommendations. At that point you'll get a better result by **narrowing what the brief is for** than by asking for more pages.

### ★ Intervening at the compression-design gate

At the end of Phase 1 the skill stops and shows you **"here is what I'll keep and what I'll cut."** This is **the cheapest point at which to change direction** — far better than discovering the omission after the brief is written. Just say so:

```
Drop all the methodology, but keep the Japan case
Keep only the supply-demand table
Leave all three recommendations, cut the background in half
Chapter 2's issues are the point of this brief — give them the space
```

Agreement on what lives and what dies drives compression quality more than the drafting does. Most unsatisfying briefs are decided at this step, not the next one.

> In non-interactive contexts (batch runs, subagents) where a round-trip is impossible, it does not halt. It **states the compression design and its assumptions in the report** and proceeds.

### Reading the three-axis review

The brief comes with a report like this. It is half the reason to use the skill.

```
## Compression summary
- Source: (filename), ~M chars → brief: K chars (ratio 1/x, by character count)
  · page count appended after hwpx/docx conversion
- Type: policy_brief | Audience: decision-makers
- Assets kept: conclusions/issues/recommendations n / key figures n (sourced n, [no source in original] n) / acronym definitions n / tables n
- Three-axis review: fidelity ✅ | self-containment ✅ | evidence ⚠️ 1 ([no source in original] → confirm with author)
- Deliberately excluded: methodology detail, 2 of 3 international cases, appendix
```

**Read the last line first — "deliberately excluded."** Even with three green checks, if something you needed is on that list, the brief failed. The review can tell you whether something was dropped; it cannot tell you whether it was *safe* to drop. That judgment is yours.

The verdict marks read as follows.

| Mark | Meaning | What you do |
|---|---|---|
| ✅ | No problem on that axis | — |
| 🔴 | The brief contains a statement absent from the original = **hallucination during compression** | Must be fixed. The skill points to the location |
| ⚠️ `[no source in original]` | A figure **the original itself** left uncited | Not a defect in the brief — it's **a query about the source document**. Ask the original author where it came from |
| `rearranged from source figures` | A table built from the original's numbers where the original had none | No new fact was added. Fine to leave as is |

On axis 3 the skill separates two cases that are easy to confuse. **They get opposite treatment.**

- **Lost in compression** — the original had a citation and the brief dropped it. → **Restore the original's citation.** Legitimate fix.
- **Never sourced in the original** — the original never cited it either. → Mark "(uncited in original)" in the brief and ask the author to confirm. **Do not invent a source.** Baseless attributions like "author's estimate" are equally forbidden.

If the review doesn't convince you, push back:

```
Re-check axis 1. Paragraph 3 reads stronger than the original does
Tell me which page of the original this figure came from
```

### What inputs it accepts

| Input | Handling | Without the tool |
|---|---|---|
| `.hwp` / `.hwpx` | Text and tables extracted via the [kordoc](https://github.com/chrisryugj/kordoc) MCP server | Paste the text and it proceeds |
| `.docx` | pandoc or python-docx | 〃 |
| `.pdf` | pdf skill | 〃 |
| `.md` · pasted text | Used directly | — |

> HWP/HWPX is Hangul Word Processor format — the de facto standard for Korean government and institutional documents, and the reason first-class support for it matters here. Note that text extraction from `.hwp` can flatten tables and figures. If the original is table-heavy, check the extraction before starting, and name any table that must survive at the compression-design gate.

**To verify that the sources themselves are real**, install [fact-verify](https://github.com/parkjui92/fact-verify) alongside it and the two connect. Axis 3 here checks agreement with the original; it does not check whether the cited source exists and actually contains that figure.

---

## Options and outputs

| Option | Default | Values |
|--------|---------|--------|
| Length | `2p` | `1p` / `2p` / `4p` / `8p`, or "1/N of the original" |
| Type | `policy_brief` | `policy_brief` / `exec_summary` / `one_pager` / `issue_paper` |
| Audience | Policy decision-makers | Decision-makers / practitioners / general |

| Output | Contents |
|---|---|
| Brief body (`.md`) | Conclusion-first brief matching the chosen type, length, and audience |
| Compression report | Ratio, assets kept, **three-axis review results**, what was deliberately excluded |
| (on request) `.hwpx` / `.docx` | Distribution-ready conversion. For institutional templates, connect [form-tailor](https://github.com/parkjui92/form-tailor) |

The governing principle is that every key item in the brief stays **traceable back to where it came from in the original**. Which means that when someone in the meeting asks where a figure came from, you can answer.

**The original is never modified.** Output always goes to a new file.
