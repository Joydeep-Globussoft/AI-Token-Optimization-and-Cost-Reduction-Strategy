# Claude Customization Guide for Coding Workflows

A practical setup guide for getting the best results from Claude across Frontend, Backend, Debugging, Code Review, Testing, and Documentation work.

This covers two layers, and you'll get the best results by using both together:

1. **Claude.ai settings** — Profile Preferences ("Instructions for Claude"), Styles, Projects, and Memory
2. **Per-task prompts** — copy-paste templates tuned for each use case

---

## 1. Claude.ai Settings — What to Configure

| Setting | Where to find it | What it does |
|---|---|---|
| **Instructions for Claude** (Profile Preferences) | Settings (menu labels shift between releases — look under Profile/General) | Always-on context loaded into every new chat |
| **Styles** | `+` menu in a chat → Use style | Per-chat tone/format preset (Normal, Concise, Explanatory, Formal, or custom) |
| **Projects** | Left sidebar → Projects (paid plans only) | Scoped instructions + knowledge files for one codebase/client |
| **Search & reference past chats** | Settings → Preferences (if enabled) | On-demand retrieval — Claude pulls up prior conversations when you ask it to |
| **Generate memory from chat history** | Settings → Preferences (if enabled) | Passive, automatic filing of durable facts as you chat — don't duplicate this in your instructions, let it handle recurring details |
| **Artifacts / Code Execution** | Settings (feature toggles — exact section name varies by release) | Turn on for runnable code, file creation, and rendered previews |

### Recommended: Instructions for Claude (global, account-wide)

Keep this short and durable — it should hold true across almost every coding conversation you have. Put task-specific rules (see below) in **Project instructions** instead, since those change per codebase.

```
I'm a [your role, e.g., "full-stack developer"] working primarily in [languages/frameworks, e.g., "TypeScript, React, Node.js, PostgreSQL"].

Default behavior for code:
- Give me working code first, explanation after — not the reverse.
- Assume I know the language; skip basic syntax explanations unless I ask.
- Flag security issues, race conditions, or performance problems even if I didn't ask about them.
- If my code or approach has a real problem, say so directly — don't just validate it.
- When there are multiple reasonable approaches, briefly name the tradeoff instead of picking silently.
- Prefer standard library / well-maintained packages over reinventing things.
- No filler ("Great question!", excessive caveats, restating my question back to me).

```

**What to leave out of this field:**
- One-off preferences that only apply to a single task — those belong in the chat itself, not the global field.
- Project-specific jargon, repo names, or conventions — those belong in Project instructions.
- Anything that would stop Claude from giving you honest pushback (e.g. "always agree with my approach," "never criticize my code") — this degrades the quality of feedback you get and isn't worth the short-term comfort.

### Recommended: Style

- **Concise** or **Normal** for day-to-day coding — Explanatory/Formal styles add prose that slows down iterative work.
- Switch to **Explanatory** only when you're deliberately learning a new concept, not for routine tasks.

### Recommended: Projects

Create one Project per codebase/repo. Put in the Project's knowledge/instructions:
- Your actual style guide or linting rules (paste the real config, e.g. `.eslintrc`, `pyproject.toml`)
- Folder structure / architecture notes
- Naming conventions specific to that repo
- What NOT to change (e.g., "never touch `/legacy`, it's frozen")

This matters more than any global instruction — Claude performs much better with the real repo context than with generic rules.

---

## 2. Per-Use-Case Prompts

Use these as starting templates. Fill the brackets, keep the structure — the structure is what actually improves output quality, more than the wording.

**When to skip the template:** these are built for non-trivial work. For a two-line fix, a single utility function, or a quick clarifying question, just ask directly — filling out a full template for something that small adds friction without adding quality.

### 🎨 Frontend

```
Build/modify a [component/page] in [React/Vue/Svelte/etc.] using [styling approach, e.g. Tailwind].

Requirements:
- [functional requirement 1]
- [functional requirement 2]
- Responsive: [breakpoints or "mobile-first"]
- Accessibility: semantic HTML, keyboard navigation, ARIA where needed
- State management: [local state / Redux / Zustand / context — specify]

States to handle :
- Loading: [skeleton / spinner / specify, or "your call"]
- Empty: [what shows when there's no data]
- Error: [what shows on a failed fetch/action]

Match the existing pattern in [file/component name] if relevant.
Do not introduce new dependencies unless necessary — if you do, tell me why.
Show the full component, not a diff, unless I ask for a diff.
```

