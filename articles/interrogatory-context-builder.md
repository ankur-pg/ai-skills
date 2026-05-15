# Building an Interrogatory Context Builder Skill

AI coding agents are very good at turning instructions into code.
The hard part is often not the coding.
The hard part is making sure the agent has the right context before it starts.

That is the problem I wanted to explore.
Instead of writing a long prompt by hand every time, I wanted a reusable skill that could interview me, extract the missing context, and produce a durable artifact that another AI session or human could use later.

The result is a small public skill called `interrogatory-context-builder`.

Repository:
https://github.com/ankur-pg/ai-skills

Demo app:
https://github.com/ankur-pg/test-skill

## The Idea

Most AI-assisted development starts with a prompt like:

```text
Build a simple Node.js app.
```

That looks clear, but it hides a lot of decisions:

- What should the app actually do?
- Who owns the source of truth?
- What is explicitly out of scope?
- Should the server or browser handle the logic?
- Should the result be a one-time value or keep updating?
- What should be recorded so another session can continue later?

The skill changes the workflow.
Instead of the human pushing all context up front, the AI pulls context through a short interview.

The key rule is simple:

```text
Ask one focused question at a time before designing or implementing.
```

## What the Skill Does

The skill has three modes.

**Build Mode** is for starting from weak or missing context.
The agent interviews the human until the task is safe to plan or implement.

**Review Mode** is for checking an existing ticket, spec, ADR, or design note.
The agent reads the document, identifies gaps and risky assumptions, then interviews an expert to verify them.

**Handoff Mode** is for preserving a session.
The agent converts the conversation into a concise artifact such as `CONTEXT.md`, a feature brief, an implementation brief, or a session handoff.

The skill intentionally avoids premature implementation.
It asks questions first, labels facts and assumptions separately, captures rationale, records rejected alternatives, and writes context that survives beyond the chat.

## The Skill Structure

The public repo uses a simple Agent Skills style layout:

```text
skills/
  interrogatory-context-builder/
    SKILL.md
    examples/
      interview-examples.md
    references/
      artifacts.md
```

The main skill file defines:

- when to use the skill
- the operating modes
- interview rules
- what context to elicit
- review-mode behavior
- artifact guidance
- anti-patterns
- output style

The reference file contains reusable templates for:

- context briefs
- spec review reports
- session handoffs

The examples file shows the behavior in practice.

## Testing the Skill on a Real Repo

To test the skill, I created a second empty repository:

```text
ankur-pg/test-skill
```

The goal was intentionally small:

```text
A simple Node.js app, which displays today's date and time in the user's locale when the page loads.
```

Instead of immediately scaffolding the app, the skill forced an interview.

The first question was:

```text
Should the date and time be computed in the browser using the user's local clock and locale, or should the Node.js server generate it before sending the page?
```

Answer:

```text
Use the user's local clock.
```

The next question was:

```text
Should this be a minimal plain HTML page served by Node.js, or do you want a framework such as Express, Vite, or Next.js?
```

Answer:

```text
Minimal plain HTML page.
```

The next question was:

```text
Should the page display the date/time once at load time only, or should it keep updating every second like a clock?
```

Answer:

```text
Once at load time only.
```

At that point, the skill had enough context to produce a durable brief.

## The Context Artifact

Before writing code, the skill created `CONTEXT.md` in the demo repo.

That file captured:

- objective
- user experience
- decisions
- rationale
- rejected alternatives
- constraints
- open questions
- next actions

The important decisions were recorded in a table:

| Decision | Rationale | Rejected Alternatives |
| --- | --- | --- |
| Use the browser's local clock and locale | The displayed date/time should reflect the user's own locale and device time | Server-generated date/time |
| Use a minimal plain HTML page | The app is simple and does not need a frontend framework | Express-rendered template, Vite, Next.js |
| Display once on load | The requirement is to show the time when the page loads, not a live clock | Updating every second |

This was the useful part.
The context did not just say what to build.
It preserved why those decisions were made.

## Building From the Context

After reviewing the context brief, the implementation was straightforward.

The demo app uses no external dependencies.
Node.js serves static files, and the browser computes the localized date/time on page load.

The app contains:

```text
CONTEXT.md
README.md
package.json
server.js
server.test.js
public/
  index.html
  app.js
  styles.css
```

The browser logic is intentionally small:

```js
const output = document.querySelector("#local-date-time");
const now = new Date();

if (output) {
  output.dateTime = now.toISOString();
  output.textContent = new Intl.DateTimeFormat(undefined, {
    dateStyle: "full",
    timeStyle: "long",
  }).format(now);
}
```

Using `undefined` as the locale tells the browser to use the user's default locale.
Using `new Date()` means the value comes from the user's local clock.
Because there is no interval or timer, the value is rendered once when the page loads.

## Validation

The skill itself was tested by using it in a real workflow rather than only reading the prompt.

For the public skill repo:

- the skill frontmatter was parsed successfully
- the files were checked for whitespace issues
- the files were checked for non-ASCII characters
- the skill was reviewed through a pull request before merging

For the demo Node.js app:

- `CONTEXT.md` was created before implementation
- the implementation followed the decisions in `CONTEXT.md`
- `npm test` passed
- the local server was started successfully
- the root page returned `200 OK`
- the client-side JavaScript was smoke checked
- the code was pushed to GitHub

The test repo is intentionally small, but that is the point.
The workflow can be evaluated without the noise of a large system.

## What I Learned

The skill is not just a better prompt.
It is a reusable workflow.

The most useful behavior was not that it generated code.
It was that it slowed the agent down before code generation and made the hidden decisions explicit.

The `CONTEXT.md` artifact also changed the shape of the work.
Once the brief existed, implementation became a mechanical step instead of a guessing exercise.

This pattern should become more valuable as tasks get larger.
For a small Node.js app, it may feel slightly formal.
For a production feature with product constraints, ownership boundaries, data rules, security concerns, and rollout risk, this kind of context artifact can prevent a lot of rework.

## How to Try It

Use the skill by pointing an AI agent at the skill file:

```text
Read skills/interrogatory-context-builder/SKILL.md.

Use Build Mode.
I want to build <your feature>.
Interview me one question at a time before designing or implementing anything.

Once you have enough context, create a CONTEXT.md file.
After I approve that context, use it to implement the feature.
```

The expected first response should be a single focused question, not a design and not code.

That is the core idea:

```text
Context first.
Implementation second.
```
