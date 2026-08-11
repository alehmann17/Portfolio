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

## Photos

One per thread, sitting at the top of its card. Drop them beside `index.html`:

```
portrait-evidence.jpg
portrait-tooling.jpg
portrait-teams.jpg
```

They render at 4:3 (16:9 on narrow screens), cropped from center, so keep the subject away from
the edges. Around 1200px wide is plenty. Keep the three consistent in distance and light or the
row reads as a scrapbook.

Each sits under a grayscale and duotone wash that clears when you hover or select its card.

**All three load or none do.** If any file is missing, the whole photo band drops and the three
cards go back to plain text together. Two photos and a gap looks broken; three text cards looks
deliberate. So if the photos vanish, one filename is wrong — check all three before assuming
it's the code.

The band height comes from `padding-bottom: 75%` with the image absolutely positioned inside.
That keeps all three bands identical no matter what the source images measure. Don't swap it
back to `aspect-ratio` with an in-flow image; that lets a stray file change one card's height.

The `alt` attributes are empty on purpose — each card already carries its name and description
as real text, so a screen reader would otherwise hear the same thing twice. If a photo carries
information the caption doesn't, describe it there.

## Email

The address never appears in the served HTML. It's split across two `data-` attributes and
reassembled at runtime, which defeats scrapers reading the raw source. With JS off it degrades
to `aclehman31 at yahoo dot com`, which a person can still read and retype.

To change it, edit `data-u` and `data-d` on the `.mail` link and the visible fallback text
beside them.

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
