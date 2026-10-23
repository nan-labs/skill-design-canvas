---
name: skill-design-canvas
description: >-
  Canvas etiquette for design agents working in a shared design canvas file
  (any tool with boards or frames on an infinite canvas).
  Use whenever an agent creates or iterates designs, notes or handoff boards on
  a canvas people also work in: new work goes in new rows, existing boards are
  never edited or deleted without asking, nothing overlaps, decisions stay with
  humans, notes look distinct and record the conversation (not specs), human
  comments are the brief and are replied to before resolving, and pages stay
  small through user-approved housekeeping.
license: MIT
metadata:
  version: "0.2.0"
  author: Ronan Flynn-Curran
---
# Canvas etiquette for design agents

Rules for an agent that designs on a canvas people share. The goal: a human can open the file at any point and see what the agent was asked, what it made, what was decided, and nothing of theirs has moved or vanished.

Vocabulary: a **take** is one design option produced from a prompt. A **row** is one prompt's set of takes. A **retake** is a revised take; a **comp** combines several picked takes. Humans mark decisions: ✓ picked · ★ final · ⏸ parked.

The rules are tool-neutral. Supporting detail lives in `references/`: [notes-style.md](references/notes-style.md) (exact note tokens and paste-ready snippets), [housekeeping.md](references/housekeeping.md) (pages, Contents boards, moving boards). Optional: [paper.md](references/paper.md) shows how these rules map onto one example tool, Paper; read it only when working in Paper.

## Hard rules

1. **Never edit, move, rename, hide or delete an existing board** unless the human asks for that exact change. Put new work in new rows. You may fix boards you created in this session. **Never delete boards at all**, and never move boards between pages without the user's explicit OK for that move. A design that disappears from where they left it reads as lost data (see references/housekeeping.md).
2. **Never switch the file open in the user's app.** Before every write, confirm the file name is the one you expect. If it isn't, stop and ask.
3. **Only humans set decision marks.** You may suggest one in a label's text. Never draw the chip yourself.
4. **Nothing overlaps**, counting a 32px title zone above every board, checked against **live geometry from a complete canvas read**, never against your plan.
5. **Notes never look like designs.** Use the notes style (references/notes-style.md) for everything that explains rather than proposes. **Notes explain the conversation, not the specs.** Follow Writing notes.
6. **Never lay out on a partial board list.** Many canvas tools cap or paginate board lists. If you can't prove the list is complete, stop and report.
7. **Keep pages in the soft range** (60–80 boards) and keep each page's Contents board current. See references/housekeeping.md.

## Steps

1. **Read the whole canvas and prove it's complete.**
   - Get the tool's true board count for the page and the full board list. Record both.
   - If the list is shorter than the count, it's truncated: don't use it. Fetch every top-level board another way (for example a tree read plus a per-board read) with its x, y, width, height and visibility.
   - Assert that the number of boards you collected equals the true count. Save the list (for example `live-before.json`).
   - **Read all comment threads too**, including resolved ones, and treat open human comments as the brief for this pass (see Working with comments).
   - Lowest = `max(worldY + height)` over the **full** list. The new row header goes at **lowest + 320**, x = 0. Skip the Contents board when computing the row grid. It sits above row 1.
   - **Check the page size before adding the row.** Use the board count plus the boards you're about to add: above about 80, raise housekeeping (references/housekeeping.md) in the same message as the new row; at about 95 or more, resolve housekeeping first.
