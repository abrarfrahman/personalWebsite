# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project context

This repo currently holds a small static personal site (`index.html`, `About.html`,
`Papers.html`, `Side.html`, plus assets in `Abrar Rahman_files/`). No build step, no
package manager, no dependencies — the HTML files are edited directly and served as-is.

The larger goal this repo is growing into: **in-house AI tooling for a healthcare-AI
content operation.** The four workstreams are content **sourcing**, **distribution**,
**engagement**, and **revenue**. New code should be understood as serving one of those
four, and it's worth naming which one in the PR description.

Treat the existing static site as legacy surface area. Don't refactor it, restructure it,
or migrate it to a framework unless explicitly asked.

## Workflow rules

### Branch naming

**All branches must be prefixed `abrar/`.** No exceptions.

```
git checkout -b abrar/feed-ingest
git checkout -b abrar/fix-og-tags
```

PR titles are free-form — the prefix applies to branch names only.

### Committing and pushing

- Never commit or push unless asked. Never commit directly to `main` — branch first.
- Don't add co-author trailers or tool attribution to commits unless asked.
- Match the existing commit style: short, lowercase, plain-spoken (`sold -> acquired`,
  `promise update`, `cleaner front page`). No Conventional Commits prefixes.

### Pull requests

Open PRs with the `gh` CLI rather than handing over a link:

```
gh pr create --title "..." --body "..."
```

Asking to push implies opening the PR — don't stop at the pushed branch and wait for a
follow-up. Keep the body to the standard above: architectural decisions only, one line is
fine. Don't add tool attribution footers.

### Scope

- Do what was asked. Don't add unrequested features, abstractions, or "while I was in
  there" cleanups.
- Prefer editing existing files over creating new ones.
- Don't create README or docs files unless asked.

## Documentation

Keep it short. READMEs, PR descriptions, and design notes should cover **critical
architectural decisions only** — what was chosen, and why that choice over the obvious
alternative. That's the part that isn't recoverable from reading the code later.

Technical detail belongs in code comments, next to the code it describes, and only where
the code doesn't already say it. A comment explaining a non-obvious constraint, a rate
limit, or why an ugly workaround exists earns its place. A comment restating what the
line does is noise.

Don't write:

- Prose walkthroughs of how a function works
- Setup instructions that duplicate the actual commands
- Changelogs, summaries of work performed, or lists of files touched in a PR description
- Docs for code that hasn't stabilized yet

A one-line PR description is a fine PR description.

## Code conventions

Nothing is established yet — most of this repo's future code doesn't exist. When adding
the first code in a new language or area, pick a mainstream default, keep it boring, and
note the choice in the PR so it can become a convention deliberately rather than by
accident.

For the existing HTML: plain HTML + CSS, no frameworks, no build. Styles live in
`Abrar Rahman_files/style.css` (with `skeleton.css` and `normalize.css` as the base).
Keep edits in that idiom.

## Things to be careful about

### Secrets and credentials

The content tooling will touch API keys — social platform tokens, LLM providers,
analytics, payment processors. Never hardcode a credential, never commit one, never echo
one into terminal output or a log line. Use environment variables and keep the real
values out of the repo. If you need a new secret, add the *name* to a `.env.example` and
tell me what to fill in.

### Publishing and outbound actions

Anything that posts publicly, sends to subscribers, DMs, or charges money is
irreversible. Confirm with me before running it, even in a script I asked you to write —
approval to write the code is not approval to fire it. Default new tooling to a dry-run
or `--publish` opt-in flag rather than posting by default.

### Scraping and platform terms

Content sourcing means pulling from other people's platforms. Prefer official APIs and
documented feeds over scraping. Respect rate limits and `robots.txt`. If a task can only
be done in a way that plainly violates a platform's terms, say so before building it
rather than after.

### Healthcare content

Posts trend on healthcare AI, which means clinical claims will pass through this tooling.
Don't let generated or summarized content assert medical facts, efficacy, or outcomes
that aren't in the source. When summarizing a paper or study, preserve hedging and
sample-size caveats — don't sharpen a tentative finding into a confident one. Attribute
claims to their source. Never generate anything shaped like medical advice to a reader.

Never pipe PHI or patient-identifiable data through this tooling. If a source contains
it, stop and flag it.

### Large files

`sage.png` is 10MB and already committed. Don't add more binaries of that size — compress
images before committing, and keep large raw assets out of git.

## Verifying work

There is no test suite or linter yet. Until there is, verify by actually running the
thing: open the HTML in a browser for site changes, execute the script for tooling
changes. Don't report something as working on the strength of reading the code.

When you add the first real tooling, add tests alongside it and document the run command
here.
