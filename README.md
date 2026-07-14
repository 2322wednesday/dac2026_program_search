# DAC 2026 Program Explorer (비공식 / Unofficial)

제63회 설계자동화학회(**DAC 2026**, 63rd Design Automation Conference, 2026년 7월 25–31일, 롱비치 컨벤션 센터)의
프로그램을 키워드·초록·저자로 검색하고 관련 논문을 추천받을 수 있는 **단일 파일 웹앱**입니다.
[hyeondong.kim/icml26](https://www.hyeondong.kim/icml26/) 스타일을 참고해 만들었습니다.

## 파일 구성

| 파일 | 설명 |
|------|------|
| `index.html` | 앱 본체(단일 파일). 데이터가 내장돼 있어 **더블클릭만으로** 바로 열립니다. |
| `papers.json` | 논문 데이터셋. 같은 폴더에 두고 웹서버로 열면 이 파일을 우선 사용합니다. |
| `README.md` | 이 문서 |

## 주요 기능

- **키워드 검색** — 제목·초록·키워드·저자를 한 번에 검색. `전체/제목/초록/저자/키워드` 범위 선택, `OR/AND` 모드, `"따옴표"` 구문 검색 지원.
- **저자 검색** — 저자명 클릭 또는 범위를 `저자`로 두고 검색하면 해당 저자의 논문만 모아 봅니다.
- **관련 논문 추천** — 각 논문 상세에서 제목·초록·키워드(TF‑IDF 코사인 유사도) + 같은 트랙 + 공저자 관계로 유사 논문을 추천합니다.
- **일정(세션) 보기** — 날짜별·세션별로 프로그램을 펼쳐 봅니다.
- **내 일정(My Plan)** — ★로 관심 논문을 담고 `.ics`로 캘린더에 내보냅니다.
- **트랙·날짜·유형 필터**, **인기 키워드** 칩, **한국어/English 전환**, **라이트/다크 테마**.

## 데이터 범위 (중요)

DAC 2026 공식 프로그램([63dac.conference-program.com](https://63dac.conference-program.com/))은
자바스크립트로 목록을 불러오기 때문에 전체 목록을 한 번에 내려받을 수 없습니다.
그래서 전 세션 페이지(sess105–329)를 직접 훑어 **실제 데이터**로 전체 프로그램을 수집했습니다.

- 수록 논문: **1,035편** — 리서치(544) + 엔지니어링(283) + WIP·Late-Breaking(106) + 특별세션/전시포럼/패널/기조/튜토리얼/워크숍 등
- 세션 **163개**(총 225개 세션 크롤) · 저자 **3,951명** · 트랙: AI/ML, EDA, Design, Security, Systems, Quantum, Chiplet
- 검색: 제목·초록·키워드는 부분어까지, **저자는 이름 구문 매칭**(예: “Seonghan Kwon” → 해당 저자 논문). 약칭도 전체어 우선 정렬(예: “PHAP” → 해당 논문 우선).
- 초록: 대표 논문 일부에 전문 포함(그 외는 공식 페이지 링크 제공)
- 참고: 소수 세션은 공식 페이지가 404/빈 페이지였고, 초대형 포스터 세션 1건은 페이지 변환 한계로 일부 뒤쪽 항목이 누락될 수 있습니다.

## 전체 프로그램으로 확장하기

`papers.json`을 **동일한 스키마**의 전체 데이터로 교체한 뒤 웹서버로 열면 앱이 그대로 확장됩니다.

```jsonc
{
  "meta": { "conference": "...", "venue": "...", ... },
  "papers": [
    {
      "id": "RESEARCH976",
      "title": "논문 제목",
      "authors": ["Author One", "Author Two"],
      "author_affil": [{"name": "Author One", "affil": "소속"}],  // 선택
      "abstract": "초록 전문(선택)",
      "session": "세션명",
      "sess_id": "sess124",
      "type": "research",            // research|panel|keynote|tutorial|workshop|special
      "track": "AI/ML",              // 비우면 제목/초록에서 자동 분류
      "day": "Monday, July 27",
      "date": "2026-07-27",
      "start": "1:30pm", "end": "3:00pm", "room": "Mtg Room 101B",
      "url": "https://63dac.conference-program.com/?post_type=page&p=16&id=RESEARCH976&sess=sess124"
    }
  ]
}
```

`track`, `abstract`, `author_affil`은 비워도 동작합니다(트랙은 자동 추정, 검색·추천은 사용 가능한 필드로 수행).

## 여는 방법

- **바로 열기**: `index.html` 더블클릭 (데이터 내장, 오프라인 동작)
- **웹 호스팅**(전체 데이터 확장 시 권장): `index.html`과 `papers.json`을 같은 폴더에 두고 정적 호스팅
  (GitHub Pages, Netlify 등) 또는 `python3 -m http.server` 로 실행

## 고지

DAC / ACM / IEEE 와 무관한 **비공식** 커뮤니티 도구입니다. 논문 제목·초록·저자 등 원문 정보의
저작권은 각 저자와 학회에 있으며, 정확한 최신 정보는 항상 [공식 프로그램](https://63dac.conference-program.com/)을 확인하세요.
