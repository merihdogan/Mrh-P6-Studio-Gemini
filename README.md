<p align="center"><img src="mrh-logo.svg" width="72" alt=""></p>

# MRH P6 for Gemini CLI

Ask **Gemini CLI** about your **Primavera P6 Professional** schedules.
MRH P6 is an open-source MCP server for the P6 SQLite database on your computer.

## Install

```bash
gemini extensions install https://github.com/merihdogan/mrh-p6-gemini
```

Needs `git` and **Node.js 22.13+**. Start `gemini` and ask: *"Open C:\\P6\\PPMDBSQLite.db and summarize the project."*
Update with `gemini extensions update mrh-p6`.

**Editing** is off by default. To allow it for a session, start Gemini CLI with the variable set:

```bash
set MRH_EDIT=1 && gemini
```

(PowerShell: `$env:MRH_EDIT="1"; gemini` · macOS/Linux: `MRH_EDIT=1 gemini`)

## What you can ask

- *"Open C:\\P6\\PPMDBSQLite.db and summarize the project open in P6."*
- *"Which critical activities finish in the next 3 weeks?"*
- *"Compare with the baseline and list the 10 activities that slipped most."*
- *"Run a schedule health check and explain the failed checks."*
- *"Mark A1020 complete with actual dates 3–14 March."* (editing enabled)

## Tools

| Tool | What it does |
|---|---|
| `mrh_db_find` | List P6 SQLite (.db/.sqlite) files in a folder. |
| `mrh_db_open` | Open a Primavera P6 Professional SQLite database by full path. |
| `mrh_project_list` | List the projects in the open database with data date, finish, baseline project and activity count.. |
| `mrh_project_overview` | Progress (duration weighted), planned progress from the baseline, forecast vs baseline finish, status counts, critical/negative-float counts and progress per top-level WBS.. |
| `mrh_activity_search` | Filter and list activities. |
| `mrh_activity_get` | Full details of one activity: dates, durations, float, constraints, predecessors and successors, resource assignments, activity codes, user fields and notes.. |
| `mrh_health_check` | Run 15 schedule quality checks (missing logic, leads, lags, non-FS links, hard constraints, high/negative float, long durations, invalid dates, status errors, duplicate links, loops, redundant links, critical activities) and return a 0-100 score with sample activity codes per check.. |
| `mrh_baseline_compare` | Activities whose current finish is later than the baseline finish, worst first.. |
| `mrh_lookahead` | Activities that start or finish within N days after the data date.. |
| `mrh_sql_read` | Run one read-only SELECT/WITH query on the P6 database (P6 table names such as TASK, TASKPRED, PROJWBS, CALENDAR, RSRC, TASKRSRC). |
| `mrh_xer_export` | Write a project to an XER file (the database is not changed). |
| `mrh_backup_list` | List the automatic backups of the open database (taken before edits, kept next to the database in mrh_yedekler).. |
| `mrh_activity_update` ✏️ | Apply the same changes to one or many activities in ONE transaction (all or nothing). |
| `mrh_activity_create` ✏️ | Create a not-started activity (code is generated when omitted). |
| `mrh_activity_delete` ✏️ | Delete activities together with their links, resource assignments, codes, user field values and notes. |
| `mrh_link_add` ✏️ | Add a relationship between two activities. |
| `mrh_link_update` ✏️ | Change the type and/or lag of a relationship (link_id from mrh_activity_get).. |
| `mrh_link_delete` ✏️ | Delete relationships by link_id (from mrh_activity_get).. |
| `mrh_data_date_set` ✏️ | Set the project data date (YYYY-MM-DD). |
| `mrh_undo` ✏️ | Revert the last edit made in this session. |

✏️ = editing tool, only available when editing is enabled (`MRH_EDIT=1`).

## Safety

- **Read-only by default.** Editing tools are hidden until editing is enabled.
- A compressed **backup** is written next to the database (`mrh_yedekler\\<name>`) before the first edit, and every edit runs in a single transaction (all or nothing).
- `mrh_undo` reverts the last edit; it refuses if the same rows were changed elsewhere afterwards.
- Dates, float and the critical path come from P6. After editing, press **F5** (refresh) and **F9** (schedule) in P6.
- Direct database edits bypass P6's own validation and are not supported by Oracle. **Work on a copy first**, and make sure sharing project data with an AI service is allowed by your company.

## Requirements

- **Node.js 22.13 or newer** (uses the built-in `node:sqlite`; no npm packages). Check with `node --version`.
- A Primavera P6 **Professional SQLite** database. SQL Server / Oracle databases are not supported.

## Try it without P6

```bash
node araclar/dgn-demo-olustur.js ornek/mrh-demo.db
```

creates a synthetic demo database (65 activities, baseline, progress) you can open with `mrh_db_open`.

## Tests

```bash
npm test
```

## Türkçe

**MRH P6**, Primavera P6 Professional SQLite veritabanlarını Gemini CLI ile konuşarak incelemenizi ve (izin verirseniz) düzenlemenizi sağlayan açık kaynak bir MCP sunucusudur.

- Varsayılan olarak **salt okunurdur**; düzenleme için `MRH_EDIT=1` gerekir.
- İlk düzenlemeden önce veritabanının yanındaki `mrh_yedekler` klasörüne otomatik yedek alınır; `mrh_undo` son işlemi geri alır.
- Düzenlemeden sonra P6'da **F5** ve **F9** yapın. Önce veritabanınızın bir **kopyası** üzerinde deneyin.
- Gereksinim: **Node.js 22.13+**. Ek paket gerekmez.

**Kurulum:** `gemini extensions install https://github.com/merihdogan/mrh-p6-gemini`
Düzenleme için Gemini CLI'ı `MRH_EDIT=1` ortam değişkeniyle başlatın.

---

Made by **Merih Doğan** · MIT License · Part of [MRH P6 Studio](https://github.com/merihdogan).
Primavera and P6 are trademarks of Oracle and/or its affiliates. This project is independent and not affiliated with Oracle, Anthropic, OpenAI or Google.
