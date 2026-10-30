# KepubDoctor

An Android app that repairs EPUB files and converts them to Kobo's KEPUB format, tuned for e-readers.
It's been specially made to fix common mistakes I've found in scraped books I've downloaded but it also works great for normal books. Everything runs on your phone: no internet access, no accounts, no permissions.

## Features

### Output formats
- **Repaired EPUB**: the same book with its structure and content problems fixed.
- **KEPUB** (`.kepub.epub`): a Kobo-native file with reading-position spans, so progress, highlights and stats work like a store-bought book.

Files are saved with a `_healed` suffix through the system file picker, so the original is never touched.

### Structure repair
- Fixes or recreates `mimetype` and `META-INF/container.xml`.
- Removes manifest entries that point to missing files (including file names with spaces or special characters, written as `%20` in the OPF).
- Generates a missing table of contents (`nav.xhtml` for EPUB 3, `toc.ncx` for EPUB 2).
- Adds the `cover-image` property that Kobo needs to detect covers.
- Adds a blank standalone cover page when the book does not start with one, so Kobo does not apply the cover layout to the first chapter.
- Normalizes language codes and cleans unneeded Calibre metadata while **keeping series info**, so Kobo still groups books by series.
- Re-encodes files to UTF-8 and repairs malformed XHTML.

### Content cleanup
- Decodes double-escaped HTML entities (for example `&amp;#8230;` becomes `…`).
- Recovers stray tags that were left as visible text (underline, bold, etc.) as real formatting.
- Moves spaces that sit next to italic or bold tags inside the tag.
- Removes empty `div`/`span` leftovers from ads and widgets, while keeping anchors that links point to and CSS-drawn scene separators.
- Removes empty "spacer" paragraphs and collapses double line breaks.
- Normalizes paragraph margins without losing horizontal margins.
- Strips Unicode replacement characters, Microsoft Word leftovers and empty `<!--sse-->` comments.
- **Custom phrases**: type any text you want removed from every chapter (site watermarks, "read at ..." banners, etc.), one phrase per line. Matches are literal and case-sensitive, also when the phrase is split across tags. Your list is saved between sessions.

### Made for e-readers
- Removes fixed margins and widths from `html`, `body` and `@page` so your reader's margin settings apply.
- Removes fixed font sizes, line heights and font families (in px/pt) from `html`, `body` and `p`, so your reader's own settings win. Fonts embedded by the book on purpose are kept.
- Optional **first-line indent** switch: for books with no styling at all (typical for web novels), adds a minimal stylesheet with a first-line indent, no gaps between paragraphs and justified text. Off by default; it has no effect on books that already have styles.
- Resizes JPG and PNG images wider than 1448 px (Kobo Clara BW screen size). Images that already fit are never touched, and a resized copy is only used if it is actually smaller.
- Removes unused images, fonts and stylesheets that nothing in the book references.
- Stores already-compressed files (images) without recompressing them, so saving is faster.

### KEPUB conversion
- Wraps text in sentence-level `koboSpan` elements with the ids Kobo expects.
- Adds the `book-columns` / `book-inner` containers.
- Preserves the spacing between sentences, so words never end up glued together.
- Converts straight quotes, `...` and `--` to proper typographic characters.

### Safety
- DRM-protected books are detected and rejected. Books that only obfuscate embedded fonts are accepted.
- Protected against Zip Slip (malicious paths inside the archive).
- If something goes wrong while optimizing, the original data is kept. Temporary files are cleaned up.

## How to use

1. Open the app and tap **Select EPUB**.
2. Choose the output format: **EPUB (repaired)** or **KEPUB**.
3. Optionally add phrases to remove (one per line) and turn on the first-line indent switch.
4. Tap **Process and save**, check the report of what was fixed, and pick where to save the result.
5. Copy the file to your e-reader. For Kobo, use the `.kepub.epub` file.

## Limitations

- DRM-protected books are not supported.
- Styles written in `<style>` blocks inside chapters are not adjusted, only `.css` files.
- Image resizing covers JPG and PNG only.
- The first-line indent switch only applies to books that have no styling.

## Screenshots
<table>
  <tr>
    <align="center">
      <img width="1220" height="2712" alt="Screenshot_2026-10-30-10-52-25-131_com example kepubdoctor" src="https://github.com/user-attachments/assets/a6f7bb89-9a99-4f68-8847-afea7304dedb" />
    </td>
  </tr>
</table>



