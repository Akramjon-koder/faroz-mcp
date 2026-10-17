# Faroz — MCP server & Gemini CLI extension

Connect AI assistants (Claude, ChatGPT, Gemini and any MCP client) to [Faroz](https://faroz.uz), a learning platform for children aged 8–17. Teachers, parents and content authors use it to plan lessons, create learning content and check students' homework by simply talking to the assistant.

**MCP endpoint:** `https://faroz.uz/mcp` (Streamable HTTP, OAuth 2.0 with dynamic client registration)
**Official MCP Registry:** `uz.faroz/faroz`

## What you can do

| Area | Capabilities |
|------|--------------|
| Courses | Browse the catalog and your own courses, search, open full details (lessons, exercises, subject, level, age range, price). Create courses and edit title, description, subject, level, age range, tags and visibility. |
| Lessons | Add lessons to a course, edit them, attach up to 5 exercises to each lesson, reorder the course plan, and switch on homework with a task text. |
| Homework review | List students' submissions, open one to see the task, the student's comment, the photos and the text of PDF / Word / PowerPoint files, then grade it (0–100) with feedback the student sees in the app, or send it back for rework. Grades feed the student's mastery. |
| Student progress | See a course's students ranked by mastery (no phone numbers or emails). |
| Interactive exercises | Turn any text, topic or lesson plan into ready-to-use exercises. Each exercise is checked against the Faroz format before saving, so it opens correctly in the mobile and web apps. |
| Games | Browse game categories and games. Authors with the `games` permission can build games from 1–20 exercises. |
| Library | Browse the subject tree and your media library (video, audio, images) to reuse in exercises. |

### Exercise block types (11)
Text multiple choice · picture multiple choice · fill in the blanks · typed answer · speak-aloud answer (speech recognition) · image · image gallery · audio · video · styled rich text · callout boxes (rules, examples, tips)

## Tools

**Read-only:** `whoami`, `search`, `fetch`, `list_courses`, `get_course`, `get_course_filters`, `list_course_students`, `list_homework_submissions`, `get_homework_submission`, `list_exercises`, `get_exercise`, `get_exercise_format_guide`, `validate_exercise_body`, `list_game_categories`, `list_games`, `list_media`, `list_subjects`

**Create:** `create_course`, `add_lesson`, `create_exercise`, `create_game`

**Edit (overwrites existing content):** `update_course`, `update_lesson`, `reorder_lessons`, `update_exercise`, `update_game`, `review_homework`

There are **no delete tools**. Every call runs as the signed-in user, with exactly the same permissions as in the Faroz app. Homework tools work only for the course's staff (teacher, support teacher, course admin).

## Building a complete course (built-in playbook)
Every client gets the same course-authoring playbook automatically — no setup needed:

- **Server instructions** tell the assistant to read it before creating a course.
- **Tool guide:** `get_exercise_format_guide(topics=["course_authoring"])` returns the full playbook (also the resource `faroz://docs/exercise/course_authoring`).
- **Prompt:** `create_course` (arguments: `topic`, optional `audience`, `lessons`) — in Claude pick it from the prompt menu, in Gemini CLI run `/create_course`.
- **Tool results:** `create_course` returns `next_steps`; `create_exercise` / `update_exercise` return `validation_warnings` (e.g. a test without a question, unstyled text) so the assistant fixes them.
- **Skill file:** [`skills/faroz-course-builder/SKILL.md`](skills/faroz-course-builder/SKILL.md) — upload it as a skill in Claude, or use it from the ChatGPT/Codex plugin.

What the playbook enforces: full course metadata and a banner, 4–6 lessons with 2–3 exercises each (theory → practice → test, final test at the end), all 11 block types across the course, a question above every answer block, styled text and callouts, verified image/video/audio links, homework on 1–2 lessons, building with `is_active=false` and publishing only after a final check.

## Homework
Students submit homework for a lesson in the Faroz app: images, audio, PDF, Word or presentations, up to 20 MB in total, plus an optional comment. Files are stored privately and served only through signed links that expire. Teachers review them in the app or with an AI assistant through this server.

## Install

**Claude:** Settings → Connectors → *Add custom connector* → URL `https://faroz.uz/mcp` → Connect → sign in with your Faroz account.

**ChatGPT:** Settings → Apps & Connectors → Advanced → Developer mode → *Create* → MCP Server URL `https://faroz.uz/mcp`, Authentication: OAuth.

**Gemini CLI**
```bash
gemini extensions install https://github.com/Akramjon-koder/faroz-mcp
```

**Any MCP client:** add a remote server with the URL `https://faroz.uz/mcp`. The client registers itself (RFC 7591), then opens the Faroz sign-in page (email/password or Google).

## Example prompts
- "Create a 5th-grade math course on fractions with 4 lessons and 2 exercises in each lesson."
- "Make a beautiful course about the Solar System for 9–13 year olds." (the assistant follows the course playbook: banner, video, pictures, tests, homework)
- "Here is a paragraph about the water cycle. Make a multiple-choice, a fill-in-the-blank and a typed-answer exercise from it, then add them to lesson 2."
- "Open my fractions course, check every lesson's exercises for mistakes or unclear questions, and fix what you find."
- "Show my courses and list the exercises in the first lesson."
- "Check all homework waiting for review in my fractions course, grade each one and write short feedback for every student."
- "Who are the top 5 students in my course by mastery?"

## Requirements
A free Faroz account: sign up at [faroz.uz](https://faroz.uz). No API key is needed.

## Roadmap
Possible next steps:
- Per-student progress reports for teachers and parents
- Help for students with their lessons and learning progress

## License
The files in this repository (extension manifest, context file and skills) are released under the [MIT License](LICENSE). The Faroz platform and the MCP server behind `https://faroz.uz/mcp` are proprietary services.

## Privacy & support
- Privacy policy: https://faroz.uz/privacy
- Terms: https://faroz.uz/terms
- Support: akramjonminochdinov@gmail.com
