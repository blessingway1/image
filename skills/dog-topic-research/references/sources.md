# 트랙별 도구 · 검색어 · 출처

도구 이름은 vidIQ MCP 기준(`mcp__vidIQ_for_Claude__…`). 지연 로드 도구면 ToolSearch로 스키마부터 불러온다.

---

## 트랙 ① 영상 시장 (한국어)

### 호출 레시피 (기본 8~12회)

| # | 도구 | 설정 | 목적 |
|---|---|---|---|
| 1 | `vidiq_outliers` | keyword=핵심 행동 입말, language=`ko`, publishedWithin=`threeMonths`, **requireAllTitleTerms=`true`**, sort=`breakoutScore`, limit 30 | 채널 평균 대비 터진 롱폼 |
| 2 | 위와 같음 | keyword=동의어 2~3개로 반복 | 같은 행동의 다른 입말 |
| 3 | `vidiq_youtube_search` | query=입말, type=`["video"]`, order=`viewCount`, publishedAfter=90일 전, regionCode=`KR`, videoDuration=`medium`, limit 30 | 절대 조회 상위 + **설명란 목차** |
| 4 | 위와 같음 | videoDuration=`short` | 쇼츠·카드형 시장 |
| 5 | `vidiq_trending_videos` | videoFormat=`short`, titleQuery=입말, videoTitleLanguage=`ko`, sortBy=`vph` | 지금 속도가 붙는 쇼츠 |
| 6 | `vidiq_channel_videos` | 우리 채널 `UCe5nZhgg5cjwtU0nDpuf0Bw` | 재탕 검사용 최근 편 목록 |

**잡음 줄이기 (실측):**
- requireAllTitleTerms 없이 "강아지 심리 행동"으로 돌리면 정치 뉴스·연애 심리가 섞인다. 첫 호출은 반드시 엄격 모드로 하고, 결과가 5개 미만일 때만 느슨하게 푼다.
- 한국어 outliers는 롱폼만 된다(쇼츠 거부). 쇼츠는 #4·#5로 본다.
- `youtube_search`는 조회 상위를 주지만 채널 평균 대비 배수(outlierScore)가 비어 있는 경우가 많다. 배수는 outliers 결과를 우선한다.

### 영상 1건당 기록할 것

제목 · 채널 · 구독자 · 조회 · 배수(breakoutScore/outlierScore) · 공개일 · 길이 · **포맷 분류** · URL(`https://www.youtube.com/watch?v=<id>`)

포맷 분류: `롱폼-심리채널` / `롱폼-방송클립` / `롱폼-수의사·훈련사` / `쇼츠-카드형` / `쇼츠-스토리` / `사연·구조`. 길이 20분↑ 방송사 채널이면 방송클립, 60초 이하면 쇼츠.

### 분석할 것

- **문형**: 상위 제목의 뼈대(예: `[행동] 이유 N가지, ~만은 아닙니다` / `~할 때 하면 안 되는 행동 N가지`). thumbnail-title-rules.md의 3칸(주어 강아지·페이로드·장면)으로 가볍게 표기.
- **항목 구성**: 설명란 타임스탬프에서 항목 목록을 뽑아 경쟁작끼리 겹치는 항목 / 아무도 안 다룬 항목을 표로.
- **경쟁 공백**: "다들 X를 말하는데 Y는 아무도 안 한다" 한 줄 3개 이내.

---

## 트랙 ② 연구 (한국어 + 영어)

### 검색 경로 (WebSearch → WebFetch로 원문·초록 확인)

| 순서 | 출처 | 검색 예 |
|---|---|---|
| 1 | Google Scholar / PubMed | `"inequity aversion" dogs`, `jealousy dogs Harris Prouvost` |
| 2 | 저널 사이트 | Applied Animal Behaviour Science · Journal of Veterinary Behavior · Animal Cognition · PLOS ONE · Scientific Reports · Animals(MDPI) · PNAS |
| 3 | 국내 | KCI(kci.go.kr) · RISS · 국내 수의대 연구 — 국내 연구는 시청자 밀착도가 높아 우선순위 1 (fact-check-rules.md §3) |
| 4 | 가이드라인 | AVSAB 입장 성명 · AAHA · WSAVA · 머크 수의 매뉴얼 |
| 5 | 척도 | C-BARQ · HCPI · CBPI — "설문에 이 항목이 있다"는 강한 근거 |

### 연구 1건당 기록할 것

저자 · 연도 · 저널 · **국가·기관(정식 명칭)** · 표본(마릿수·조건) · **측정 방식**(행동 관찰/설문/소변·혈액/fMRI) · 결과 방향 · 한계·반론 · **등급** · 원문 URL

등급: ✅ 원문(최소 초록) 확인 / ⚠️ 근거 있으나 반론·범위 제한 / 🔍 원문 미확인 / ❌ 반박됨.

### 꼭 같이 찾을 것

- **반박된 통념** — 그 소재에서 보호자가 믿는데 연구가 뒤집은 것(처형 재료).
- **반론 논문** — ✅ 연구에 반론이 있으면 같이 적는다(Schilder 2014처럼).
- **경쟁 영상이 인용한 연구** — 설명란 참고문헌을 원문으로 역추적해 귀속이 맞는지 본다. 틀렸으면 리포트에 "경쟁작 귀속 오류"로 기록(우리 처형·차별점 재료).

---

## 트랙 ③ 뉴스 · 통계 (국내)

| 출처 | 쓰임 |
|---|---|
| KB경영연구소 『한국 반려동물 보고서』(격년) | 반려견 가구 수, 다견 비율, 양육 고민 |
| 농림축산식품부 『동물보호 국민의식조사』(매년) | 양육 가구 비율, 양육 포기·파양 이유 |
| 한국농촌경제연구원 · 통계청 | 보조 수치 |
| 국내 뉴스(최근 90일) | 시의성 — 법·제도 변화, 사고, 계절 이슈 |

수치마다 값 · 조사 기관 · 조사 연도 · 표본 · URL. 보고서 원문을 못 열면 기사 인용으로 쓰되 🔍.

---

## 트랙 ④ 시청자 언어

| 도구 | 사용 |
|---|---|
| `vidiq_comment_insights` 또는 `vidiq_video_comments` | 트랙 ①의 상위 영상 2~3편(우리 포맷 우선) |
| WebSearch | `site:kin.naver.com <입말>`, `<입말> 카페`, `<입말> 고민` — 네이버 지식iN·카페 질문 제목 |

뽑을 것:
- 보호자 입말 **그대로 인용**(훅·썸네일 조건절 재료).
- 반복 고민 Top 5.
- 오해(통념) — "~라서 그런 거죠?" 형태의 믿음. 트랙 ② 반박 근거와 짝을 맞춘다.
- 댓글 속 불만(예: "같은 말만 반복") — 우리 대본 함정 예방 재료.

개인 식별 정보(닉네임·프로필)는 리포트에 옮기지 않는다. 문장만 인용한다.
