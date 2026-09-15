---
# ─────────────────────────────────────────────────────────────
#  PROJECT 1
#  Rename this file to your project's slug (e.g. loan-default-model.md)
#  — the filename becomes the URL: /work/loan-default-model/
# ─────────────────────────────────────────────────────────────

order: 1                       # controls ordering across the site
title: "Music Matcher"
tagline: "Selecting Songs for any Situation: Human-curated & AI-powered"
status: Live                   # shown as a pill; set to "" to hide

demo_url: "https://rag-integrated-system-project-rojo.streamlit.app"
repo_url: "https://github.com/raul-rojor/RAG-integrated-system-project"

tech:
  - Python
  - Anthropic API
  - RAG
  - pytest
  - Streamlit

# Two or three punchy lines shown on the project card
highlights:
  - "71 automated tests, 100% passing. Verified reliability of AI paired with a deterministic scorer."
  - "Built RAG guardrails to prevent LLM hallucinations and keep song scoring based on human taste."
  - "Graceful offline fallbacks at every step → from AI-powered recommendations to local song catalog."

# Cover images. One path renders a single frame; two paths render side by side.
cover:
  - /images/music-matcher-demo-left.png
  - /images/music-matcher-demo-right.png
cover_labels:
  - "Input: the user describes a situation"
  - "Output: ranks songs with citations and explanations"
cover_caption: >-
  A run of the deployed app: a free-text situation goes in, is scored according to the rulebook,
  and recommendations come back with a cited source and a written reason for each match.

# Optional fact bar on the project page
facts:
  - label: Role
    value: "Solo build"
  - label: Timeline
    value: "Summer 2026"
  - label: Data
    value: "Local 20-song catalog + real web-sourced & verified candidates"
---

## The problem

Existing music recommenders rely on a user's past listening experience, an inscrutable black-box scoring algorithm, or tediously quantified taste inputs. The previous recommender I built only accessed a local song catalog and required structured numeric inputs from users and songs. Real users describe their preferences in natural language and look for songs from all those available online.

## Approach

The first step in improving the user experience was to allow for natural language inputs. I created a rulebook mapping mood descriptors to scalar values for different musical qualities and wrapped the existing scoring formula in a RAG layer accessing this rulebook. This was made possible by integrating an LLM through a BYOK Anthropic API mode. Since the original 20-song catalog greatly limited the set of recommendable songs, making precise song-to-user-preference matches incredibly rare, I added a second LLM step to connect user tastes with all potentially corresponding songs. Claude searches for songs on the web, cites their existence, and maps their musical qualities. This song-searching process was split into two steps: first a free search, then formatting with a dictionary-guaranteed structured call. Amplifying the dataset means users get an increased chance to find the perfect track. Lastly, the top scoring songs are ranked and outputted to users while Claude unwinds its song-quantifying logic by giving users the reasons behind song matches using warm, natural explanations. Adding three online LLM-powered steps adds hallucination and offline-error risks. To address these risks, I made song citations mandatory, clamped music features to valid ranges, introduced offline fallbacks with printed warnings, and included 71 pytests which all pass offline.

## Results

| Outcome | Metric | Status |
| --- | --- | --- |
| Recommendations match mood | Subjective user judgments indicate matches (N=5) | 95% song-to-situation alignment|
| Citation enforcement | 100% of proposals validated | Zero hallucinations shipped |
| Graceful fallbacks | Every online step can smoothly degrade offline | Fallback warnings logged |
| Test coverage | 71 tests | All tests passing |

## What I'd do differently

While allowing natural language inputs helped to smoothen the user experience, free-text can lead to a lack of context. Short situation/mood descriptions tend to limit the accuracy of song recommendations since the user's taste cannot be confidently arrived at. Additionally, since the input data is first fed into an LLM, user taste dictionaries are highly sensitive to small wording changes in the input. While the RAG layer serves to ground LLM outputs through more deterministic mapping, LLM input-sensitivity remains a reality. Finally, the scoring formula I designed weighs genre very heavily and categorically, with no partial points for similar genres. The heavy genre weight reflects my personal music preferences as creator of the scoring algorithm, but the all-or-nothing genre comparisons are a genuine issue. I would make three changes to address these drawbacks: adding a second RAG layer and corresponding rulebook in the input parsing step for the LLM to recognize when there is a lack of context and ask the user for more, lowering the Claude temperature in the API settings to reduce input sensitivity by producing more deterministic answers, and mapping genres onto a vector space to make genre scores account for genre proximity (cosine similarity giving genre similarity).
