# Pages and housekeeping

Canvas tools slow down, and some truncate their board lists, as pages grow, so keep each page small. Housekeeping is a conversation with the user, never a silent clean-up.

**Soft range: 60–80 boards per page.** This is a target with flex, not a cutoff.
- Before every new row, get the page's true board count (not the length of a possibly capped list).
- **Past about 80:** add the row, but propose housekeeping in the same message.
- **About 95 or more:** stop and agree housekeeping first. Past 100, lists truncate and layout breaks.
- Also raise it unprompted when a page mixes unrelated workstreams or old explorations bury current work.

**Propose, don't act.** Give a row-by-row split the user can approve or edit:

| Row | What it is | Proposal |
|---|---|---|
| row n · {what it explores} | current work | stay |
| row m · {what it explores} | separate workstream | → page `{Workstream}` |
| row k · {what it explores} | explored, not chosen (e.g. early brand or look-and-feel explorations) | → page `Archive` |

- The first page, where active work lands, is named `In progress`. Never leave it as the tool's default name (such as `Page 1`); rename it the first time you work in a file. It reads as a status that pairs with `Archive`.
- Workstream pages are named in plain words (`Brand`, `Onboarding`, `Archive`). Archive holds discarded or unpicked iterations, kept for reference and never deleted.
- Moves happen only after the user explicitly OKs that split. Move whole rows (header, Labels, Pathway, designs) together.
- Ask before moving any board you didn't create. Flag boards made by other people separately.
- Never delete. If something looks redundant, say so and let the user decide.
- **Pages can't be reordered or deleted from the agent.** A new page is inserted **right after the page the user is viewing**, so create pages in reverse of the order you want. Ask the user to drag pages if the order matters.

### Contents board

Every page with more than one row gets one short Contents board, at the top-left above row 1: x = the page's leftmost x, bottom edge 320 above the topmost board (negative y is fine). It uses the notes style, is about 880 wide, and has 48px padding.
- Name: `Contents · {Project} · {page name}`.
- Sections:
  1. **Pages:** one line per workstream (`{page} · {what lives there}`) with its page URL.
  2. **Moved:** a dated log (`{d Mon} · {board or row name} · {from} → {to}`) with the new URL.
  3. A meta line: `Updated {date}, {time} PT`.
- **Links are plain text.** Some tools strip links silently, so write URLs as plain text for copy and paste.
  - Prefer page URLs: they stay valid while the page exists.
  - Board URLs can go stale when a board changes page. Use them only in the Moved log, and re-point them on every move.
- Update the Contents board on **both** pages in the same pass as any move.
- To find stale links, search the page text for the old page id.
- Optionally leave a small stub notes board in the freed slot: `Notes · {Project} · moved to {page}`, with the date, what moved and the new URL.

### Moving boards between pages

Do this only after the user's OK.

1. Full-read both pages (step 1 procedure). Save the before lists.
2. Move the boards with the tool's move operation, one row per batch, keeping ids so references and comments stay attached. If positions are kept, the moved boards will likely collide on the target page.
3. Re-layout the moved row on the target page as a new row:
   - its header goes at the target's lowest + 320
   - shift every board in the row by one delta, so the grid is kept
4. Update the Contents boards on both pages, and add the stub if wanted.
5. Full-read both pages again:
   - source count drops by the number of boards moved, and the target count rises by the same number
   - run the overlap check on the target page
6. Report to the user what moved, where to, and the Contents board(s) to look at.

Don't move between files: copies usually get new ids, which break links. Keep housekeeping to pages within one file.
