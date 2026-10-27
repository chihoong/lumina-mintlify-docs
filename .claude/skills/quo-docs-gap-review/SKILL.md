---
name: quo-docs-gap-review
description: Review recent conversations on the Quo support and sales lines (Support, Whiteglove Support, Sales, GA Sales), compare what customers and prospects asked — and what the team told them — against the Lumina Mintlify docs in this repo, and propose concrete doc updates for gaps, mistakes, and hard-to-find content. Use whenever the user wants to audit the docs against real customer conversations (e.g. "what are the docs missing based on this week's texts", "review Quo for doc gaps", "find mistakes in the support docs", "propose doc updates from support and sales conversations"). Read-only on Quo; writes a findings report and only edits .mdx pages after the user approves specific proposals.
---

# Quo docs gap review

Find where the Lumina docs fail real users. Each run reads recent texts on the
Quo support and sales lines, works out what people needed to know, checks the
docs in this repo for it, and proposes specific MDX changes.

A finding is one of:

| Type | Meaning | Example |
|---|---|---|
| **Gap** | People asked about it and the docs don't cover it | "Can I run two kiosks on one Square location?" — no page answers it |
| **Mistake** | The docs contradict what the team told customers, or what customers saw in the app | Docs say **Restart App** is under **Settings**; support told 3 customers it's under **Troubleshoot** |
| **Findability** | The docs cover it, but people still asked — the answer is buried, mistitled, or missing a cross-link | Reader pairing is in `troubleshooting.mdx` but customers search for it during setup |
| **Team inconsistency** | Agents or reps gave answers that contradict the docs or each other | Not a doc change by default — flag it for the team |

## Defaults for this workspace

- **Inboxes:** read all four lines unless the user names specific ones:
  - Support — `+19499195467` (inbox id `PNwFRiOqRU`)
  - Whiteglove Support — `+12135315467` (inbox id `PNJTSrJER5`)
  - Sales — confirm number and id with `list-inboxes`
  - GA Sales — confirm number and id with `list-inboxes`

  Never read the Sandbox inbox. Inbox ids can change, so always confirm with
  `list-inboxes` before pulling.
- **Lookback window:** the last 7 days ending now, unless the user gives a
  different range. Compute `createdAfter` from the current date.
- **Docs in scope:** every page listed in the `navigation` of `docs.json`, plus
  `whats-new.mdx`. Skip `component-showcase.mdx` and anything under `sources/`
  (that folder is a scraped copy of the old site, not the source of truth).
- **Report file:** `drafts/docs-gap-review/<YYYY-MM-DD>.md`. `drafts/` is in
  `.mintignore` so it never publishes, and `drafts/docs-gap-review/` is in
  `.gitignore` because reports are derived from customer conversations.
- **Quo tools:** `list-inboxes`, `fetch-messages`. `list-contacts` /
  `get-contact` only if you need to tell a customer from a teammate. This skill
  never calls `send-message`.
- **House rules:** read `AGENTS.md` before writing any proposal. Its
  terminology, style, and content boundaries apply to every proposed edit.

## How Quo returns messages

`fetch-messages` returns a flat, reverse-chronological list. Each message is a
header line plus a body:

```
AC<id> | <participant phone> | <direction> | <ISO timestamp>
<message body (may be blank for media-only messages)>
```

- `incoming` — the customer or prospect wrote it.
- `outgoing` — the Lumina team wrote it.
- In multi-participant mode the response ends with `[conversation: PHONE = CN...]`
  legend lines and may include `[more_available: true]` plus a
  `[next_conversations: <token>]` token.

## Workflow

1. **Confirm the inboxes.** Call `list-inboxes`. Match Support and Whiteglove
   Support by number, and Sales and GA Sales by name. If a line is missing or
   ambiguous, say so and continue with the rest. Don't guess.

2. **Pull the window from each inbox.** Call `fetch-messages` with that
   `inboxPhoneNumber`, `createdAfter`, and `maxResults: 100`. Multi-participant
   mode caps at 100 conversations and doesn't support `pageToken`, so page with
   `conversationPageToken` set to the `[next_conversations: ...]` token while
   `[more_available: true]`. If one participant's thread is truncated,
   re-fetch with `participantPhoneNumber` and follow `pageToken`. Tag every
   thread with its line and whether it's a **support** or **sales** line.

3. **Reconstruct and filter threads.** Group by participant, sort oldest to
   newest, and merge consecutive same-direction messages sent within a few
   minutes into one turn. Drop:
   - Outbound broadcasts — near-identical outgoing messages sent to many
     participants with no reply.
   - Greetings, "sorry we missed your call" openers, pleasantries, and
     agent hand-off chatter.
   - Media-only messages with no text.

4. **Extract what people needed to know.** For each thread, write down:
   - **Support lines:** each underlying question or problem, what the agent
     told them, and whether it resolved the issue. Note any steps, menu paths,
     button names, or settings the agent named — these are what you check
     against the docs.
   - **Sales lines:** questions about how the product works (hardware,
     Square, setup time, menu sync, loyalty, team access), objections that
     came from a misunderstanding, and any capability the rep described.
     Ignore pricing, discounts, contract terms, and negotiation — those don't
     belong in the docs.

   Generalize each item into a self-contained question with no names, phone
   numbers, store names, addresses, or order numbers. Match on meaning, so
   "does the reader plug into the iPad" and "do I cable the Square reader to
   the tablet" are one item. Keep a count of how many threads raised it and
   which lines they came from.

