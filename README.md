# 신입행원 페르소나 분석 스킬 (은행 공통)

어느 은행에서나 쓸 수 있는 **신입행원 교육용 페르소나 분석** Claude Code 스킬입니다.

은행 연수·인사 담당자나 영업점 선배·멘토와 문답하면서 세 가지 근거를 모아 신입행원 페르소나를 만듭니다.
- 상품·업무 데이터
- 설문·인터뷰 결과
- 공개 자료

결과는 **보고서형 HTML 한 장**입니다.

> IBK기업은행 신입행원용으로 만든 [IBK 상품 상담 가이드](https://github.com/2ztngpm7t6-blip/ibk-product-consult-guide)의 페르소나 분석 과정을 다른 은행에서도 쓸 수 있게 일반화했습니다.

## 할 수 있는 것

| | 내용 |
|---|---|
| 대상 기관 | 시중·특수·지방·인터넷전문은행, 상호금융·저축은행 |
| 대상 직무 | 개인금융 창구, 기업금융 |
| 문답 상대 | 연수·인사 담당자, 영업점 선배·멘토 |
| 근거 | 상품·업무 데이터, 설문·인터뷰 결과, 공개 자료, 그리고 현장 의견(출처 표시) |
| 결과물 | 대표 페르소나, 성장 5단계(은행 연수 제도에 맞춰 조정), 어려움 맵(막대·히트맵), 설문 결과 그래프, 자료에서 찾은 것, 교육 제안 |

## 정확성과 개인정보

**모든 항목에 출처 태그와 신뢰도가 붙습니다.**

| 신뢰도 | 조건 |
|---|---|
| 높음 | 서로 다른 유형의 출처 2개 이상 |
| 보통 | 한 유형 |
| 추정 | 현장의견만 있거나, 설문 응답자가 5명 미만 |

- 신뢰도는 스크립트가 출처로 다시 계산합니다. 출처 없는 항목, 없는 출처 ID, 부풀린 신뢰도가 있으면 경고합니다.
- 자료와 현장 의견이 어긋나면 **불일치**로 표시합니다.

**설문·인터뷰 원자료는 분석 전에 자동으로 가명화합니다.**
- 이름·사번·연락처·이메일 열을 지우고, 지점명은 `지점A`처럼 바꿉니다.
- 문장 속 전화번호와 "홍길동 대리" 같은 호칭도 지웁니다.

**보고서에는 집계만 넣습니다.**
- 응답자 5명 미만이면 분포를 숨깁니다.
- 소속·지점 문항은 집계하지 않습니다.

## 설치

이 저장소를 받아 그 폴더에서 Claude Code를 열면 바로 쓸 수 있습니다. 다른 폴더에서도 쓰려면 스킬 폴더를 개인 스킬 폴더로 복사합니다.

```bash
cp -r .claude/skills/bank-rookie-persona ~/.claude/skills/
```

Python 3.9 이상이 필요합니다(외부 패키지 없음).

## 사용 예

Claude Code에 이렇게 말하면 됩니다.

- "우리 은행 개인금융 창구 신입행원 페르소나 분석하자"
- "신입 설문지부터 만들어 줘" → 설문지·인터뷰 질문지 생성
- "설문 결과 CSV 줄게, 분석해 줘" → 가명화 → 집계 → 보고서

스크립트만 따로 쓸 수도 있습니다.

```bash
python .claude/skills/bank-rookie-persona/scripts/anonymize.py raw.csv --out anon.csv
```

```bash
python .claude/skills/bank-rookie-persona/scripts/survey_summary.py anon.csv --out survey.json
```

```bash
python .claude/skills/bank-rookie-persona/scripts/render_report.py persona.json --out report.html
```

## 예시

`examples/`에는 **가상 은행(예시은행)** 으로 두 직무를 시범 실행한 결과가 있습니다. 실제 은행·직원과 무관합니다.

| 직무 | 위치 | 가상 자료 |
|---|---|---|
| 개인금융 창구 | `examples/` | 설문 12명 |
| 기업금융 | `examples/corporate/` | 설문 10명, 기업금융 상품 목록 12개(`products_가상.json`) |

각 폴더에 들어 있는 파일:

| 파일 | 내용 |
|---|---|
| `survey_anonymized.csv` | 가명화를 거친 가상 설문 |
| `survey.json` | 집계 결과 |
| `persona.json` | 페르소나 분석 입력 |
| `*.html` | 완성 보고서 |

## 파일 구성

```
.claude/skills/bank-rookie-persona/
  SKILL.md                         진행 순서, 근거·신뢰도·개인정보 규칙
  references/interview-guide.md    담당자·선배 문답 라운드별 질문
  references/survey-kit.md         신입 설문지(15문항)·선배 인터뷰 질문지
  references/analysis-framework.md 기관 유형별 관점, 직무별 데이터 체크리스트, 5단계 기본틀
  references/persona-schema.md     persona.json 형식
  scripts/anonymize.py             원자료 가명화
  scripts/survey_summary.py        설문 집계(5명 미만 숨김)
  scripts/render_report.py         보고서 HTML 생성·근거 검사
```

## 주의

- 집단 페르소나를 만드는 도구입니다. 개인 평가에 쓰지 마세요.
- 설문 원자료와 가명화 전 파일은 저장소에 올리지 마세요(`.gitignore`에 `raw/`, `*_raw.*` 포함).
- 결과는 교육 설계 참고용 분석입니다. 은행의 공식 자료가 아닙니다.
