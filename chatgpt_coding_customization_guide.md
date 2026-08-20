# ChatGPT Customization Guide for Coding Workflows

A simple guide for getting better results from ChatGPT for Frontend, Backend, Debugging, Code Review, Testing, and Documentation.

## 1. Two Layers to Use

A good coding workflow can use two layers together:

1. **ChatGPT settings** — Custom Instructions, Memory, Projects, and Custom GPTs
2. **Per-task prompts** — short prompts made for the specific task

Use global settings for things that are true most of the time. Use Projects and task prompts for codebase-specific details.

---

## 2. ChatGPT Settings

| Setting | Where to find it | Main use |
|---|---|---|
| **Custom Instructions** | Settings → Personalization → Custom Instructions | General preferences for your chats |
| **Memory** | Settings → Personalization → Memory | Remember useful information from previous chats |
| **Projects** | Sidebar → Projects | Keep chats, files, and instructions together for one project |
| **Custom GPTs** | Explore GPTs → Create | Create a separate assistant with its own instructions and knowledge |
| **Canvas / Code Interpreter** | Available in supported chats | Work with editable content or run/analyze code |

### Custom Instructions

Custom Instructions are applied to your chats and are useful for setting your normal coding preferences.

The current character limit depends on your plan:

- **Free and Go:** up to 1,500 characters
- **Plus, Pro, Enterprise, Business, and Education:** up to 5,000 characters


### Example: What ChatGPT should know about you
The field is short, so keep only what's true across nearly every coding chat. Put codebase-specific rules in a Project instead.

**"What would you like ChatGPT to know about you?"**
```
I'm a [your role, e.g., "backend engineer"] working mainly in [languages/frameworks].
Experience level: [e.g., "senior — skip fundamentals unless I ask"].
```

### Example: How ChatGPT should respond

```text
For code:
- Give working code first and explain it after.
- Skip basic syntax explanations unless I ask.
- Point out security, performance, and concurrency issues when relevant.
- Push back if my approach has a real problem.
- If there are multiple good approaches, briefly explain the tradeoff.
- Avoid filler and unnecessary repetition.
- Prefer standard libraries or well-maintained packages.
- Ask every time for approval before starting each step.
- not make assumptions about requirements, technology, UI, architecture, or implementation details.
```

Keep these instructions short. Do not put project-specific rules here if they are only needed for one project.

---

## 3. Memory

Memory can help ChatGPT remember useful information from previous conversations, so you do not have to repeat the same details.

For example, it may remember:

- Your usual programming languages
- Your general preferences
- Useful information from earlier conversations

Memory is different from Custom Instructions:

- **Custom Instructions:** You directly tell ChatGPT how you want it to respond.
- **Memory:** ChatGPT can remember useful information from your conversations.

Do not depend on Memory for important project rules. Put important and current rules in your Project instructions or in the current prompt.

### Good practice

Review your Memory from time to time and remove information that is no longer useful or correct.

---

## 4. Projects

Projects are useful for long-running coding work.

A Project can keep:

- Chats
- Reference files
- Project instructions
- Project-related context

### Put these in Project instructions/files

- Coding style and lint rules
- Folder structure
- Architecture notes
- Naming conventions
- Important project requirements
- Files or folders that should not be changed

Project instructions apply only inside that Project and override global Custom Instructions there.

### Project memory

Projects can use project memory. Depending on the memory mode and plan, ChatGPT may use information from other chats in the same project.

When creating a new Project, you can choose **project-only memory** when that option is available. With project-only memory:

- Previous saved memories are not used.
- Chats can refer to other chats in the same Project.
- Chats cannot refer to chats outside that Project.

This is useful when you want a separate, focused workspace for a codebase.

---

## 5. Custom GPTs

Custom GPTs are useful when you repeatedly need a specific type of assistant, such as:

- A code reviewer
- A documentation assistant
- A testing assistant
- A team coding assistant

A Custom GPT can have its own instructions, knowledge files, and supported capabilities.

Remember:

- A Custom GPT is a separate assistant configuration.
- Its behavior depends on its own instructions and configuration.
- Do not assume your normal ChatGPT setup will work exactly the same way inside a Custom GPT.
- Current ChatGPT Memory is not available in Custom GPTs, according to OpenAI's current documentation.

---

# 6. Per-Task Prompts

Good task prompts do not need to be very long. Give ChatGPT the information it actually needs.