5. **Index the docs.** Read `docs.json`, then read every in-scope page in full.
   For each page note its title, `description`, headings, and the concrete
   facts it states (menu paths, button labels, steps, limits, requirements).
   Read the pages themselves; don't rely on titles alone.

6. **Check each item against the docs.** For every extracted item, search the
   indexed pages and decide:
   - **Covered and correct** — drop it, unless it was raised 3+ times; then
     it's a **Findability** candidate.
   - **Not covered** — **Gap**.
   - **Covered but contradicted** by what the team said, or by what the
     customer reported seeing in the app — **Mistake**. Decide which side is
     likely right:
     - If several agents consistently say the same thing and it differs from
       the docs, the docs are probably stale.
     - If one agent contradicts the docs and others agree with them, it's a
       **Team inconsistency**, not a doc mistake.
     - If you can't tell, propose the change and mark it
       `{/* UNVERIFIED: ... */}` as `AGENTS.md` requires.
   - **About something the docs shouldn't cover** (pricing, contracts, server
     internals, API error codes, rollout flag names) — drop it and list it
     under "Out of scope" in the report.

7. **Rank the findings.** Order by impact: Mistakes first (wrong docs cause
   repeat tickets), then Gaps and Findability by how many threads raised them,
   then Team inconsistencies. Support-line evidence outweighs sales-line
   evidence for how the product works today; sales-line evidence is strongest
   for what prospects need to know before buying.

8. **Draft proposals.** For each Mistake, Gap, and Findability finding, write
   a concrete change:
   - Target file and the heading it goes under (or "new page" with a proposed
     filename and `docs.json` group).
   - The exact MDX to add or replace, as a before/after block. Follow
     `AGENTS.md`: second person, active voice, sentence-case headings, **bold**
     UI labels, Remy vs. Lumina Kiosk naming, "teammate" not "member",
     "the Admin role", "invite" not "invitation". Reuse the page's existing
     components (`<Steps>`, `<Note>`, `<Warning>`, etc.) rather than inventing
     new structure.
   - For Findability: a better `title` / `description` / heading, a
     cross-link from the page where people look first, or an entry in
     `troubleshooting.mdx`.
   - Any fact you can only source from a text conversation and can't confirm
     against the Remy app gets an `{/* UNVERIFIED: ... */}` comment.

   Team inconsistencies get a short note for the team (what was said, what the
   docs say, how many times) and no doc edit unless the user asks for one.

9. **Write the report** to `drafts/docs-gap-review/<YYYY-MM-DD>.md` using the
   format below, then summarize it in chat: window covered, threads read per
   line, and counts per finding type. List the top proposals by number.

10. **Apply only what the user approves.** Ask which proposals to apply (by
    number). Then:
    - If on `main`, create a branch named `docs/quo-gap-review-<YYYY-MM-DD>`.
    - Make the edits with the Edit tool, matching each page's existing style.
    - Add any new page to `docs.json` navigation.
    - Run `mint broken-links` and fix anything your edits broke.
    - Don't commit, push, or open a PR unless the user asks.

## Report format

```markdown
# Docs gap review — <start date> to <end date>

Threads read: Support <n> · Whiteglove Support <n> · Sales <n> · GA Sales <n>
Findings: <n> mistakes · <n> gaps · <n> findability · <n> team inconsistencies

## Proposals

### 1. [Mistake] <short title>
- **Page:** `troubleshooting.mdx` › Reader connection
- **Evidence:** raised in 4 threads (Support 3, Whiteglove 1). Customers
  couldn't find **Add a Reader**; agents directed them to
  **Admin Dashboard** › **Troubleshoot** › **Square Payment or Reader**.
- **What the docs say now:** <quote the current line>
- **Proposed change:**
  ````mdx
  <before/after MDX>
  ````
- **Confidence:** high / medium / low (UNVERIFIED)

### 2. [Gap] <short title>
...

## Team inconsistencies
- <topic> — agents said X in <n> threads and Y in <n>; docs say <Z>.

## Out of scope
- <topic> (<n> threads) — pricing / contract / internal; not a docs change.

## Unanswered questions
- <generalized question nobody answered> (<line>, <date>)
```

## Guardrails

- **Read-only on Quo.** Never call `send-message` or change anything in Quo.
- **Strip PII.** No customer or prospect names, phone numbers, addresses,
  store names, or order numbers in the report or the docs. Quo is not HIPAA
  compliant, so never carry health information forward.
- **Don't invent facts.** Every proposed sentence must trace to something the
  team said, something a customer observed, or existing docs. If the
  conversations don't settle how a feature works, say so and mark it
  `UNVERIFIED` rather than filling the gap with a guess.
- **Keep sales out of the docs.** Pricing, discounts, promotions, contract
  terms, and anything a rep promised about the roadmap stay out of proposed
  edits. List roadmap promises under "Team inconsistencies" if they
  contradict the docs.
- **Don't edit without approval.** The report is the deliverable. Edit `.mdx`
  files only for proposals the user picks, and never touch `sources/`.
