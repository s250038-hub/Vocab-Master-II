# 📚 Vocab Master II

> **Collect words. Master them. Play with them.**
> A local-first vocabulary vault with Cambridge Dictionary lookups, photo scanning, flashcards, four study modes, a scored Sentence Lab, and five word games — all in a single HTML file.

![Single file](https://img.shields.io/badge/single--file-HTML-4d7dff?style=flat-square)
![No build step](https://img.shields.io/badge/build-none-22c55e?style=flat-square)
![Local first](https://img.shields.io/badge/data-localStorage-8b5cf6?style=flat-square)
![Version](https://img.shields.io/badge/version-4-f5a524?style=flat-square)

---

## What this version includes

- **Clean start** — opens with one empty *My Vault*. The old demo vaults (*Everyday English*, *Exam Sprint*) are removed automatically, even from previously saved data; vaults you created yourself are kept.
- **Cambridge Dictionary meanings** everywhere (lookup, word drawer, game-word inspector, photo scan), with an automatic fallback source and a clear badge showing which one answered.
- **Photo Scan → one meaning → fast add** — photograph a page, tap a word, see exactly **one** meaning matched to the sentence in your photo, confirm it, and save it with a single tap.
- **Sentence Lab** — write a sentence using a vault word and get an instant **0–100 score** with a list of errors, suggested fixes, and bonus points.
- **Much faster lookups** — dictionary sources are queried in parallel and results are cached permanently in your browser.

---

## Contents

- [Quick start](#quick-start)
- [Features](#features)
  - [My Vault](#my-vault)
  - [Photo Scan](#photo-scan)
  - [Sentence Lab](#sentence-lab)
  - [Flashcards](#flashcards)
  - [Practice](#practice)
  - [Definition Quiz](#definition-quiz)
  - [Spelling Quiz](#spelling-quiz)
  - [Word Games](#word-games)
  - [Word drawer](#word-drawer)
- [Dictionary & data sources](#dictionary--data-sources)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Data & storage](#data--storage)
- [Import & export format](#import--export-format)
- [Privacy & offline behavior](#privacy--offline-behavior)
- [Browser support](#browser-support)
- [Troubleshooting](#troubleshooting)
- [Tech stack](#tech-stack)
- [Code map](#code-map)

---

## Quick start

No installation, no build step, no dependencies to manage.

1. Save the app as **`vocab-master.html`**.
2. Open it in a modern browser — double-clicking the file works.
3. *(Optional, recommended on strict browsers)* serve it locally:

   ```bash
   python -m http.server 8000
   # then visit http://localhost:8000/vocab-master.html
   ```

4. Add your first word with **Look up**, or tap **Scan photo** and point your camera at a page.

> **Internet is needed** for the first load (fonts, icons), for dictionary lookups, for the extended game word list, and for the first photo scan (the OCR engine downloads once and is then browser-cached). Everything else — your vault, study modes, Sentence Lab, and games with the built-in word list — works offline.

---

## Features

### My Vault

The home screen and heart of the app.

- **Look up** a word (hyphens and short phrases are accepted). Cambridge suggestions appear with checkboxes — keep the meanings you want, and optionally add your own meaning or translation with a part of speech.
- **Saved words list** with live search across words *and* definitions, a part-of-speech filter, and five sort modes: Custom order, A→Z, Z→A, Newest, Oldest.
- **Drag to reorder** words when *Custom order* is selected.
- **Bulk selection** — select all filtered words, delete in one action, and **Undo** from the toast that appears.
- **Multiple vaults** — create, rename, switch, and delete collections (at least one vault is always kept). The sidebar shows live counts of words, mastered words, and vaults.
- **Import / Export** any vault as a portable `.words` file (see [format](#import--export-format)).

### Photo Scan

Turn any page, screen, or sign into vocabulary — with context-aware meaning selection.

1. Tap **Scan photo · one meaning · fast add** and choose or take a photo.
2. The image is downscaled for speed and read locally by the OCR engine (a progress bar shows recognition).
3. Detected words appear as tappable chips (common words like *the/and* are filtered; up to 60 candidates). Words already in your vault are marked ✓ and skipped by auto-advance.
4. Tap a word. The app fetches Cambridge meanings and ranks every sense against **the sentence from your photo** — word overlap with definitions, examples and guide words, plus a light part-of-speech hint from the preceding word.
5. You see **exactly one meaning** — the best match — with the photo sentence quoted underneath so you can check it. If the photo contains only the word, the most common sense is shown.
   - **Not this one** cycles to the next candidate meaning, one at a time (never a wall of definitions).
   - **Skip** moves on without saving.
6. **⚡ Add to vault** saves the word with that single meaning and automatically jumps to the next scanned word — tap, check, add, repeat.

Photos are processed **entirely in your browser** and never uploaded.

### Sentence Lab

Active recall through writing: pick a vault word, use it, get graded.

- Choose the target word from a dropdown or hit **Random**; its meanings are shown as a reminder, with a pronunciation button.
- Write one sentence (live word count; **Ctrl/⌘ + Enter** to check).
- The checker verifies you actually used the word — inflections count (`-s`, `-es`, `-ed`, `-ing`, `y → ies/ied`, dropped-e forms, and consonant doubling like *run → running*).
- Roughly twenty grammar and style rules run instantly, offline. Each issue is listed with a suggested fix.

**Penalties**

| Check | Points |
|---|---|
| Missing the target word | score capped at **30** |
| Sentence under 4 words | −10 |
| "should/could/would **of**" instead of "…have" | −6 |
| No capital letter at the start | −5 |
| No ending punctuation (`. ! ?`) | −5 |
| Looks like more than one sentence | −5 |
| Lowercase pronoun "i" | −5 |
| ALL CAPS | −5 |
| "your welcome" → "you're welcome" | −5 |
| Over 60 words | −5 |
| Wrong article **a / an** (sound-aware, e.g. *an hour*, *a university*) | −4 each (max 2) |
| Repeated word ("the the") | −4 |
| Missing apostrophe (*dont, isnt, im, ive…*) | −4 |
| "alot" → "a lot" | −4 |
| "its a / its very…" → "it's" | −4 |
| Space before punctuation / missing space after | −3 |
| Unbalanced parentheses or quotation marks | −3 |
| Doubled `!!` or `??` | −2 |
| Extra spaces between words | −2 |

**Bonuses**

| Achieved | Points |
|---|---|
| Each additional vault word used | +4 (up to +12) |
| A linking word (*because, although, however, unless…*) | +5 |
| Good sentence length (8–24 words) | +5 |
| Varied vocabulary (≥80% unique words, 8+ words) | +3 |

Score = `100 − penalties + bonuses`, clamped to 0–100.

| Score | Grade |
|---|---|
| 90–100 | Excellent! 🏆 |
| 75–89 | Great job! |
| 60–74 | Good — fix the small issues |
| 0–59 | Needs work — try again |

**Attempts, Best, and Average** are saved between sessions. *Edit & recheck* keeps your sentence for fixing; *Next word* clears and picks a new target.

> The checker is rule-based: it judges form and common mistakes, not whether your idea is brilliant. It runs instantly and needs no internet.

### Flashcards

- Paper-style 3D flip cards: word on the front, all meanings on the back.
- Flip with a tap or **Space**, browse with **← / →**, shuffle and restart anytime.
- Mark each card **I know this** or **Still learning** — mastered words get a green badge everywhere.
- Pronounce the word or any definition with the speaker buttons.

### Practice

Multiple-choice drills: read the word, pick the correct definition. Choose 2–6 answer options; distractors come from your own vault, so the difficulty is personal. 10 points per correct answer, with a results screen at the end.

### Definition Quiz

The reverse direction: a definition (with its part of speech) is shown — type the word. Immediate right/wrong feedback reveals the answer, and a final score summarizes the run.

### Spelling Quiz

Listen to the word via text-to-speech, then spell it. A hint button reveals the part of speech, definition, and letter count if you can't hear audio. Misspellings are shown struck-through next to the correct spelling.

### Word Games

Five complete games. Every word you discover or play becomes a tappable chip — open it to see its Cambridge meaning and add it to your vault without leaving the game.

| Game | Board | Clear / score by | Run ends |
|---|---|---|---|
| **Vocabtetris** | 13×13 | Falling letter blocks; words across or down clear | Stack reaches the top |
| **Vocablocky** | 9×9 | Placing pieces; straight runs spelling words clear | No remaining piece fits |
| **Endless Scrabble** | 9×9 | Classic tile play with bonus squares | Board completely filled (win!) |
| **Spelling Bee** | 7 or 10 letters | Words of 4+ letters using the hive | All possible words found (Queen Bee!) |
| **Wordle** | 4 / 5 / 6 letters | Guess in six tries | Solved or out of guesses |

**Vocabtetris** — steer and rotate letter tetrominoes. Any contiguous run spelling a dictionary word clears; gravity drops survivors and can trigger **chain reactions** with multipliers. Clear threshold is configurable (2-, 3-, or 4-letter minimum). Speed rises as you score. Scoring: `(cells×15 + words×10) × chain`. Touch pad + full keyboard control, pause anytime.

**Vocablocky** — drag or tap pieces (single cells up to 3×3 squares) onto the grid. Words of the chosen minimum length (3+ or 4+) clear for `(cells×10 + words×10)`; valid shorter words still earn small points. Game over only when nothing fits.

**Endless Scrabble** — a 7-tile rack, no turn limit. First word must cover the center ★; later words must connect with no gaps. Triple/Double Word and Triple/Double Letter squares multiply scores; using all 7 tiles earns a **+50 bingo**. Recall tiles or swap the whole rack freely.

**Spelling Bee** — build words from the hive (repeats allowed); every word must contain the amber center letter. 4-letter words score 1, longer words score their length, and a **pangram** (all letters used) earns +7 in Classic or +20 in Special mode. A progress bar tracks how much of the hive you've found.

**Wordle** — green/yellow/gray feedback with animated reveals, on-screen + physical keyboard, and persistent **Wins / Streak / Score**. Faster solves earn more: `(7 − guesses) × 20`.

Game word validation uses a full English word list when online, with a built-in offline list as fallback — the badge above the games shows which is active and how many words it contains.

### Word drawer

Tap any word anywhere in the app to open a side drawer:

- Meanings with parts of speech, Cambridge **example sentences**, and a source badge.
- **Edit** meanings (add/remove rows, change part of speech), **delete** with undo, toggle **mastered**, and hear pronunciation.
- For words found in games that aren't in your vault yet: one-tap **Add to current vault**.

---

## Dictionary & data sources

| Purpose | Source | Behavior |
|---|---|---|
| Word meanings | `dictionary.cambridge.org` | Fetched through **three CORS proxies in parallel** — the first successful response wins (6 s timeout each). |
| Meaning fallback | `api.dictionaryapi.dev` | Races alongside Cambridge with a **1.2 s head start** for Cambridge (5 s timeout). The UI badges which source answered. |
| Definition cache | your browser | Every successful lookup is stored (up to **400 words**) — repeat lookups are instant, even offline. |
| Game word validation | `dwyl/english-words` word list | Downloaded via jsDelivr, then GitHub raw as backup; falls back to the built-in offline list. |
| Photo OCR | Tesseract.js 5.1.1 (jsDelivr) | Lazy-loaded on first scan; recognition runs **locally** on a downscaled image. |
| Fonts & icons | Fontsource (Inter, Space Grotesk, Fraunces) + Font Awesome 6.4 | Loaded from CDNs, then browser-cached. |

Stale results are discarded: if you start a new search before the previous one finishes, only the newest result is shown.

---

## Keyboard shortcuts

| Where | Keys | Action |
|---|---|---|
| Anywhere | `Esc` | Close modal / word drawer |
| My Vault | `Enter` (in the word field) | Look up |
| Flashcards | `Space` / `←` `→` | Flip / previous / next |
| Sentence Lab | `Ctrl/⌘ + Enter` | Check & score |
| Vocabtetris | `←` `→` / `↑` / `↓` / `Space` / `P` | Move / rotate / soft drop / hard drop / pause |
| Spelling Bee | `A–Z` / `Backspace` / `Enter` | Type / delete / submit |
| Wordle | `A–Z` / `Backspace` / `Enter` | Type / delete / guess |

All games also have full on-screen touch controls.

---

## Data & storage

Everything lives in your browser's `localStorage` — no account, no server, no analytics.

| Key | Contents |
|---|---|
| `vocabMaster.v4` | Vaults and words, active vault, sound preference, best scores (Tetris, Blocky, Scrabble, Bee), Wordle wins/streak/score, Sentence Lab attempts/best/total |
| `vocabMaster.defs.v1` | Cached definitions with their source (max 400 entries, oldest evicted) |

**Migration:** on first run, data from older versions (`vocabMaster.v3`, `vocabMasterVaults`, `vocabMasterActiveVault`) is imported automatically. The demo vaults *Everyday English* and *Exam Sprint* are dropped; every vault you created is preserved.

**Reset everything** (browser DevTools console):

```js
localStorage.removeItem("vocabMaster.v4");
localStorage.removeItem("vocabMaster.defs.v1");
location.reload();
```

> Data is per browser **and per origin**. Moving the file, switching browsers, or using private mode starts fresh — use **Export** to carry your vault with you.

---

## Import & export format

Export produces `VocabMaster_<vault-name>_<YYYY-MM-DD>.words` — a plain JSON array (the native save dialog is used where supported, otherwise a download). Import accepts `.words`, `.json`, or `.txt` containing that array (or an object with a `words` array). Duplicates are skipped; `known` and `addedAt` are preserved.

```json
[
  {
    "id": "lx3k9ab12cd",
    "word": "serendipity",
    "definitions": [
      { "pos": "noun", "def": "The fact of finding interesting or valuable things by chance." }
    ],
    "addedAt": 1763000000000,
    "known": false
  }
]
```

Valid `pos` values: `noun`, `verb`, `adj`, `adv`, `phrase`, `other`.

---

## Privacy & offline behavior

- **Photos never leave your device** — OCR runs in the browser.
- Network requests go only to the dictionary sources/proxies and public CDNs listed above. Your vault contents are never sent anywhere.
- No tracking, no cookies, no telemetry.

| Capability | Offline? |
|---|---|
| Vault management, flashcards, all quizzes, Sentence Lab | ✅ Fully |
| Word games | ✅ With the built-in word list (extended list loads when online) |
| Lookups of previously cached words | ✅ Instant from cache |
| New dictionary lookups / first photo scan | ❌ Internet required |

---

## Browser support

Recent evergreen browsers — Chrome, Edge, Firefox, and Safari (2023 or newer). The app uses standard web APIs (`localStorage`, Fetch + AbortController, DOMParser, Canvas, WebAudio, SpeechSynthesis, drag & drop). The native file-save dialog is a progressive enhancement with a download fallback. The layout is fully responsive: the sidebar becomes a slide-in drawer on tablets/phones, and game boards scale down for small screens.

---

## Troubleshooting

**Lookups feel slow or fail.**
Public CORS proxies are occasionally rate-limited. The app already races three of them plus a fallback, and caches every success — a word looked up once opens instantly forever after. Check your connection if even the fallback fails.

**Photo scan says "No English words found" or a word has no meaning.**
Try a sharper, closer, well-lit shot. OCR typos and proper names won't match a dictionary — use **Skip**, or add the word manually with your own meaning.

**The scan engine won't load.**
The first scan downloads the OCR engine from jsDelivr; it needs internet once. Afterwards it is browser-cached.

**No pronunciation.**
Unmute with the speaker button in the top bar. Some browsers require a user interaction before audio, and available voices vary by platform. If speech synthesis is unavailable, the Spelling Quiz hint works as a fallback.

**My words disappeared.**
Data is tied to the browser profile and the file's origin. Private/incognito windows clear on close. Export regularly if your collection matters to you.

**Something behaves oddly when opening the file directly.**
Serve it over `http://localhost` (see [Quick start](#quick-start)) — a few browsers restrict CDN or storage behavior on `file://` pages.

---

## Tech stack

- **Vanilla JavaScript** (one IIFE, zero frameworks, zero build tooling)
- **Web APIs:** localStorage · Fetch + AbortController · DOMParser · Canvas / createImageBitmap · WebAudio (procedural sound effects) · SpeechSynthesis (pronunciation) · File System Access (optional) · HTML drag & drop
- **CDN libraries:** Tesseract.js 5.1.1 (OCR) · Font Awesome 6.4 (icons) · Fontsource variable fonts — Inter, Space Grotesk, Fraunces
- **Design:** dark aurora theme with drifting light fields and film grain, glass cards, paper-textured flashcards, palette-tinted scrollbars, staggered motion, and full mobile responsiveness

---

## Code map

```
.
├── vocab-master.html   ← the entire application (HTML + CSS + JS)
└── README.md
```

Inside the single `<script>`, sections are marked with comment banners:

```
helpers → persistent state & migration → toasts → sound & speech → modal
→ navigation → vaults → word list (search/filter/sort/drag/bulk/undo)
→ Cambridge lookup (parallel proxies, cache) → import/export → word drawer
→ flashcards → practice → definition quiz → spelling quiz
→ game dictionary → games hub → Vocabtetris → Vocablocky → Scrabble
→ Spelling Bee → Wordle → photo scan → Sentence Lab
→ global event delegation → drag & drop → keyboard router → init
```

All UI events flow through one delegated `data-action` handler, so every button in every view is wired through the same predictable path.

---

*Built for word lovers. Collect something wonderful today. 🐝*
