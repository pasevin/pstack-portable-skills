# pstack portable skills

Harness-agnostic ports of the code-quality and writing-quality skills from
**pstack**, a Cursor plugin by Lauren Tan ("poteto").

- Original plugin: https://github.com/cursor/plugins/tree/main/pstack
- Author: Lauren Tan
- Source version ported: pstack 0.14.2
- License: MIT (original `LICENSE` reproduced in this repo)

This is not the full pstack plugin. Only the skills that are pure engineering
principles, code-quality workflows, or writing-quality workflows are ported
here, in the standard `SKILL.md` format so they can be dropped into any
agent's skills directory, regardless of which coding agent or IDE you use.

## What was excluded, and why

pstack ships ~38 skills as one interlocking system plus two Cursor-plugin
agent definitions (`poteto-agent`, `Comment Sicko`) and a Cursor Rules file.
The following were left out because they depend on Cursor-specific
infrastructure that has no equivalent outside Cursor, or because they are
meta/orchestration skills rather than code- or writing-quality skills:

- `poteto-mode`, `setup-pstack`: depend on Cursor's Rules feature
  (`~/.cursor/rules/*.mdc`) and Cursor's plugin/mode UI chrome.
- `recall`, `automate-me`, `reflect`: depend on Cursor's chat transcript
  storage path (`~/.cursor/projects/*/agent-transcripts/`) and Cursor's
  built-in `create-skill` skill.
- `how`, `why`, `architect`, `arena`, `interrogate`, `swarm`, `figure-it-out`,
  `create-verification-skill`, `maintain-verification-skill`: a multi-model
  investigation/design panel system, out of scope for this port.

## What was ported unchanged

These are copied verbatim from pstack 0.14.2, including their reference
files. No Cursor-specific content was found in any of them.

Principles (engineering judgment, one rule per skill):

- `principle-boundary-discipline`
- `principle-build-the-lever`
- `principle-encode-lessons-in-structure`
- `principle-exhaust-the-design-space`
- `principle-experience-first`
- `principle-fix-root-causes`
- `principle-foundational-thinking`
- `principle-guard-the-context-window`
- `principle-laziness-protocol`
- `principle-make-operations-idempotent`
- `principle-migrate-callers-then-delete-legacy-apis`
- `principle-minimize-reader-load`
- `principle-model-the-domain`
- `principle-never-block-on-the-human`
- `principle-outcome-oriented-execution`
- `principle-prove-it-works`
- `principle-redesign-from-first-principles`
- `principle-separate-before-serializing-shared-state`
- `principle-sequence-verifiable-units`
- `principle-subtract-before-you-add`
- `principle-type-system-discipline`

Code and writing quality:

- `typescript-best-practices`
- `tdd`
- `unslop`
- `blast-radius`
- `teach`

## What was ported with a precise edit

Two skills depended on Cursor-only plumbing for one specific step. The
surrounding content is untouched; only the named mechanism was replaced.

- `no-comments`: originally dispatched a Cursor-plugin-registered subagent
  persona (`subagent_type: "Comment Sicko"`, defined in pstack's
  `agents/comment-sicko.md`, resolved via Cursor's plugin agent registry).
  That persona's full original text is preserved verbatim in
  `skills/no-comments/references/comment-sicko.md`, and the skill now
  instructs spawning a fresh, isolated subagent with that file as its
  instructions instead of naming a Cursor-registered agent type.
- `show-me-your-work`: originally audited its decision log against a
  transcript file at a Cursor-specific path
  (`~/.cursor/projects/*/agent-transcripts/`). That line now reads the
  session's own transcript generically, if the environment exposes one,
  with the same "never read unrelated sessions" constraint.

## Using these skills

### Install with the skills CLI

This repo's layout (`skills/<name>/SKILL.md`) matches what the open-source
[`skills` CLI](https://github.com/vercel-labs/skills) expects, so any skill
here installs with one command, no cloning required:

```bash
# List every skill in this repo
npx skills add pasevin/pstack-portable-skills --list

# Install one skill into the current project
npx skills add pasevin/pstack-portable-skills --skill unslop

# Install a skill for every project on your machine
npx skills add pasevin/pstack-portable-skills --skill unslop -g

# Install everything in this repo
npx skills add pasevin/pstack-portable-skills --all
```

The CLI supports 75+ agents (Claude Code, Cursor, Codex, and more) via the
`--agent` flag; see its README for the full list.

### Install by hand

Copy any `skills/<name>` directory into wherever your agent or tool reads
skills from (for example a `skills/` directory read by your coding agent,
whether that's a user-level directory or a project-level one). Each skill is
self-contained in its own directory with a `SKILL.md` and, where noted above,
a `references/` or `scripts/` subdirectory.

Some skills reference each other by name in prose (for example
`principle-type-system-discipline` mentions `typescript-best-practices`).
Where the referenced skill is included in this repo, install both for the
cross-reference to make sense. Where it isn't (for example `blast-radius`
mentioning `how`/`why`/`arena`), the skill still functions on its own; the
absent skill is just not available to delegate that one step to.