**Settings tip:** turn on Artifacts so components render live and you can iterate visually instead of reading code blind.

### ⚙️ Backend
```
Implement [endpoint/service/function] in [language/framework].

Context :
- Existing data model: [paste schema or describe]
- Auth model: [e.g., JWT middleware already exists at X — who's allowed to call this?]
- Error handling convention: [e.g., custom AppError class, or describe]

Requirements:
- [input/output contract]
- [validation rules]
- [performance/scale constraints if any]


Behavior on failure:
- Invalid input: [return 400 with field errors / throw AppError / specify]
- Downstream failure (DB down, external API times out): [retry / fail fast / specify]
- Concurrent calls / idempotency: [does calling this twice with the same input need to be safe? specify or say "not a concern here"]

Call out: any assumption you're making about data shape, and any edge case you're not handling.
```
### 🐛 Debugging

```
This code/error is not behaving as expected.

Expected behavior: [what should happen]
Actual behavior: [what happens instead]
Error message / stack trace: [paste exact text]
Relevant code: [paste the smallest reproducible snippet]
What I've already tried: [list, so Claude doesn't repeat it]

Don't just patch the symptom — tell me the root cause, then the fix.
If you need more context (logs, other files) to be sure, ask before guessing.
```

**Settings tip:** enable Code Execution — Claude can actually run snippets and reproduce the bug instead of reasoning about it blind, which is far more reliable for debugging.

<details>
<summary>Worked example</summary>

```
This code/error is not behaving as expected.

Expected behavior: POST /users should return 201 with the new user's id.
Actual behavior: Returns 500, no response body.
Error message / stack trace: TypeError: Cannot read properties of undefined (reading 'insertOne') at UserRepo.create (userRepo.js:14)
Relevant code:
  async create(data) {
    return this.db.collection('users').insertOne(data);
  }
What I've already tried: confirmed MongoDB is running locally; the same repo class works fine in the test suite.

Don't just patch the symptom — tell me the root cause, then the fix.
If you need more context (logs, other files) to be sure, ask before guessing.
```

This is what "smallest reproducible snippet" and "what I've already tried" look like filled in — specific enough that Claude isn't guessing at your setup.
</details>

### 🔍 Code Review

```
Review [PR/diff/file] for [correctness / security / performance / style — pick focus, or say "all"].

What this change is trying to do and why: [1-2 lines — e.g. "fixes race condition in checkout" or "adds pagination to /orders"]

- Point out real problems only — skip nitpicks unless I ask for a full nitpick pass.
- For each issue: severity (blocker/major/minor), why it's a problem, and a concrete fix.
- Flag any changed logic that isn't covered by a test, even if nothing else is wrong with it.
- Note anything that's a good pattern too, briefly — not just criticism.
- If something looks intentional but unusual, ask rather than assume it's wrong — use the stated intent above to judge, not just the diff in isolation.

[paste diff/code]

```


### ✅ Testing

```
Write tests for [file/function/module] using [test framework, e.g., Jest, pytest, Vitest].

Cover:
- Happy path
- Edge cases: [list known tricky inputs, or say "identify them yourself"]
- Error/failure states
- [mocking requirements — what should be mocked vs. real]

Match existing test file conventions if I've shown you one.
Tell me which cases you think are still worth adding manually (e.g., ones needing real infra) even if you didn't write them.
```

### 📚 Documentation

```
Write [README section / API docs / inline docstrings / architecture doc] for [module/feature].

Audience: [other engineers on the team / external API consumers / future-you]
Format: [Markdown / JSDoc / docstring style — specify]

Include:
- What it does and why (not just parameters)
- Usage example that actually runs
- Known limitations or gotchas

Keep it as short as it can be while staying complete — no padding.
```

---

## 3. Quick Reference: Settings by Task

| Task | Style | Feature to enable | Project vs. one-off chat |
|---|---|---|---|
| Frontend | Concise | Artifacts (live preview) | Project (needs design system context) |
| Backend | Concise | Code Execution (optional, for testing logic) | Project |
| Debugging | Concise | Code Execution | Either — one-off is fine for isolated bugs |
| Code Review | Normal | — | Project (needs style guide) |
| Testing | Concise | Code Execution | Project |
| Documentation | Explanatory (for audience-facing docs) | Artifacts (renders Markdown) | Either |

---

