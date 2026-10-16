# Faroz — MCP server & Gemini CLI extension

Connect AI assistants (Claude, ChatGPT, Gemini and any MCP client) to [Faroz](https://faroz.uz), a learning platform for children aged 8–17. Teachers, parents and content authors use it to plan lessons and create learning content by simply talking to the assistant.

**MCP endpoint:** `https://faroz.uz/mcp` (Streamable HTTP, OAuth 2.0 with dynamic client registration)
**Official MCP Registry:** `uz.faroz/faroz`

## What you can do

| Area | Capabilities |
|------|--------------|
| Courses | Browse the catalog and your own courses, search, open full details (lessons, exercises, subject, level, age range, price). Create courses and edit title, description, subject, level, age range, tags and visibility. |
| Lessons | Add lessons to a course, edit them, attach up to 5 exercises to each lesson, and reorder the course plan. |
| Interactive exercises | Turn any text, topic or lesson plan into ready-to-use exercises. Each exercise is checked against the Faroz format before saving, so it opens correctly in the mobile and web apps. |
| Games | Browse game categories and games. Authors with the `games` permission can build games from 1–20 exercises. |
| Library | Browse the subject tree and your media library (video, audio, images) to reuse in exercises. |

### Exercise block types (11)
Text multiple choice · picture multiple choice · fill in the blanks · typed answer · speak-aloud answer (speech recognition) · image · image gallery · audio · video · styled rich text · callout boxes (rules, examples, tips)

## Tools

**Read-only:** `whoami`, `search`, `fetch`, `list_courses`, `get_course`, `get_course_filters`, `list_exercises`, `get_exercise`, `get_exercise_format_guide`, `validate_exercise_body`, `list_game_categories`, `list_games`, `list_media`, `list_subjects`

**Create:** `create_course`, `add_lesson`, `create_exercise`, `create_game`

**Edit (overwrites existing content):** `update_course`, `update_lesson`, `reorder_lessons`, `update_exercise`, `update_game`

There are **no delete tools**. Every call runs as the signed-in user, with exactly the same permissions as in the Faroz app.

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
- "Here is a paragraph about the water cycle. Make a multiple-choice, a fill-in-the-blank and a typed-answer exercise from it, then add them to lesson 2."
- "Open my fractions course, check every lesson's exercises for mistakes or unclear questions, and fix what you find."
- "Show my courses and list the exercises in the first lesson."

## Requirements
A free Faroz account: sign up at [faroz.uz](https://faroz.uz). No API key is needed.

## Roadmap
Planned teacher tools, not available yet:
- View students' lesson results, attendance and mastery ratings in your courses
- Check submitted assignments and see the results
- Send feedback to students
- Help students with lessons and learning progress

These tools will act only on students in the teacher's own courses.

## Privacy & support
- Privacy policy: https://faroz.uz/privacy
- Terms: https://faroz.uz/terms
- Support: akramjonminochdinov@gmail.com
