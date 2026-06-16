---
name: qa-checklist
description: Generate a senior QA checklist after implementation commits. Run after every coding session that ends with committed code.
---

# QA Checklist Generator

When this skill is invoked:
1. Generate and split the checklist into two parts: **Automated** (subagent runs) and **Manual** (user runs)
2. Dispatch a subagent to execute all automated checks immediately
3. Present the manual checklist to the user

## Step 1 — Gather session context

1. Run `git log --oneline` to see what was committed this session
2. Run `git diff <baseline>..HEAD --stat` to see what files changed
3. Get a real `brandId` from the database: `node -e "const {PrismaClient}=require('@prisma/client');const p=new PrismaClient();p.brand.findFirst().then(b=>console.log(b?.id)).finally(()=>p.\$disconnect())"`
4. For each meaningful change, derive testable checks

## Step 2 — Classify every check

**Automated (subagent runs)** — anything verifiable without human eyes or interaction:
- API routes: GET/POST/PATCH/DELETE calls with PowerShell, checking response shape and values
- Auth flows: login, logout, cookie lifecycle, 401 on unauthenticated requests
- Validation rejections: wrong type, missing field, empty string
- Unit/integration test suite: `npm test`
- TypeScript check: `npx tsc --noEmit`
- File content assertions: confirm console.logs removed, no banned patterns
- `estimateCost`, `weekKeyForDate`, and other pure exported functions via `npx tsx -e`
- DB persistence: write then read back to confirm record was created

**Manual (user runs)** — anything that requires eyes, clicks, or live external services:
- UI renders correctly (layout, icons, colours, loading states)
- Error states shown inline (e.g. "Invalid password" text appearing on the page)
- Redirect behaviour after form submit
- Dashboard tab content looks correct (charts, tables populated)
- AI-generated content quality (blog post, ad copy, chat response)
- Budget breach → alert banner appears site-wide
- AlertBanner dismiss interaction
- Journey step execution with real delays
- WhatsApp message delivery
- Anything requiring a live external service (Shopify API, Meta API, WhatsApp Cloud)

## Step 3 — Dispatch the automated subagent

Once both lists are written, dispatch a **single subagent** to run all automated checks.

The subagent prompt must include:
- All automated checks numbered and described
- The base URL (`http://localhost:3000`)
- The `ADMIN_PASSWORD` from `.env` (read it — don't hardcode a guess)
- The `brandId` fetched in Step 1
- The working directory
- Required report format: each check as ✅ PASS or ❌ FAIL with actual observed value
- A final summary: X/N passed, list any failures

The subagent runs all checks sequentially (some depend on a shared `$session` cookie). It must handle the login flow first and reuse the session for authenticated requests.

Report the subagent's results back to the user inline.

## Step 4 — Present the manual checklist

After the subagent finishes, present the manual checklist clearly labelled:

```
## Manual QA — Your Turn

### <Section>
- [ ] <what to do> → expected: <what you should see>
  URL or instruction on the line below

### Known skips (needs external service / not yet testable)
- <item>
```

---

## Rules for generating checks

**Coverage — for every change, cover:**
- Happy path
- Validation / rejection (wrong type, missing field, out-of-range, empty string)
- Edge cases relevant to the feature
- Error states

**Automated check format:**
```
## CHECK N — <short description>
<powershell command or npm command>
Expected: <exact value or shape>
```

**Manual check format:**
```
- [ ] <what to do> → expected: <what you should see>
  http://localhost:3000/...  (or plain instruction)
```

**For authenticated API checks** — always establish session first:
```powershell
$session = New-Object Microsoft.PowerShell.Commands.WebRequestSession
Invoke-RestMethod "http://localhost:3000/api/auth/login" -Method POST -ContentType "application/json" -Body '{"password":"ADMIN_PASSWORD"}' -SessionVariable session
```

**For validation failures** (`Invoke-RestMethod` throws on 4xx):
```powershell
try { Invoke-RestMethod ... } catch { $_.ErrorDetails.Message }
```

**For write operations** — always follow with a read to confirm persistence.

---

## Senior QA standards

- Never check just "returns 200" — check the actual shape and field values
- Every write (POST/PATCH) must be followed by a read confirming the record exists
- Validation must cover: wrong type, missing field, out-of-range, empty string
- For UI: check not just that a tab loads, but that the right content is in it
- If a feature auto-creates on first access — verify first call creates, second call reads existing
- Flag anything that can't be tested yet (stub, needs live service, needs real delay)

## Token efficiency

Keep it tight. One line per check. Command on the next line, indented. No prose unless a setup step is non-obvious.
