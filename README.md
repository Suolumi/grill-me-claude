# grill-me-claude

`grill-me` and `grilling` agent skills, fine-tuned for [Claude Code](https://claude.com/claude-code).

## Origin

Both skills come from Matt Pocock's [mattpocock/skills](https://github.com/mattpocock/skills) (`skills/productivity/`), under the MIT license (see [LICENSE](LICENSE)).

## Changes from upstream

`grill-me` is unchanged. `grilling` is adapted to Claude Code's `AskUserQuestion` tool:

- Each round of questions goes through `AskUserQuestion` instead of a numbered plain-text list, with the recommended answer first and marked "(Recommended)". Rounds larger than the tool's 4-question limit are split across parallel calls.
- Text typed into the "Other" box counts as a settled answer. Options are never worded as deferrals ("let me explain").
- Design decisions (UI, layout, code/config/API shape) get a `preview` on every option for side-by-side comparison, and the chosen option's notes field is read as part of the answer. Other decisions have no previews.
- If the user rejects the `AskUserQuestion` call itself, ask before falling back to plain text.

## Install

Copy both directories into `~/.claude/skills/`. `grill-me` only invokes `grilling`, so install both.
