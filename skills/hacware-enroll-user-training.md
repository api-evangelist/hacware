---
name: Enroll a user in security training
description: Authenticate, list training courses, enroll a user, remind them, and track training completion status.
api: openapi/hacware-openapi.yml
operations: [PostAPIAuth, GetCourseList, AddCourseMember, PostSendTrainReminder, PostTrainStatus]
---

# Enroll a user in security training

Assign HacWare micro-training to an employee and follow their completion.

## Steps
1. **Authenticate** — `POST /api/v1/auth/` (`PostAPIAuth`) with `appid` + `sec`; use the returned access token as `Authorization: Bearer <access_token>`.
2. **List courses** — `GET /api/v1/train/courselist/` (`GetCourseList`) to choose the course to assign.
3. **Enroll the user** — `POST /api/v1/train/add_course_member/` (`AddCourseMember`) with the user's email and the target course.
4. **Nudge the user** — `POST /api/v1/train/reminder/` (`PostSendTrainReminder`) to send a training reminder.
5. **Track completion** — `GET /api/v1/train/status/` (`PostTrainStatus`) to read the user's training status/progress.

## Conventions & error handling
- Bearer auth (`authentication/hacware-authentication.yml`); 401 => refresh token and retry.
- 4xx errors carry `{"message": "..."}` (`errors/hacware-problem-types.yml`).
- List endpoints paginate with `page_num` / `page_size` (`conventions/hacware-conventions.yml`).