2. **Add the row header**, 120 tall, spanning the row (from the first board's x to the last board's right edge). Name it `Row · {Project} · {what this row explores} · row {n}`. Include the prompt (short form), its number, who and when (PT).
3. **Add the Labels board** (exploration rows) at header y + 234: 160 tall, row width, transparent. Name it `Labels · {Project} · {what this row explores}`. One block per take, aligned to its board's x and **inset 24px from the left (text is never flush to any edge, including the board's left edge)**: what it is, then your one-line reasoning. The human's decision gets added later.
4. **Add the design boards**, top-aligned at header y + 508 (or + 234 with no Labels board), **160 apart**, in spawn order. Group a take's states (desktop → mobile → other states). Name each `{Project} · {what it is} · {variant} · {device}[ · {state}][ · {short id}]`. Place and verify each board with the placement procedure in references/paper.md.
5. **Draw the pathway** when a take is picked and a new prompt follows. Add a transparent `Pathway · {Project} · {from} → {to}` board in the row gap (lowest bottom + 40, 200 tall, row width), containing:
   - an origin dot (4.5px radius) under each picked take
   - a 1.5px `#4F46E5` arrow down to the board's bottom edge
   - a prompt chip (`#n` + short form)

   A comp is several arrows meeting one chip. A retake is a dashed arrow between sibling label blocks. A parked take keeps its place with no arrow.
