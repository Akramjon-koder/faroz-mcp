---
name: faroz-course-builder
description: Build a complete, beautiful Faroz course through the Faroz MCP tools (create_course, add_lesson, create_exercise, list_media, list_subjects, get_course_filters, validate_exercise_body, update_course, reorder_lessons…). ALWAYS use when the user asks to create/make/build a course, lessons, or a lesson set on any topic in Faroz ("kurs yarat", "kurs tuz", "darslar qil", "make a course about…"), or to extend/improve an existing Faroz course. Ensures every tool is used, full metadata + banner, 4–6 lessons with theory/practice/test exercises, all 11 block types, styled text, homework, validation and a final checklist before publishing.
---

<!-- Body is a copy of backend/edu_app/api/mcp/docs/course_authoring.md (served by the MCP
     server as the `course_authoring` guide topic and the `create_course` prompt).
     Edit that file, then regenerate: .claude/skills/faroz-course-builder/sync.sh -->

# Course authoring — building a complete, beautiful Faroz course

Use this playbook whenever the user asks to **create a course** (or "make lessons/exercises for a topic") through the Faroz MCP tools. A course is judged by the child who opens it on a phone: it must look finished (banner, description, lesson descriptions), feel alive (styled text, callouts, pictures, video), and make the child *do* things (several different exercise types, a test, homework).

The most common failure — and what this playbook prevents — is a "minimal" course: one lesson, one exercise, plain unstyled text, `title_test` without a question, no banner, no metadata, no video, half the tools never used.

---

## 0. Defaults when the user doesn't say

| Decision | Default |
|---|---|
| Number of lessons | **4–6** (a single-topic request still gets ≥ 3 lessons: intro → core → practice/test) |
| Exercises per lesson | **2–3** (max 5): ① theory ② practice ③ mini-test (the last lesson ends with a final test) |
| Pages per exercise | theory 3–5 · practice 2–4 · test 1–2 |
| Language | the language the user wrote in (Uzbek Latin by default). Never mix languages inside one string. Use one apostrophe style consistently (`o'`, `g'`). |
| Age range | infer from topic/grade (e.g. 7-sinf → 12–14); never leave 0–99 |
| Price | `free` unless the user asks |
| Exercise access | `private` (course students can still open it — lessons grant access) unless the user wants it reusable → `free` |
| First lesson | `is_public: true` (free preview for non-members); the rest `false` (unlocked at 80 % progress) |
| Homework | on for 1–2 lessons where a product is natural (a drawing, a solved problem set, a recording) |

Only ask the user a question if the topic itself is unclear. Otherwise build, then report.

---

## 1. Use every tool — the workflow

Run the steps in order. Each step names the tool(s); skipping a step is the usual cause of an ugly course.

