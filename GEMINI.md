# Faroz

Faroz (https://faroz.uz) is a learning platform for kids aged 8–17. It has courses, lessons, exercises and games.
The `faroz` MCP server acts on behalf of the signed-in Faroz user, with the same permissions as in the app.

- Content hierarchy: Course → Lessons (≤5 exercise IDs each); Game category → Game (1–20 exercises).
- **Building a course (mandatory):** before `create_course` / `add_lesson`, read `get_exercise_format_guide(topics=["course_authoring"])` and follow it step by step — fill every course field incl. `banner_url`, 4–6 lessons with 2–3 exercises each (theory → practice → test), all 11 block types across the course, a styled `quill_text` question above every answer block, homework on 1–2 lessons, build with `is_active=false`, re-check with `get_course`, then publish with `update_course(is_active=true)`. Follow the `next_steps` returned by `create_course` and fix every `validation_warnings` item returned by `create_exercise`.
- The `create_course` MCP prompt (`/create_course <topic>`) runs the same playbook.
- Before creating an exercise, call `get_exercise_format_guide` (at least `tool_schemas` and `ui_layout_rules`), then check the body with `validate_exercise_body`.
- Write in the language the user asks for (Uzbek or English). Do not mix languages in one text.
- The server has no delete tools.
