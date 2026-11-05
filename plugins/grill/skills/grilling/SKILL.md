---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, using the **AskUserQuestion** tool — never plain-text question lists. Put your recommended answer first in the options list and mark it "(Recommended)" in the label, per that tool's own convention. Give each question a real set of distinct options (not just a recommendation plus filler). The tool caps a single call at 4 questions, so if the frontier is larger, split it across multiple AskUserQuestion calls in the same turn (in parallel) rather than trimming the frontier or falling back to text. Wait for the user's answers before the next round.

In a question without previews, the user can always answer by typing their own text directly into the built-in "Other" box instead of picking a listed option — that typed text **is** their settled answer for that decision, not a request to defer or explain later. Treat it exactly like picking a listed option: fold it into the design tree and move on to the next round. Never word a listed option as a deferral (e.g. "let me explain", "I'll clarify") — a deferral option just re-asks the same question a beat later for no reason, when the user could have typed the real answer immediately via "Other".

Previews depend on the kind of decision:

- **Design decisions** (how something looks or is laid out: a UI, a menu, a page, a diagram; or the shape of code, config or an API): give every option a `preview`, an ASCII mockup or a snippet the user can compare side by side. The user asked for this format. That layout has no "Other" box, but it gives the chosen option a **notes** field: say so in the question text when it helps ("add a note to adjust the option you pick"). Read the note as part of the answer: it may amend the option, settle a related question, or ask you to stop. Make each option something a note can amend, and include a real alternative among the options rather than counting on "Other".
- **Every other decision**: no `preview`. Without one the question keeps the free-text "Other" box; describe trade-offs in the `description` text.

A preview on any option switches the whole question to the preview layout, so a question is either all previews or none.

If AskUserQuestion itself gets rejected (the user declines the tool use, not just an option inside it), that is not necessarily a signal to stop using the tool — check with the user first. They may want the same questions re-asked through the tool rather than switched to plain text; only fall back to plain text if they say so.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
