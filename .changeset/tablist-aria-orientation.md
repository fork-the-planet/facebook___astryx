---
'@astryxdesign/core': patch
---

[fix] TabList: stop rendering aria-orientation on the nav element. aria-orientation is not an allowed attribute on the navigation role, so it triggered a critical axe aria-allowed-attr violation (also surfaced via Toolbar and ToolbarEdgeCompensation stories that reuse the same DOM). The orientation prop still drives arrow-key navigation and the keyboard hint. (#3343)
@cixzhang
