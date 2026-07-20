# report-to-brief

[![Version](https://img.shields.io/badge/version-0.2.0-blue.svg)](CHANGELOG.md)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-purple.svg)

긴 보고서를 **배포 가능한 정책브리프·요약본(1~8쪽)으로 압축**하는 [Claude Code](https://claude.com/claude-code) 스킬입니다.

단순 요약이 아닙니다. **두괄식**(결론 먼저)·**자립성**(브리프만 읽어도 이해)·**근거 보존**(핵심 수치·출처 유지) 원칙으로 재구성하고, 압축 과정에서 생기기 쉬운 **핵심 누락·의미 왜곡·출처 유실**을 3축 검수로 잡아냅니다.

> **English**: A Claude Code skill that compresses long reports into deployable policy briefs / executive summaries (1–8 pages). Not a naive summarizer — it rebuilds the document conclusion-first, keeps it self-contained, and preserves key figures *with their sources*, then runs a 3-axis compression review (fidelity to source / self-containment / evidence preservation) to catch the omissions, distortions, and dropped citations that ordinary summarization introduces.

## 왜 다른가

LLM 요약은 흔히 세 가지로 실패합니다. report-to-brief는 그걸 검수 게이트로 막습니다:

| 요약의 흔한 실패 | report-to-brief의 방어 |
|------------------|------------------------|
| 압축하다 **없는 내용을 지어냄**(환각) | 축1: 브리프의 모든 진술이 원본에 실재하는지 대조 |
| 수치는 남고 **출처가 떨어져 나감** | 축3: 핵심 수치에 출처 동반 강제 |
| 원본의 조건·뉘앙스를 **과잉 단정**으로 | 축1: 뉘앙스 왜곡 점검 |
| 원본을 알아야만 이해되는 **반쪽 요약** | 축2: 자립성·두괄식 점검 |

## 작동 방식

```
[원본 보고서] → 해부(핵심 자산 추출) → 브리프 집필(두괄식) → 3축 압축검수 → 출력
                      ↑ 경량 게이트                              ↑ 검증 내장
```

1. **해부**: 절대 버리지 않을 핵심 자산(결론·수치+출처·제언·표)을 뽑고, 버릴 것을 표시. 압축 설계를 먼저 확인받음.
2. **집필**: 선택 유형 골격으로 재구성 — `policy_brief`(배경→쟁점→근거→제언) / `exec_summary` / `one_pager` / `issue_paper`.
3. **3축 검수**: 원본 충실성 / 자립성·두괄식 / 근거·출처 보존.
4. **출력**: 마크다운 + 압축 요약 리포트. 요청 시 hwpx/docx (기관 양식은 form-tailor 연계).

예시: [examples/example-brief.md](examples/example-brief.md)

## 설치

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/parkjui92/report-to-brief.git
```

### 의존성 (없으면 폴백)

- `.hwp/.hwpx` 입력: [kordoc](https://github.com/chrisryugj/kordoc) MCP
- `.docx`: pandoc/python-docx · `.pdf`: pdf 스킬
- 출처 실재 검증 연계(선택): [fact-verify](https://github.com/parkjui92/fact-verify)

## 사용법

```
이 보고서 2쪽 정책브리프로 압축해줘
50쪽 보고서 핵심만 한 장으로
이 연구보고서 executive summary 만들어줘 (의사결정자용)
이슈페이퍼로 요약, 수치랑 출처는 살려서
```

## 옵션

| 옵션 | 기본값 | 설명 |
|------|--------|------|
| 분량 | 2p | 1p / 2p / 4p / 8p 또는 "원본의 1/N" |
| 유형 | policy_brief | policy_brief / exec_summary / one_pager / issue_paper |
| 청중 | 정책결정자 | 의사결정자 / 실무자 / 일반 |

## 범위

- 압축은 **원본의 재서술**입니다. 원본에 없는 주장·수치·출처를 만들지 않습니다.
- 원본을 수정하지 않습니다(새 파일 산출).

## 연구자용 스킬 시리즈

- **report-to-brief** (이 저장소) — 보고서→브리프 압축
- **[form-tailor](https://github.com/parkjui92/form-tailor)** — 기관 양식 맞춤 제작
- **[fact-verify](https://github.com/parkjui92/fact-verify)** — 출처 신뢰도 검증
- **[paper-proofread](https://github.com/parkjui92/paper-proofread)** — 한국어 학술 원고 교정교열

## 라이선스

[Apache License 2.0](LICENSE)
