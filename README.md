# WordCraft for iPad

Static web build of [WordCraft](https://github.com/storytold/wordcraft) (upstream commit `04f6f53`), hosted on GitHub Pages so it can be opened in Safari on iPad and added to the Home Screen.

WordCraft is © its authors and licensed under MIT or Apache-2.0 (see `LICENSE-MIT`, `LICENSE-APACHE`). Bundled fonts and icons keep their own licenses, listed in `ATTRIBUTION.md`. "ArtCraft" and its logos are trademarks of their owner; this is an unofficial personal build.

Rebuild: `cd apps/wordcraft-web && trunk build --release` in the upstream repo, then copy `dist/web/*` here.

## Arabic support

The upstream web build has no Arabic font (it cannot read system fonts in the browser), so Arabic showed as boxes. This build bundles Noto Naskh Arabic Regular/Bold (SIL OFL 1.1, see `OFL-NotoNaskhArabic.txt`) as a document and interface font; the code change is in `ipad.patch` (fonts go in upstream `assets/fonts/`).

## Touch and Apple Pencil

eframe turned both the pointer events and the touch events of one touch into presses, so every tap arrived twice and read as a double-click: a tap selected a word instead of placing the caret, and drags were unreliable. `index.html` now keeps only the touch path for finger and pen (mouse unchanged) and turns off the browser's own panning, selection callout and magnifier on the canvas. Included in `ipad.patch`.

Two fingers scroll the page: a first touch is held back 90 ms, and a second finger landing in that window turns the gesture into a scroll (sent to the editor as wheel events); otherwise the press is replayed and one finger or the pen selects as before.

Scrolling (rev. 2): Safari's pinch zoom is blocked; finger motion is locked to one direction, sent as small wheel steps (the editor eases large ones, which made the page jump) and keeps gliding after the fingers lift. A strip on the right edge scrolls with one finger. Hidden on devices with a mouse.

### Copy, cut and paste by touch
Hold a finger still on the page (about half a second) to open a bar with Cut, Copy, Paste,
Select All and More… (the editor's full right-click menu). Holding no longer presses, so the
current selection is kept. The bar also appears after selecting text by dragging a finger.
Copy and Cut write the system clipboard; Paste reads it (Safari asks to allow it once per paste)
and falls back to the editor's own last copy.

## Zotero

The desktop Zotero integration needs the Zotero app on the same computer, which a web page cannot reach. The red **Zotero** button opens a panel that searches the user's zotero.org library through the Zotero Web API (API key only, stored in the browser; the user ID comes from `/keys/current`) and adds the chosen items as document sources, so WordCraft's own live citations, citation styles and bibliography cite them. The web shell runs commands the page queues in `window.__wcQueue` (see `apps/wordcraft-web/src/web.rs` in `ipad.patch`).

### Chicago 17th / 18th edition full note
The Zotero panel's style list starts with "Chicago 17 – full note" and "Chicago 18 – full note".
Each citation is inserted as a footnote: the first citation of a source is a full note, later
ones are short notes, and the 17th edition uses "Ibid." when the same source is cited twice in a row.
The 17th edition keeps the place of publication and uses "et al." from four authors; the 18th drops
the place and uses "et al." from three. "Insert bibliography" builds a Chicago bibliography from the
sources cited in the footnotes. Notes are plain text, not live fields.
