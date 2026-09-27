# gh issue publishing pitfalls (verified this repo)

Reproduced failures when publishing `to-tickets` output to GitHub Issues via
`gh`. Use these exact shapes; do not improvise the quoting.

## 1. Create one issue, capture its number
`--json` is NOT supported on `gh issue create` in this `gh` build.

```bash
n=$(gh issue create \
  --title "Triage #01: ..." \
  --label "ready-for-agent" \
  --body-file /tmp/ticket01.md \
  | tail -1 | grep -oE '[0-9]+$')
echo "created #$n"
```

## 2. Body via file, never inline heredoc
Inline `<<'EOF'` inside a `&&`-chained `gh` call produces
`unexpected EOF while looking for matching ''`. Write first:

```bash
cat > /tmp/ticket01.md <<'EOF'
## What to build
...
EOF
gh issue create --title "..." --label "ready-for-agent" --body-file /tmp/ticket01.md
```

## 3. Blocking edges — comma-separated
```bash
# CORRECT (comma):
gh issue create --title "..." --label "ready-for-agent" \
  --body-file /tmp/ticket04.md --blocked-by 229,230

# WRONG (space) -> "invalid issue format: \"229 230\"":
# gh issue create ... --blocked-by 229 230
```

## 4. Verify edges landed (shape is blockedBy.nodes[])
```bash
gh issue view 232 --json blockedBy --jq '[.blockedBy.nodes[].number]|join(", ")'
# -> "229, 230"
```

## 5. Label pre-flight (avoid silent AFK starvation)
```bash
gh api repos/<owner>/<repo>/labels/ready-for-agent  # 200 = ok; 404 = create
gh label create "ready-for-agent" --color "0e8a16" \
  --description "Agent-grabbable issue — Principal loop dispatches an AFK student." --force
```

## 6. Dependency-ordered publish loop (blockers first)
```bash
n1=$(gh issue create --title "#01 ..." --label ready-for-agent --body-file /tmp/t01.md | tail -1 | grep -oE '[0-9]+$')
n2=$(gh issue create --title "#02 ..." --label ready-for-agent --body-file /tmp/t02.md --blocked-by "$n1" | tail -1 | grep -oE '[0-9]+$')
n3=$(gh issue create --title "#03 ..." --label ready-for-agent --body-file /tmp/t03.md --blocked-by "$n1" | tail -1 | grep -oE '[0-9]+$')
n4=$(gh issue create --title "#04 ..." --label ready-for-agent --body-file /tmp/t04.md --blocked-by "$n2,$n3" | tail -1 | grep -oE '[0-9]+$')
n5=$(gh issue create --title "#05 ..." --label ready-for-agent --body-file /tmp/t05.md | tail -1 | grep -oE '[0-9]+$')
```