6. **Put notes on the canvas** as `Notes · {Project} · {topic}` boards, written per Writing notes: what was asked, what changed, who decided what and why. **Specs, tokens, scales, property tables and CSS go in the handoff doc in the repo**, linked from the note in one line. Use inline redlines (a `notes` layer of `note · …` children in a design board's margins) only when a specific measurement is what the conversation is about.
7. **Verify on live data, then report.**
   - Make every new board visible (duplicates of hidden boards come out hidden).
   - After the **last** write, do a fresh full read (step 1 procedure, saved as for example `live-after.json`) and run the overlap check on it.
   - Screenshot each new board. Look for clipping, overflow, text flush to the edges and collisions with names. Build a row overview by compositing per-board screenshots at their live positions, including the row above, and look at it.
   - Update the page's Contents board if this row starts a new workstream.
   - Report the true board count, the number of boards read, the (empty) hit list, the live positions, and open questions.

## Naming

- Use ` · ` separators and sentence case. Designs lead with the project; other boards lead with `Row`, `Labels`, `Pathway` or `Notes`.
- Short ids (slate chips `A`, `B1`, or codes like `C3a`) go **last**.
- No `v2`, `Iteration 2`, `recommended` or `rejected` in names.
- Example: `{Project} wfC · C3a Refined desktop` → `{Project} · Front page and log · refined · desktop · C3a`.

## Writing notes

Notes are the layer the canvas is missing: the conversation behind the design. The designs already carry their own properties, so notes explain the back-and-forth that produced them: what the agent was asked, what it did, and what people decided.

**Notes track**
- **The ask:** the prompt or comment, who made it, and when.
- **The change:** how this version differs from the last one, and why.
- **Decisions:** who decided what. For example, "Alex decided…", "Sam confirmed…", "Picked B over A because…".
- **Framing:** "Here we're showing…", and any earlier discussion or context it builds on, as a link.
- **Open questions**, if there are any.

**Notes leave out**
- Specs, tokens, scales, every property or parameter. They're in the design and the repo handoff. Link the handoff once.
- Outside material such as research, docs, guidelines or licence text. Don't bring it in; add a link for anyone who wants to dig deeper.
- Best practices and icon or type families. Name the practice in one line, add an external link if it helps, and stop. No paragraphs.

**Principles**
- **Don't make me think.** A reader should get it in about 10 seconds.
- **Don't overstate.** No hype and no hedging.
- **Say it once.** Don't repeat what the design, its label or the row header already says.
- **Bold, plain language for decisions.** Write "Alex picked the eye icon.", not "the eye may be preferred".
- **Say why, and whose call it was.** One short clause: "because Lucide feels overused", "Alex asked for…".
- **Keep it short.** Aim for 3–6 lines, or about 80 words, per section, with a whole notes board fitting on about one screen (1440 × 900). This is a target with flex, not a cutoff. If it runs long, cut detail and link out.

**Example**
- ❌ A board with the icon library's version and licence, stroke and size specs, a 14-row icon table, colour tokens, a contrast table and pasted guidance on icon conventions.
- ✅ "**Icons: Iconoir.** Alex asked for small left icons from a family that isn't overused in 2026, so not Lucide or Heroicons. **Alex changed two:** visibility is now an eye, and Manage all boards is a lightning bolt. Full icon map and specs: `HANDOFF.md` (link). Iconoir: iconoir.com"

## Grid

| Rule | Value |
|---|---|
| Between boards | 160 |
| Between rows | 320 |
| Row header | 120 tall |
| Header → first band | 234 (top to top) |
| Labels board | 160 tall, then 114 to the designs; text inset 24px from the left |
| Title zone | 32 above every board |
| Alignment | Top-aligned in a row |

## Working with comments

Human comments are the brief for the next pass. Read them before you design anything.

- **Read first.** At the start of any iteration on a file, list every comment thread, including resolved ones (some tools list only open threads by default), and read each thread in full. Record the thread id, author, time (PT), text, status, and the board it's pinned to, with the board's name. Use the pin offset and a screenshot to see which element the comment points at.
- **Map every comment to one outcome:**
  1. **Change the design:** as a new row or take, per the hard rules. Never edit the commented board.
  2. **Hand to the builder:** for production, with the concrete change, file or area, and an acceptance check.
  3. **Already done.**
  4. **Question for the user.**
- **Verify before calling it done.** Check the live site or the repo (or the canvas, for design changes). A comment isn't done because a brief was sent.
- **Resolve with care.** Resolve only after the change is verified live or on the canvas, and only with the user's OK. A standing OK for "addressed comments" counts. Never resolve someone's question or an open decision. Keep threads open while their change is pending, and revisit them after it ships.
- **Reply before you resolve. Never resolve without a reply.** A human opening the thread later must see why, when and how it closed, not just a status change. Post this reply on the thread, then resolve:
  - `Changes made · {d Mon YYYY}, {h:mm} PT` (add `· by {agent}` if several agents work the file).
  - Add one short line saying what changed and where, if it isn't obvious (for example `Shipped in production` or `Built as take B2 in row 14`).
  - **Link to where the change lives if it isn't the commented frame:** a new take or row, another board or page, or production (`…/file/{fileId}/{pageId}/{nodeId}`, a live URL, or a commit or deploy). If the change replaced the content of the commented frame in place, no link is needed.
- **Order: reply, read the reply back, then set the thread to resolved, then read the thread back.** If the reply fails or the tool can't post replies, don't resolve. Leave the thread open and give the user the exact reply text (with the thread id and board) to post or approve.
- **Resolving is a write.** Assert `fileName` first (hard rule 2), touch only the thread ids you mean to, and read each one back to confirm its reply and status.
- **Report the outcome yourself as well:** in chat, the handoff doc or a notes board, list which threads you replied to and resolved and which are still open (with ids and why).

## Overlap check

Run it on **live** geometry from a **complete** read, never on the plan, and only **after** the last write (content, fit-content heights, moves).

```python
# boards = full live list from step 1's procedure, each {id, x, y, w, h}
assert len(boards) == board_count, "truncated read: do not trust"
R = [(b["id"], b["x"], b["y"] - 32, b["w"], b["h"] + 32) for b in boards]
hits = [(a[0], b[0]) for i, a in enumerate(R) for b in R[i+1:]
        if a[1] < b[1] + b[3] and b[1] < a[1] + a[3] and a[2] < b[2] + b[4] and b[2] < a[2] + a[4]]
assert not hits, hits
# also: header_y >= max bottom of pre-existing boards + 320, and every new board's y >= header_y
```

It passes when the read is complete, there are no hits, every new board has `isVisible: true`, and every new board's live position matches its target within 1px.

**On failure:** move only boards you created this session, as a whole row by one delta, so the grid is kept. Re-read and re-check. Never move someone else's board. If it still fails after one fix pass, stop and report the ids, the live numbers and the hit pairs. Don't claim success.

## Dispatching canvas work to another agent

Paste the full-read rule into the brief: "Read every board (compare the list length with the true board count; if truncated, fetch the rest another way), place from the true lowest bottom, verify each move by reading it back, and run the overlap check on a fresh complete read after the last write." A brief that only says "lowest + 320" leaves the truncation trap open.
