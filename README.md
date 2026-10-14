# WordCraft for iPad

Static web build of [WordCraft](https://github.com/storytold/wordcraft) (upstream commit `04f6f53`), hosted on GitHub Pages so it can be opened in Safari on iPad and added to the Home Screen.

WordCraft is © its authors and licensed under MIT or Apache-2.0 (see `LICENSE-MIT`, `LICENSE-APACHE`). Bundled fonts and icons keep their own licenses, listed in `ATTRIBUTION.md`. "ArtCraft" and its logos are trademarks of their owner; this is an unofficial personal build.

Rebuild: `cd apps/wordcraft-web && trunk build --release` in the upstream repo, then copy `dist/web/*` here.

## Arabic support

The upstream web build has no Arabic font (it cannot read system fonts in the browser), so Arabic showed as boxes. This build bundles Noto Naskh Arabic Regular/Bold (SIL OFL 1.1, see `OFL-NotoNaskhArabic.txt`) as a document and interface font; the code change is in `arabic-font.patch` (fonts go in upstream `assets/fonts/`).
