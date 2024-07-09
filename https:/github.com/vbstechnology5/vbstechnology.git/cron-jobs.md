# Cron Jobs

1. **Minute (0-59)**: Specifies the minute when the task should run. It can be any value from 0 to 59.
2. **Hour (0-23)**: Specifies the hour when the task should run. It can be any value from 0 to 23.
3. **Day of the Month (1-31)**: Specifies the day of the month when the task should run. It can be any value from 1 to 31.
4. **Month (1-12 or names)**: Specifies the month when the task should run. It can be any value from 1 to 12 or month names (e.g., "jan," "feb," etc.).
5. **Day of the Week (0-7 or names)**: Specifies the day of the week when the task should run. Days can be represented as numbers (0=Sunday, 1=Monday, ..., 6=Saturday) or day names (e.g., "sun," "mon," etc.). Note that both 0 and 7 represent Sunday.

A cron job schedule is written as a space-separated list of these five fields. For example, the following schedule:

```bash
30 1 * * 4
```

Means "Run a task at 1:30 AM every Thursday." The first field represents the minute (30), the second field is the hour (1), the third field allows any day of the month (_), the fourth field allows any month (_), and the fifth field specifies Thursday (4).
