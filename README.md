# andrewlehman.com

Two files, no build step, no dependencies past Google Fonts.

```
index.html    content + ~120 lines of vanilla JS
style.css     the design system
```

Open `index.html` in a browser to run it. Push both files to any static host to ship it.

## How the page works

Everything sits in one index of 14 items: roles, papers, and service in a single list.
Three controls re-sort that list without reloading anything.

**Lenses** (`Arrange by` in the sticky bar) regroup the same items three ways:

| Lens | Groups by | Reads as |
|------|-----------|----------|
| Thread | primary thread | what the work has in common |
| Time | nothing, one run | a chronology, with span bars |
| Kind | role / paper / service | a conventional CV |

**Threads** are the cross-cutting tags: `evidence`, `tooling`, `teams`. Click a thread card
at the top, or a chip inside any expanded item, and the index filters to everything carrying
that thread. Click again to clear. An item can carry more than one.

**Items** collapse to a title and a date. Click the header to expand.

**The constellation** above the controls plots the same 14 items, year across, thread down.
Roles and service draw as spans because they have duration; papers draw as dots because they
happened once. A vertical hairline links an item to any second thread it carries. Hover a mark
to name it, click to jump to it.

Nothing about the plot is hand-positioned. It reads the same `data-` attributes as the index,
packs each band into lanes so overlapping spans never sit on top of each other, and sizes its
own viewBox to whatever the busiest band needs. Add five more items to one thread and it grows
to fit. It carries `aria-hidden` because it repeats the index directly below it, so screen
readers get the content once instead of twice.

## Portrait

Drop a square `portrait.jpg` next to `index.html` and the masthead picks it up. Until then it
shows an `AL` monogram, and it falls back to the monogram if the file ever goes missing. The
image sits under a grayscale and duotone treatment that clears on hover, so it reads as part of
the palette rather than a photo pasted onto it. Roughly 600×600 is plenty.

## Adding an item

Copy an existing `<article class="item">` and set five attributes:

```html
<article class="item"
         data-threads="evidence tooling"   <!-- first one decides its group in Thread lens -->
         data-kind="role"                  <!-- role | paper | service -->
         data-start="2025.42"              <!-- decimal year, .42 ≈ June -->
         data-end="2025.67"
         data-sort="2025.42">              <!-- sort key, usually = data-start -->
```

The JS reads those attributes and handles chips, counts, span bars, sorting, and filtering.
No list to update anywhere else.

## Editing the look

Colors and type live in `:root` at the top of `style.css`, with a matching
`prefers-color-scheme: dark` block underneath. Change one, change the other.

- `--accent` (`#8c3a2b`) is the only non-neutral color on the page.
- Fonts: Newsreader for prose, IBM Plex Mono for anything that reads as metadata.
- `.shell` caps the page at 58rem. Item body text caps at 40rem for readability.

## Behavior worth preserving

- Without JS the page still renders every item, expanded, in document order. The `no-js`
  class on `<html>` handles it. Search engines and no-script readers see the full content.
- `prefers-reduced-motion` disables the re-sort animation and smooth scrolling.
- The print stylesheet expands every item and drops the controls, so `⌘P` gives a clean CV.
- Item headers are keyboard-operable (`Enter` / `Space`) and report `aria-expanded`.

## Open items

- The Arthroscopy systematic review is marked `in production`. Swap the flag for a DOI once
  it gets an issue.
- No ORCID or Google Scholar link yet. With five outputs listed, that's the highest-value
  addition to the Papers group.