1. **`whoami`** — confirm who you act as and their permissions.
2. **`list_courses(scope="mine", search=<topic>)`** — avoid duplicating an existing course; if one exists, ask whether to extend it (`update_course` / `add_lesson`) instead.
3. **`list_subjects`** + **`get_course_filters`** — pick `subject` from the subject tree **by exact name** (prefer the leaf, e.g. `Mexanika` under `Fizika`) and **reuse existing** `categories` / `genres` / `tags` spellings (don't create `matematika` next to `Matematika`).
4. **`get_exercise_format_guide(topics=["tool_schemas","ui_layout_rules","quill_text_styling","container_usage","gallery_usage"])`** — read **all five** topics once before writing the first body.
5. **`list_media(media_type="videos"|"audios"|"images", query=<topic>, scope="usable")`** — look for platform media first. A video from the library (YouTube links work in the `video` block) beats any external link. If nothing fits, use stable public URLs (see §5).
6. **`list_exercises`** / **`get_exercise`** — optional: open a good existing exercise as a style sample, and reuse an existing exercise when it fits a lesson.
7. **`create_course`** — fill **every** field: `title`, `description`, `subject`, `categories`, `genres`, `tags`, `level`, `age_min`, `age_max`, `price_type`, `banner_url`, and `is_active: false` while building.
8. For each lesson:
   1. Write each exercise body → **`validate_exercise_body`** → fix **all errors and all warnings** → **`create_exercise`** (with `description` and `group_categories`).
   2. **`add_lesson`** with `title` ("1-dars. …"), a one-sentence `description`, `exercise_ids` (in play order), `is_public`, and `homework_enabled` + a concrete `homework_description` where planned.
9. **`reorder_lessons`** if lessons were added out of order; **`update_lesson`** / **`update_exercise`** to fix anything found in review.
10. **`get_course`** — re-read the whole course and run the §7 checklist.
11. **`update_course(is_active=true)`** — publish only after the checklist passes.
12. Report to the user: course link (`fetch`/`search` results include the web URL `…/user/course/<id>`), lesson list with exercise types used, homework, and anything you could not do (e.g. no suitable video found).

---

## 2. Course metadata — make the card look finished

- **Title**: short, catchy, topic first — `"Fizika: Nyuton qonunlari"`, `"Kasrlar sirlari"`. ≤ 40 chars.
- **Description**: 2–3 sentences: what the child learns + how (video, pictures, tests, homework). No marketing fluff.
- **Banner** (`banner_url`): a wide, bright, topic-relevant image (landscape, ≥ 1000 px wide). A course without a banner looks broken in the catalog.
- **Level**: `Boshlang'ich` / `O'rta` / `Yuqori` (match the existing spelling from other courses).
- **Tags**: 3–6 lowercase search words.

---

## 3. Lesson recipe

Every lesson follows the same arc so the child learns the rhythm:

| Exercise | Purpose | Pages (typical) |
|---|---|---|
| ① **Theory** — `"<Mavzu>: tushuntirish"` | hook → explanation → rule/formula → worked example | 1. hook page (`quill_text` header + question, `image` or `video`) · 2–3. explanation (`quill_text` + `container` rule/formula + `gallery`/`image`) · last: quick check (`quill_text` question → `title_test`) |
| ② **Practice** — `"<Mavzu>: mashqlar"` | apply the rule with varied input types | `placeholder_exercise`, `typing` (with numbered titles), `image_test`, `speech_exercise` for language/reading topics, `audio` for listening |
| ③ **Mini-test** — `"<Mavzu>: test"` | assess — test mode (§13 of ui_layout_rules) | (`quill_text` question → `title_test`) × 5–10, mixed with 1–2 `typing` / `image_test` |

The last lesson's test is a **final test** covering the whole course (10–15 questions).

### Coverage targets

Per **lesson**: styled `quill_text` · ≥ 1 `container` · ≥ 1 visual (`image` / `gallery` / `video`) · **≥ 3 different answer-input types** (`title_test`, `image_test`, `placeholder_exercise`, `typing`, `speech_exercise`).

Per **course**: use **all 11 block types** at least once, unless a type truly doesn't fit (e.g. `speech_exercise` in a math course — then say so in the report). `video` at least once per course (ideally the first page of each lesson). `audio` for language, music, reading, pronunciation topics.

---

## 4. Making pages beautiful

- **Every `quill_text` is styled** (`quill_text_styling`): a `header` line with a sticker (`📖 Inersiya`), **bold** key terms, colored formulas/symbols (`#3949AB`), em-dash pairs colored. A paragraph with no `attributes` is a bug (validator warns `styled_text`).
- **Callouts** with `container` (`container_usage`): rule → blue `standart`; formula → indigo `line`; example → green `left_line` + lightbulb; tip → orange; warning → red + crosshairs. Same role = same color across the whole course. ≤ 2 containers per page.
- **Question first**: every `title_test`, `image_test`, `placeholder_exercise`, `speech_exercise` and untitled `typing` has **its own** `quill_text` question right above it (validator warns `question_first`). The question starts with `❓` or `🤔` and is bold.
- **Spacing**: a `quill_text` after an answer tool starts with `{"insert":"\n"}` (`answer_sep`); before a titled `typing` too (`title_sep`).
- **`title_test` options**:
  - Short answers (numbers with units, single words) → keep **every** option **< 12 characters** (trimmed). The app then shows them in a **2-column grid** — compact and nicer. Use an **even** count (2, 4, 6).
  - Sentence answers → 3–4 options, single column.
  - Plausible distractors (common mistakes), not jokes. Vary `answer_index` across questions — don't always put the right answer in the same slot.
- **`image_test`**: exactly 4 cards, each image clearly different. When the question is "which picture shows X?", card titles must be neutral (`A`, `B`, `C`, `D`) — a card titled with the answer (`"Saturn"`) gives it away. Use descriptive titles only when the question asks about something other than the name.
- **`gallery`**: 2, 3, 4, 6 or 9 images, with `symvol_type: "A-Z"` when the text refers to them ("A rasmda…").
- **`placeholder_exercise`**: blanks are key words; `start` offsets must be exact (`previous.start + previous.text.length`); 2–4 distractors in `extraItems`.
- **Footprint**: lesson pages 2–6 blocks; split long theory into more pages instead of one tall page.
- **`speech_exercise`**: speech recognition runs in **English** (`en`) — the `answer` must be an English phrase (≤ 60 chars), e.g. an English quote or vocabulary. For an Uzbek-only topic, use it for a famous English quote/term, or skip it and say so.
- **Points**: 1 for easy, 2 for calculation/multi-step questions.

---

## 5. Media URLs

1. Prefer `list_media` results (platform-hosted, always reachable).
2. Otherwise use stable public images — `https://images.unsplash.com/photo-<id>?w=1000` (banner/hero) or `?w=600` (gallery, cards), or Wikimedia Commons thumbnails for science photos/diagrams. **Only use an image you know matches the topic**; an unrelated stock photo is worse than none. If you can fetch URLs, check them first (HTTP 200 + `image/*`).
   - Wikimedia thumbnails work **only at standard widths**: `…/thumb/<a>/<ab>/<File>/500px-<File>` (also `250px`, `960px`, `1280px`). Other widths (`400px`, `600px`) return **HTTP 400** and the image is blank. SVG files: use the PNG thumb `…/500px-<File>.svg.png` — the app cannot draw raw `.svg`.
3. Videos: YouTube watch URLs are supported by the `video` block. Use only videos you are confident exist and fit the age (verify with `https://www.youtube.com/oembed?url=<watch-url>&format=json` when you can fetch); otherwise skip the video and mention it in the report.
4. Audio: direct `.mp3` links only (`just_audio`; `.ogg` fails on iOS). The library is often empty — public-domain sources such as nasa.gov mp3 files work.
5. Never put image/video embeds inside `quill_text` (they don't render on mobile) — use the `image` / `gallery` / `video` blocks.

---

## 6. Homework

- Turn it on where the child can produce something: solve 3 problems in a notebook and photograph it, draw a diagram, record pronunciation, write 5 sentences.
- `homework_description` is concrete: what to do, how much, what to upload ("Daftaringizga 3 ta masalani yeching va rasmini yuklang"). The student can upload images, audio, PDF, Word or slides (≤ 20 MB total).

---

## 7. Final checklist (run on `get_course` before publishing)

1. Course: banner, description, subject, categories, genres, tags, level, age range all set; `is_active` flipped to `true` at the end.
2. ≥ 3 lessons (default 4–6), each with a description; first lesson `is_public: true`.
3. Each lesson has 2–3 exercises in the order theory → practice → test; the last lesson ends with a final test.
4. `validate_exercise_body` returned **0 errors and 0 warnings** for every exercise (or each remaining warning is deliberate and explained).
5. Course uses all 11 block types (or the report explains which ones don't fit and why); ≥ 1 video.
6. Every answer-input block has its own question; short `title_test` options are < 12 chars with an even count.
7. Homework on 1–2 lessons with a concrete description.
8. The report lists: course link, lessons → exercise ids + block types used, homework, and gaps.
