# report-to-brief

[![Version](https://img.shields.io/badge/version-0.2.0-blue.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)

**한국어** · [English](README.en.md)

긴 보고서를 배포 가능한 **정책브리프·요약본(1~8쪽)으로 압축**하는 [Claude Code](https://claude.com/claude-code) 스킬입니다.
요약기가 아닙니다 — **결론을 맨 앞으로**(두괄식), **브리프만으로 성립하게**(자립성), **수치는 출처와 함께**(근거 보존). 그리고 압축 과정에서 무엇이 사라지고 무엇이 뒤틀렸는지 **원본과 대조해 보고합니다.**

<!-- 데모 GIF 자리 -->

## 요약이 아니라 브리프입니다

|  | 요약 | 브리프 |
|---|---|---|
| 순서 | 원본을 따라 서론 → 결론 | **결론이 맨 앞**(두괄식) |
| 상정 독자 | 원본을 읽은 사람 | **원본을 안 읽은 사람** |
| 수치 | 남기도 하고 빠지기도 함 | **출처와 함께** 남김 |
| 목적 | 내용 전달 | **판단 근거 제공** |

형식은 고치기 쉽습니다. 어려운 건 **압축 중에 조용히 나는 사고**입니다 — 핵심 수치가 통째로 빠지고, 수치는 남았는데 출처만 떨어져 나가고, 원문의 조건부 주장이 단정으로 바뀝니다. 브리프 자체는 멀쩡해 보이고 **원본을 아는 사람만 알아챌 수 있는데**, 브리프를 받아 읽는 사람은 정의상 원본을 안 읽은 사람입니다.

→ [왜 만들었나·상세 사용법](docs/why.md)

## 3축 압축검수

```
원본 → 해부(핵심 자산 고정) → ★압축 설계 게이트 → 집필(두괄식 우선) → 3축 압축검수 → 브리프 + 압축 요약 리포트
```

압축한 뒤 **원본과 다시 대조하는 것**이 이 스킬의 본체입니다. 짧게 만드는 건 어렵지 않고, **짧게 만들면서 거짓말을 하지 않는 것**이 어렵습니다.

- **축1 원본 충실성** — 브리프의 모든 진술이 원본에 실재하는가. 없으면 🔴(압축 중 환각). 원문의 조건절이 단정으로 바뀌지 않았는지도 함께 봅니다
- **축2 자립성·두괄식** — 원본을 모르는 독자에게 성립하는가. 약어가 첫 등장 시 정의되는가
- **축3 근거·출처 보존** — 핵심 수치가 출처를 데리고 왔는가. **원문에 애초에 출처가 없던 수치**는 환각이 아니므로 `⚠️[원문 출처부재]`로 분류하고 저자 확인을 요청합니다 — 출처를 지어내지 않습니다

해부가 끝나면 "이건 살리고 이건 버리겠습니다"를 먼저 보여 주고 멈춰 섭니다. **가장 값싸게 방향을 바꿀 수 있는 지점**입니다. 전체 규격은 [SKILL.md](SKILL.md), 4단계를 끝까지 밟은 예시는 [examples/example-brief.md](examples/example-brief.md)에 있습니다(가상의 40쪽 보고서를 2쪽으로 압축한 것입니다).

## 설치

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/parkjui92/report-to-brief.git
```

번들 파일을 받는 설치 경로라면 [`report-to-brief.skill`](report-to-brief.skill)을 그대로 쓰면 됩니다.

## 쓰는 법

```
첨부한 보고서를 4쪽 정책브리프로. 청중은 국장급 의사결정자야    ← 분량·청중 지정
이 보고서 executive summary로. 결론·권고랑 리스크만            ← 유형은 문장에서 알아듣습니다
방법론은 다 빼고 해외사례는 일본 건만 살려줘                    ← ★압축 설계 게이트에서
축1 다시 봐줘. 3번 문단이 원문보다 세게 나간 것 같은데          ← 검수 결과 되묻기
```

분량은 `1p`/`2p`/`4p`/`8p` 또는 "원본의 1/N", 유형은 `policy_brief`·`exec_summary`·`one_pager`·`issue_paper`, 청중은 의사결정자·실무자·일반 중에서 고릅니다(기본값 `2p`·`policy_brief`·정책결정자).

## 무엇이 남는가

브리프 본문(`.md`)과 **압축 요약 리포트** — 압축비, 살린 자산, 3축 검수 결과, 그리고 **의도적으로 제외한 것**. 요청하면 `.hwpx`/`.docx`로 변환하고, **원본은 수정하지 않습니다**(항상 새 파일).
리포트에서 **먼저 읽어야 할 줄은 "의도적으로 제외"**입니다. 3축이 전부 ✅여도 버려진 것 중에 꼭 필요했던 게 있으면 그 브리프는 실패이고, 그 판단은 사람 몫입니다.

## 한계

- **압축은 원본의 재서술입니다.** 원본에 없는 주장·수치·출처를 만들지 않습니다 — 뒤집어 말하면 **원본이 약하면 브리프도 약합니다**
- **검수는 스킬 자신이 수행합니다.** 집필자와 검수자를 분리한 에이전트 팀 킷만큼 독립적이지 않습니다. 오류를 줄이지 없애지는 못합니다
- **출처의 실재는 확인하지 않습니다.** 축3은 원본과의 일치를 봅니다 — URL이 살아 있는지, 그 문헌이 정말 그 수치를 담고 있는지는 [fact-verify](https://github.com/parkjui92/fact-verify)의 영역입니다
- `.hwp`/`.docx`/`.pdf` 입력에는 [kordoc](https://github.com/chrisryugj/kordoc) MCP 등 추출 도구가 필요합니다(없으면 본문을 붙여넣어 진행). 분량 환산표는 국문 기준이며, 확정 쪽수는 변환 후에만 알 수 있습니다

## 시리즈

**에이전트 팀 킷** — [policy-research-kit](https://github.com/parkjui92/policy-research-kit) (정책연구보고서) · [rnd-proposal-kit](https://github.com/parkjui92/rnd-proposal-kit) (정부 R&D 제안서) · [socsci-paper-kit](https://github.com/parkjui92/socsci-paper-kit) (사회과학 논문)

**제작·편집 킷** — [lecture-deck-kit](https://github.com/parkjui92/lecture-deck-kit) (강의자료 HTML 덱 · 브라우저 라이브 편집)

**단독 스킬** — **report-to-brief** (이 저장소, 보고서 압축) · [fact-verify](https://github.com/parkjui92/fact-verify) (출처 검증) · [paper-proofread](https://github.com/parkjui92/paper-proofread) (한국어 학술 교정교열) · [form-tailor](https://github.com/parkjui92/form-tailor) (기관 양식 맞춤)

## 라이선스

[MIT](LICENSE). 동봉한 예시는 가상의 보고서로 만든 것이며, 실제 수탁 과제 산출물은 포함하지 않습니다.
