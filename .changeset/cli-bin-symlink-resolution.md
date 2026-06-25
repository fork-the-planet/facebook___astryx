---
'@astryxdesign/cli': patch
---

[fix] `npx astryx` now works when the CLI is installed as a real npm package.
The bin imported its `../src/*` modules relative to the invoked path, so running
through the `node_modules/.bin/astryx` symlink made them resolve outside the
package (`ERR_MODULE_NOT_FOUND: .../node_modules/src/...`) on Node versions that
don't realpath the bin entry. It now resolves siblings via the bin's real path
(realpath of `import.meta.url`), working whether invoked via symlink, copy, or
Windows shim. Also fixes the non-interactive `init`/`theme` error to say
`astryx <command>` instead of the stale `xds <command>`.
@josephfarina
