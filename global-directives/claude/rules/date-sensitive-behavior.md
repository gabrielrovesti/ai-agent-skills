---
paths:
  - "**/*Scheduler*"
  - "**/*Cron*"
  - "**/*ScheduledJob*"
  - "**/*CronJob*"
  - "**/*schedule*"
  - "**/*.cron"
---

# Date-sensitive and scheduler-sensitive behavior

If behavior depends on schedulers, async jobs, propagation delays, daily availability, date windows, or delayed automation:
- separate immediate effects from delayed effects
- do not generalize from partial or one-day evidence
- validate date-sensitive claims day-by-day when relevant

Do not present simplified timing assumptions as proven behavior.