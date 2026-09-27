# Mastery state

One line per domain. Level 0–4 (see the ladder in SKILL.md). Written by the agent
whenever a level changes; read at session start. Absent file = every domain starts at 0.

Format — `domain: level` (last updated: YYYY-MM-DD)

```
# example only — replace, do not keep:
# oauth-token-refresh: 2
```

Domains are free-form and per-subject. A new domain always starts at 0; never carry
a level across from a related one.
