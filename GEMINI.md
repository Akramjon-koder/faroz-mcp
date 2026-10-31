# Faroz

Faroz (https://faroz.uz) is a learning platform for kids aged 8–17. It has courses, lessons, exercises and games.
The `faroz` MCP server acts on behalf of the signed-in Faroz user, with the same permissions as in the app.

- Content hierarchy: Course → Lessons (≤5 exercise IDs each); Game category → Game (1–20 exercises).
- **Building a course (mandatory):** before `create_course` / `add_lesson`, read `get_exercise_format_guide(topics=["course_authoring"])` and follow it step by step — fill every course field incl. `banner_url`, 4–6 lessons with 2–3 exercises each (theory → practice → test), all 11 content block types across the course (`column` / `row` layouts are optional helpers for stacking or side-by-side blocks), a styled `quill_text` question above every answer block, homework on 1–2 lessons, build with `is_active=false`, re-check with `get_course`, then publish with `update_course(is_active=true)`. Follow the `next_steps` returned by `create_course` and fix every `validation_warnings` item returned by `create_exercise`.
- The `create_course` MCP prompt (`/create_course <topic>`) runs the same playbook.
- Before creating an exercise, call `get_exercise_format_guide` (at least `tool_schemas` and `ui_layout_rules`), then check the body with `validate_exercise_body`.
- Besides block pages, an exercise can hold typed pages: `grammar_check` (AI grammar check of photographed writing; mixes with block pages) and `language_speaking` (one speaking exam part; the only page). Read `get_exercise_format_guide(topics=["page_types"])` first. When editing, send typed pages back unchanged with their `id`.
- Write in the language the user asks for (Uzbek or English). Do not mix languages in one text.
- Attaching exercises to lessons: search with `list_exercises(scope="global", query=<ID, title, description or category>)`. An exercise with `locked: true` belongs to another teacher and is not available to the user yet — do not attach it; follow its `suggestion` and tell the user.
- Lesson calendar: `get_course_schedule`, then `reschedule_lesson` / `add_calendar_lesson` / `set_group_schedule`. Times are Tashkent time (`YYYY-MM-DDTHH:MM`).
- Recommendations & strategies: collect fitting recommendations with `search(types=["recommendation"])`, create missing ones with `create_recommendation` (age range + dates), then `create_strategy(recommendation_ids=[...], age_min, age_max)`.
- The server has no delete tools and never shows or changes prices, subscriptions or payments.
