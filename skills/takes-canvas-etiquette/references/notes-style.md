# Notes style

Exact tokens for notes, labels, row headers and pathways, so explanation never looks like a design proposal.

| Token | Value |
|---|---|
| Font | `"IBM Plex Mono", "JetBrains Mono", ui-monospace, monospace` |
| Background | `rgba(79,70,229,0.05)`, **never white**. Labels and Pathway boards are transparent |
| Heading | `#4338CA` |
| Body | `rgba(55,48,163,0.86)` |
| Meta, kicker, ids | `#4F46E5`, no opacity |
| Chip fill | `rgba(79,70,229,0.08)` |
| Rule | `rgba(67,56,202,0.18)` |
| Arrows | `#4F46E5`, 1.5px |
| Final (★) chip | `#4338CA` fill + white text |
| Type | title 22/30 500 · row header 20/28 500 · h2 15/22 600 · body 13/20 · small 12/18 · annotation 11/16 500 |
| Padding | notes board 64/64/72 (**never under 48 left/right**) · row header 0 × 48 · label blocks 24 left (never flush to an edge) · chip 6 × 10, radius 6 · annotation 4 × 8, radius 4 |
| Dark canvas | `#A5B4FC` / `rgba(199,210,254,0.86)` / `#818CF8` on `rgba(129,140,248,0.08)` (less tested) |
| If the design is indigo | Notes switch to cyan: `#155E75` / `rgba(22,78,99,0.86)` / `#0E7490` |

## Ready-to-paste snippets (for tools that accept HTML or CSS-like styles)

**Row header.** Create the board first, then insert this as its child:

```json
{"name": "Row · {Project} · {what this row explores} · row {n}",
 "styles": {"width": "{row width}px", "height": "120px", "backgroundColor": "rgba(79,70,229,0.05)", "display": "flex", "flexDirection": "column", "overflow": "clip"}}
```
```html
<div style="display:flex;align-items:center;justify-content:space-between;height:120px;padding:0 48px;width:100%;box-sizing:border-box"><div style="display:flex;flex-direction:column;gap:6px"><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:20px;line-height:28px;font-weight:500;color:#4338CA">{What this row explores}</div><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:13px;line-height:20px;color:rgba(55,48,163,0.86)">Prompt #{n} · “{short prompt}” · {who} · {day}, {time} PT</div></div><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:12px;line-height:18px;letter-spacing:0.04em;color:#4F46E5">{Project} · row {n}</div></div>
```

**Notes board.** Typically one to three sections: what was asked, what changed and why, and decisions or open questions. Create the board, insert the title, then one call per section, then set `height: fit-content` and re-read its height:

```json
{"name": "Notes · {Project} · {topic}",
 "styles": {"width": "1440px", "height": "900px", "backgroundColor": "rgba(79,70,229,0.05)", "display": "flex", "flexDirection": "column", "gap": "40px", "paddingLeft": "64px", "paddingRight": "64px", "paddingTop": "64px", "paddingBottom": "72px"}}
```
```html
<div layer-name="Title" style="display:flex;flex-direction:column;gap:10px;width:1312px"><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:12px;line-height:18px;letter-spacing:0.04em;color:#4F46E5">{Project} · {topic}</div><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:22px;line-height:30px;font-weight:500;letter-spacing:-0.01em;color:#4338CA">{Notes title in plain words}</div><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:13px;line-height:20px;color:rgba(55,48,163,0.86);width:960px">{One sentence: what was asked and what this board shows.}</div></div>
```
```html
<div layer-name="{Section name}" style="display:flex;flex-direction:column;gap:16px;padding-top:28px;border-top:1px solid rgba(67,56,202,0.18);width:1312px"><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:15px;line-height:22px;font-weight:600;color:#4338CA">{Section heading}</div><div style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:13px;line-height:20px;color:rgba(55,48,163,0.86);width:960px">{3–6 short lines: the ask, the change, the decision and why. Link out for detail.}</div></div>
```

**Prompt chip** (Pathway board) and **inline annotation** (inside a design board's `notes` layer):

```html
<div style="display:flex;align-items:center;gap:8px;padding:6px 10px;border-radius:6px;background:rgba(79,70,229,0.08);border:1px solid rgba(79,70,229,0.28)"><svg width="14" height="14" viewBox="0 0 16 16" fill="none" stroke="#4F46E5" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" style="flex-shrink:0"><path d="M3 3.5h10v7H7.5L4.5 13v-2.5H3z"/></svg><span style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:12px;line-height:18px;color:#4F46E5">#{n}</span><span style="font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:12px;line-height:18px;font-weight:500;color:#4338CA">{short prompt, ≤ 60 characters}</span></div>
```
```html
<div layer-name="note · {what it explains}" style="position:absolute;left:{x}px;top:{y}px;padding:4px 8px;border-radius:4px;background:rgba(79,70,229,0.08);font-family:'IBM Plex Mono','JetBrains Mono',monospace;font-size:11px;line-height:16px;font-weight:500;color:#4338CA;white-space:nowrap">{card 232 wide · padding 16 · radius 12}</div>
```
