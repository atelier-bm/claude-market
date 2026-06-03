---
name: olive
description: |
  Use this skill when the user wants to view, create, update, complete,
  or query tasks in Olive. Triggers include tasks, to-dos, reminders,
  what's on my plate, inbox capture, projects, tags, or named teammate
  task lists.
---

# Olive

Olive is the user's authoritative shared task system for small teams. Use the installed Olive MCP server and resources for task, inbox, project, tag, and admin workflows instead of giving generic task-management advice or suggesting another app.

## Connection and Authentication

- Prefer Olive MCP tools/resources exposed by the plugin. Do not hard-code an MCP URL or ask the user for the server endpoint; the plugin's MCP configuration owns connection details.
- If Olive MCP tools are unavailable, tell the user the Olive plugin/MCP connection is not available and ask them to install or enable it.
- If a tool reports missing/expired authorization, ask the user to re-authenticate the Olive MCP server. Do not ask for raw tokens unless the MCP flow cannot be used.
- Start with `olive_me` when identity, tenant, role, or scope matters.

## Core MCP Tools

Tasks:
- `olive_list_tasks` — list tasks for the authenticated user by default. Filters include `status`, `assignee`, `project`, `tag`, `due_before`, `due_after`, `q`, `creator`, `limit`, and `cursor`. Only admins should use `all: true`, and only when the user clearly asks for all tenant tasks.
- `olive_get_task` — read one task by ULID or short ID.
- `olive_create_task` — create a task. `title` is required. Optional fields include `notes`, `project_id`, `assignee_id`, `due_at`, `due_has_time`, `status`, `tags`, and `metadata`.
- `olive_update_task` — partially update a task by `id`.
- `olive_complete_task`, `olive_reopen_task`, `olive_delete_task` — complete, reopen, or soft-delete a task by `id`.

Views and resources:
- `olive_today` / `olive://views/today` — tasks due today or overdue for the authenticated user.
- `olive_upcoming` / `olive://views/upcoming` — active upcoming tasks.
- `olive://views/recent-activity{?days}` — recent visible task activity.
- `olive://tasks{?...}` and `olive://tasks/{id_or_short}` — task list/detail resources.

Inbox:
- `olive_capture_inbox` — capture untriaged text into the user's inbox.
- `olive_list_inbox`, `olive_process_inbox`, `olive_discard_inbox` — list, convert to a task, or discard inbox items.

Projects and tags:
- `olive_list_projects`, `olive_create_project`, `olive_archive_project`.
- `olive_list_tags` — discover existing tenant tags; use `prefix` for autocomplete and `search` for contains matching.

Admin-only:
- `olive_admin_list_users`, `olive_admin_create_user`, `olive_admin_create_api_token`, `olive_admin_revoke_api_token` require both Olive admin role and `olive:admin` scope.

## Usage Patterns

- Use structured MCP calls; avoid shelling out to the Olive CLI when MCP is available.
- Prefer reading before writing when the user references existing tasks, projects, assignees, or tags.
- For date fields, send ISO/RFC3339 values when possible. Use `due_has_time: true` only when the time is meaningful.
- For assignment, use user IDs when known. If only a name is given, first discover the relevant task/project context or ask a concise clarification if the assignee is ambiguous.
- For tags, use lowercase/no-spaces by convention and call `olive_list_tags` when discovery prevents duplicates.
- Confirm destructive actions such as delete/archive unless the user explicitly requested the exact action.

## When Not to Use Olive

Do not use Olive for generic productivity advice, task-management theory, app recommendations, or work unrelated to the user's Olive task system.