### 🎨Frontend Development Prompt
----
```text
Build a **[component/page]** in **[React/Vue/Svelte/etc.]** using **[Tailwind/CSS/etc.]**.

**Requirements:**

* Main functionality: [describe what it should do]
* UI elements: [buttons, forms, cards, tables, etc.]
* Responsive: mobile-first and work well on desktop
* Accessibility: semantic HTML, keyboard support, and ARIA where needed
* State management: [local state / Redux / Zustand / Context]
* Handle loading, error, and empty states where needed
* Follow the existing project structure and coding style
* Match the design/pattern of **[existing component/file]** if relevant
* Reuse existing components and utilities where possible
* Do not add new dependencies unless necessary; explain why if one is added
* Keep the implementation simple and avoid unnecessary code
* Show the **full component**, not a diff, unless I ask for a diff
* Briefly explain the main changes after the code
```
**Tip:** Use Canvas when you want an editable code workspace and the feature is available in your chat.

## ⚙️Backend Development Prompt

```text
Implement [endpoint/service/function] using [language/framework].

Context:

- Data/model: [schema or short description]
- Authentication: [auth system]
- Error handling: [existing convention]

Requirements:

- Input and expected output
- Validation rules
- Performance requirements, if any
- Handle common edge cases
- Follow the existing project structure and coding style
- Reuse existing utilities where possible
- Do not add new dependencies unless necessary
- Mention important assumptions
- Return the complete implementation and a short explanation
```

## 🐛 Debugging

```text
This code/error is not behaving as expected.

Expected behavior: [what should happen]
Actual behavior: [what happens]
Error message/stack trace: [exact error]
Relevant code: [smallest useful snippet]
What I already tried: [list]

Find the likely root cause before suggesting the fix.
If important context is missing, ask for it instead of guessing.
```

When code execution is available, Code Interpreter can help analyze or run suitable code. However, it is not the same as your real development environment, so environment-specific bugs may still require testing on your machine.

## 🔍 Code Review

```text
Review this [PR/diff/file] for [correctness/security/performance/style].

- Point out real problems first.
- For each issue, give severity, why it matters, and a concrete fix.
- Skip minor style nitpicks unless I ask for a full review.
- Mention good patterns briefly.
- If something looks unusual but may be intentional, ask rather than assuming it is wrong.

[Paste diff/code]
```

## ✅ Testing

```text
Write tests for [file/function/module] using [test framework].

Cover:
- Happy path
- Edge cases
- Error/failure cases
- [Mocking requirements]

Match the existing test style if one is provided.
Also tell me which cases should still be tested manually.
```

## 📚 Documentation

```text
Write [README section/API docs/docstrings/architecture document] for [module/feature].

Audience: [developers/API users/etc.]
Format: [Markdown/JSDoc/docstring/etc.]

Include:
- What it does and why
- A working usage example
- Important limitations or gotchas

Keep it concise and avoid unnecessary text.
```

---

# 7. Quick Reference

| Task | Useful surface | Helpful feature | Best context |
|---|---|---|---|
| Frontend | Canvas or normal chat | Canvas when available | Project |
| Backend | Normal chat or Project | Code execution when useful | Project |
| Debugging | Normal chat or Project | Code Interpreter when useful | Current task + relevant code |
| Code Review | Project or normal chat | — | Project |
| Testing | Normal chat or Project | Code Interpreter when useful | Project |
| Documentation | Canvas or normal chat | Canvas when available | Project or current task |

---

# 8. Simple Rules for Better Coding Results

1. Give only the code and context that are relevant to the task.
2. Do not upload an entire repository when a few files are enough.
3. Tell ChatGPT what you already tried when debugging.
4. Put stable project rules in Project instructions.
5. Use Custom Instructions for general preferences.
6. Use Memory as helpful context, not as the source of critical project rules.
7. Start a new or summarized conversation when an old thread becomes unnecessarily large.
8. Ask for the output format you want.
9. Reuse good prompts and project instructions instead of rewriting them every time.

---

# 9. Important Notes

- Custom Instructions apply to chats, but their character limit depends on the plan.
- Memory and Custom Instructions serve different purposes. Do not rely on an undocumented priority rule between them.
- Project instructions apply inside the Project and override global Custom Instructions there.
- Project memory behavior depends on the selected memory mode and account/workspace setup.
- Custom GPTs have their own configuration. Do not assume your normal ChatGPT settings behave identically inside them.
- Feature availability and limits can change, so check the current OpenAI Help Center if something in your ChatGPT interface looks different.

