# Faroz — MCP server / Gemini CLI extension

Connect AI assistants to [Faroz](https://faroz.uz), a learning platform for kids aged 8–17.

**MCP endpoint:** `https://faroz.uz/mcp` (Streamable HTTP, OAuth 2.0 with dynamic client registration)

## Tools
- **Read:** `whoami`, `search`, `fetch`, `list_courses`, `get_course`, `get_course_filters`, `list_exercises`, `get_exercise`, `get_exercise_format_guide`, `validate_exercise_body`, `list_game_categories`, `list_games`, `list_media`, `list_subjects`
- **Write:** `create_course`, `update_course`, `add_lesson`, `update_lesson`, `reorder_lessons`, `create_exercise`, `update_exercise`, `create_game`, `update_game`
- There are no delete tools. All calls run with the signed-in user's permissions.

## Install

**Gemini CLI**
```bash
gemini extensions install https://github.com/Akramjon-koder/faroz-mcp
```

**Claude / ChatGPT / any MCP client:** add a custom connector with the URL `https://faroz.uz/mcp`, then sign in with your Faroz account (email/password or Google).

**Official MCP Registry:** `uz.faroz/faroz`

## Example prompts
- "Create a 5th-grade math course on fractions with 3 lessons"
- "Turn this text into 3 multiple-choice exercises"
- "Show my courses"

## Privacy & support
- Privacy policy: https://faroz.uz/privacy
- Terms: https://faroz.uz/terms
- Support: akramjonminochdinov@gmail.com
