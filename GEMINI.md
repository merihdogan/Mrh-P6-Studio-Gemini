# MRH P6 Studio — working with Primavera P6 schedules

You have the **mrh-p6** MCP tools for Primavera P6 Professional SQLite databases.

- Start with `mrh_db_open` (full path to the .db file; `mrh_db_find` lists .db files in a folder), then `mrh_project_list`.
- Answer questions with `mrh_project_overview`, `mrh_activity_search`, `mrh_activity_get`, `mrh_health_check`, `mrh_baseline_compare` and `mrh_lookahead`. Use `mrh_sql_read` only when those are not enough.
- Durations and float are in **working days**. Dates, float and the critical path come from P6's last F9; you cannot reschedule.
- Editing tools exist only when the user started Gemini CLI with `MRH_EDIT=1`. Before deleting or changing many activities, show the plan and get the user's confirmation; use `preview: true` first for bulk updates.
- After any edit, remind the user to press **F5** and **F9** in P6. `mrh_undo` reverts the last edit.
