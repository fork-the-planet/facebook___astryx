---
'@astryxdesign/core': patch
---

[fix] CheckboxListItem: drop the invalid `aria-checked` from the list row. `aria-checked` is not allowed on `role="listitem"` (axe: aria-allowed-attr); checked state is already conveyed by the row's inner checkbox. This also fixes Markdown task lists, which render task items through CheckboxListItem. (#3343)

@cixzhang
