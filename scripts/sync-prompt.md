You are running inside a GitHub Action for the "วังหมีกระทิง Run Learn Land" project hub.

`data/slides.txt` (plain-text export of the team's Google Slides) has just been updated.
Run `git diff data/slides.txt` to see exactly what changed, then propagate those changes into:

1. **`index.html`** — the live dashboard (Thai). Update only the affected sections:
   - `#overview` metrics — if dates, revenue/expense targets, or participant counts changed
   - `#routes` / `#packages` / `#registration` / `#rules` / `#impact` / `#safety-code` / `#faq` — public-facing
     event info; update if the slides confirm items currently marked with a `.tbd` "รอยืนยัน" chip
     (replace the chip with the confirmed value) or change routes/packages/rules
   - `#tasks` — add/remove/update pending action items; keep owner tags (`tag owner`) and due tags (`tag due`); mark resolved items by removing them or converting the dot to `dot good` + `tag done`
   - `#depts` — if heads/members/homework/goals changed in a department card
   - `#finance` — expense bars (keep widths proportional, max bar = largest item) and revenue tables; keep `tabular-nums` formatting with comma thousands separators
   - `#meetings` — append a new `.t-item` for any new meeting date found (keep chronological order, keep the amber `.t-item.next` as the last "upcoming" entry and update its expectations)
2. **`slides-content.md`** — the structured Thai summary. Append/update the matching meeting section so it stays a faithful, organized record of the slides.

Hard rules:
- The team sometimes updates figures in the hub AHEAD of the slides (marked "อัปเดต <date>" in
  #finance / #overview, e.g. expense total 1,101,000 and venue "วิสาหกิจชุมชนนวัตกรรมเกษตรเวลเนสวังหมี"
  updated 15 ก.ค. 2569). Do NOT revert such values to older slide figures — only change them when the
  slide diff shows the slides catching up or explicitly revising them again.
- Do NOT touch the CSS design system, the cover image block, the countdown script, or the overall layout. Content edits only.
- Do NOT change the hero countdown target date unless the slides explicitly change the event date (currently 25–27 ธ.ค. 2569).
- Keep everything in Thai matching the existing tone. Keep numbers as they appear in the slides; if slide numbers are internally inconsistent, keep the slide value and add a short warning note (pattern: the existing ส้วม 550,000 footnote).
- If the diff is trivial (whitespace, reordering, typo fixes with no semantic change), update `slides-content.md` only if meaningful; otherwise make no edits at all.
- Never add content that is not in the slides.

The workflow commits `data/slides.txt`, `index.html`, and `slides-content.md` after you finish — you do not need to run git commit yourself.
