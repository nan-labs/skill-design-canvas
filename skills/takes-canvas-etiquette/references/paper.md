# Example tool: Paper

Optional. How the tool-neutral rules map onto Paper's MCP tools. Skip this file in other tools.

- True board count: `get_basic_info` → `artboardCount`; full list when `artboards[]` is short: `get_tree_summary` on `root_node_<pageId>` (depth 1) + `get_node_info` per id.
- Comments: `list_comment_threads` with `status: "all"`, `get_comment_thread`, `set_comment_thread_status`.
- Moving boards between pages: `move_nodes` with `parentId: "root_node_{targetPageId}"` keeps ids and positions; re-place with `update_styles` left/top. Page URL: `https://app.paper.design/file/{fileId}/{pageId}`; never `/file/{fileId}/{nodeId}`.
- Snippets in notes-style.md go in with `write_html`.

## Lessons


These rules come from a real failure: a new row landed on top of the row above it. `get_basic_info` reported 122 boards but listed only 100, so "lowest" was computed from a partial list, and the overlap check passed on that same partial list. Follow every rule here.

- **Board lists are capped at 100.** `get_basic_info.artboards[]` and `get_children` on the page root both stop at 100 top-level nodes (watch for `truncated: true`). Always compare against `artboardCount` and use the full-read procedure in step 1. A count that doesn't match your expectation (previous count + boards you added) is a red flag.
- Every write needs `fileId`; get it from the file URL or `get_basic_info`. Before the first write and before each batch, assert `fileName` is the expected file. Never call `open_file`.
- **Placing a board** (do this for every board):
  1. `create_artboard` with name, width and height. It auto-places the board, so ignore the position it returns.
  2. `update_styles` with `left` and `top` in world px.
  3. Read it back with `get_node_info`. A read straight after a write can be stale, so retry up to 3 times, 500 ms apart.
  4. Pass when both `worldX` and `worldY` are within 1px of the target. If it still fails after 3 reads, re-send `update_styles` once. If that fails too, stop and report the board id.
- **Heights:** a `fit-content` board only has its real height after its content is written (a Notes board went from 900 to 1418). Never place a band below it, or run the overlap check, from planned heights. Re-read `height` after the last `write_html` on that board.
- **Overlays stay inside their board.** A dimmed backdrop, modal, tooltip, toast or annotation is a `position:absolute` child of a `position:relative` wrapper the size of the board, with `overflow: clip` on the board. Never create an overlay with `create_artboard`. After writing, `get_children(board)` should list only the expected wrapper.
- **Replacing content in place** (`write_html` replace on the board's child) keeps the board's position, but re-read it anyway. When you take a child id from `get_children` output, skip the `file.id` value, which also matches `"id"`.
- **Screenshots straight after a write can come back blank.** If a screenshot is a single flat colour, wait 1 second and take it again before trusting it.
- Write content with `write_html` (`insert-children`), one chunk per section. Finish with `finish_working_on_nodes`.
- Fix small text with `set_text_content`; don't rewrite the board. `write_html` collapses double spaces, so `find_nodes` `textValue` must use single spaces. Wildcards: `*text*`.
- `get_screenshot` caps the longest side at about 2000px and has no region mode. For a row overview, screenshot each board and composite them locally at their live `worldX`/`worldY` × scale. Transparent boards composite over the canvas grey (about `#EEEEEE`).
- Read `isVisible` with `get_node_info`.
- **Pages the user isn't viewing are unmeasured.**
  - `fit-content` boards read `height: null`, and flex children read null positions.
  - Take a 1x `get_screenshot` and use its pixel height. Then pin `height` with `update_styles` before placing anything below it or running the overlap check.
  - Explicit `left`/`top`/`height` values read back fine.
- `get_tree_summary` cuts text at about 60 characters. Read full text (for example URLs) with `get_node_info.textContent`.
- **Scratch tests:** `create_file` (blank, clearly named "… (safe to delete)") does not switch the user's open file, and every tool works on it via `fileId` without `open_file`. Use it to try risky operations before doing them in a real file.

### What Paper can and can't do from the agent

| Can | Can't |
|---|---|
| Create and rename pages; list pages with true board counts | Reorder or delete pages; choose where a new page is inserted |
| Move boards between pages in one file, keeping ids, children and positions | Move boards between files (copies get new ids) |
| Plain-text URLs to files, pages and boards | Clickable links or rich text (`<a href>` is stripped) |
| List, read and resolve or reopen comments | Create comments or replies (at time of writing; re-check the tool list, and never resolve without the reply) |
| Full board list via `get_tree_summary` depth 1 + `get_node_info` | Page through `artboards[]` / `get_children` past 100 (no pagination) |
| Auto-place boards past 100 without overlap | Use positions from a capped list (that's how overlaps happen) |
