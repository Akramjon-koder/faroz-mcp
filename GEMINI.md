# Faroz

Faroz (https://faroz.uz) is a learning platform for kids aged 8–17. It has courses, lessons, exercises and games.
The `faroz` MCP server acts on behalf of the signed-in Faroz user, with the same permissions as in the app.

- Content hierarchy: Course → Lessons (≤5 exercise IDs each); Game category → Game (1–20 exercises).
- Before creating an exercise, call `get_exercise_format_guide` (at least `tool_schemas` and `ui_layout_rules`), then check the body with `validate_exercise_body`.
- Write in the language the user asks for (Uzbek or English). Do not mix languages in one text.
- The server has no delete tools.
