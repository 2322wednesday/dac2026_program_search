# DAC 2026 Program Explorer (Unofficial)

A **single-file web app** for searching the program of the 63rd Design Automation Conference (**DAC 2026**, July 25–31, 2026, Long Beach Convention Center) by keyword, abstract, or author, and for discovering related papers.
The interface was inspired by [hyeondong.kim/icml26](https://www.hyeondong.kim/icml26/).

## Files

| File | Description |
|------|-------------|
| `index.html` | The complete app in a single file. The dataset is embedded, so you can open it immediately by **double-clicking** the file. |
| `papers.json` | The paper dataset. When the app is served through a web server and this file is placed in the same directory, the app loads it in preference to the embedded data. |
| `README.md` | This document. |

## Key Features

- **Keyword search** — Search titles, abstracts, keywords, and authors at once. Supports `All/Title/Abstract/Author/Keyword` scopes, `OR/AND` modes, and exact phrase searches using `"quotation marks"`.
- **Author search** — Click an author's name, or select the `Author` search scope, to view all papers by that author.
- **Related-paper recommendations** — Each paper page recommends similar papers using title, abstract, and keyword similarity based on TF-IDF cosine similarity, along with track and co-authorship information.
- **Schedule and session view** — Browse the program by date and session.
- **My Plan** — Save papers of interest with the ★ button and export your personal schedule as an `.ics` calendar file.
- **Track, date, and type filters**, **popular-keyword** chips, **Korean/English switching**, and **light/dark themes**.

## Dataset Coverage (Important)

The official DAC 2026 program ([63dac.conference-program.com](https://63dac.conference-program.com/)) loads its listings dynamically with JavaScript, so the complete program cannot be downloaded in a single request.
To build this dataset, all available session pages (`sess105`–`sess329`) were crawled individually and the complete program was assembled from the actual program data.

- Included items: **1,035** — Research (544), Engineering (283), WIP and Late-Breaking Results (106), plus special sessions, exhibition forums, panels, keynotes, tutorials, workshops, and other events
- **163 sessions** identified from 225 crawled session pages, **3,951 authors**, and the following tracks: AI/ML, EDA, Design, Security, Systems, Quantum, and Chiplet
- Search behavior: titles, abstracts, and keywords support partial-word matching. **Author names use phrase matching**; for example, searching for “Seonghan Kwon” returns papers by that author. Acronym matches are ranked behind exact full-term matches when appropriate; for example, “PHAP” prioritizes the corresponding paper.
- Abstracts: full abstracts are included for **1,021 of 1,035 items**. The remaining 14 are event pages, such as panels, workshops, networking sessions, and PhD forums, for which no original abstract was provided.
- Related-paper recommendations: content similarity is calculated from titles, abstracts, and keywords using TF-IDF cosine similarity. Papers sharing an author are shown separately under a “More papers by the same author” section.
- My Plan: dates are displayed as horizontal columns, with each day ordered chronologically. Plans can be exported as an `.ics` file.
- Note: a small number of official session pages returned 404 errors or contained no content. One very large poster session may also be missing a few items near the end because of page-conversion limitations.

## Extending the App with a Complete Dataset

Replace `papers.json` with a complete dataset that follows the **same schema**, then serve the app through a web server. The app will automatically use the new dataset.

```jsonc
{
  "meta": { "conference": "...", "venue": "...", ... },
  "papers": [
    {
      "id": "RESEARCH976",
      "title": "Paper Title",
      "authors": ["Author One", "Author Two"],
      "author_affil": [{"name": "Author One", "affil": "Affiliation"}],  // optional
      "abstract": "Full abstract text (optional)",
      "session": "Session Name",
      "sess_id": "sess124",
      "type": "research",            // research|panel|keynote|tutorial|workshop|special
      "track": "AI/ML",              // automatically inferred from the title/abstract if omitted
      "day": "Monday, July 27",
      "date": "2026-07-27",
      "start": "1:30pm", "end": "3:00pm", "room": "Mtg Room 101B",
      "url": "https://63dac.conference-program.com/?post_type=page&p=16&id=RESEARCH976&sess=sess124"
    }
  ]
}
```

The app still works when `track`, `abstract`, or `author_affil` is omitted. Tracks are inferred automatically, while search and recommendation features use whichever fields are available.

## How to Open the App

- **Open locally**: Double-click `index.html`. The embedded dataset allows the app to run offline.
- **Host on the web** (recommended when using an expanded dataset): Place `index.html` and `papers.json` in the same directory and deploy them using a static hosting service such as GitHub Pages or Netlify. You can also run a local server with `python3 -m http.server`.

## Disclaimer

This is an **unofficial** community tool and is not affiliated with DAC, ACM, or IEEE. Copyright for paper titles, abstracts, author information, and other original materials belongs to the respective authors and conference organizers. Always consult the [official program](https://63dac.conference-program.com/) for the most accurate and up-to-date information.