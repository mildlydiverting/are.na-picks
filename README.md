# arena-picks

A local browser tool that pulls a random item from each of up to four [Are.na](https://www.are.na) boards and displays them side by side. Shuffle to get a new set, lock cards you want to keep, and save the whole thing as a portable zip.

![Screenshot showing four blocks and the basic interface](example-output/Screenshot%202026-03-19%20at%2016.56.30.png)

## Files

```
arena-picks.html       — the app
arena-config.js        — your personal settings (not committed to git)
arena-config.example.js — template to copy from
```

## Setup

**1. Get an Are.na personal access token**
Go to [are.na/settings/personal-access-tokens](https://www.are.na/settings/personal-access-tokens) and create one. Read scope is all you need.

**2. Create your config file**
Copy `arena-config.example.js` to `arena-config.js` and fill in your token and board slugs.

The slug is the last segment of the board URL. For `are.na/kim/drawing-references` the slug is `drawing-references` — just that part, no username prefix.

```js
const ARENA_CONFIG = {
  token: "your-token-here",
  boards: [
    "your-first-board-slug",
    "your-second-board-slug",
    "your-third-board-slug",
    "your-fourth-board-slug",
  ],
  allowedTypes: new Set(["Image", "Text", "Link", "Embed", "Attachment"]),
  maxRetries: 8,
};
```

**3. Open arena-picks.html in a browser**
Double-click it, or drag it onto a browser window. No server or build step needed.

## Usage

- **Shuffle ↻** — loads a new random item for each unlocked card
- **Lock** — hover a card to reveal the lock icon (top right of the image). Click to lock; locked cards are skipped on the next shuffle
- **Save ↓** — downloads a zip containing `index.md` (YAML frontmatter and one section per card) plus any images that could be fetched

## Saved zip structure

```
arena-picks-2026-03-19-14-32.zip
  index.md
  images/
    block-12345.jpg
    block-67890.png
    ...
```

The markdown includes the board name, Are.na block link, source URL (for links), and inline image references using relative paths.

## Customising

All colours are CSS custom properties at the top of the stylesheet in `arena-picks.html` — easy to retheme without touching anything else. The app follows your macOS light/dark setting automatically; the toggle in the header overrides it manually.

To show only certain block types, edit `allowedTypes` in `arena-config.js`.

## Dependencies

No build tools or package manager. One CDN library: [JSZip 3.10.1](https://stuk.github.io/jszip/) (used only for the save feature, loaded from jsDelivr).

## Notes

- Are.na's signed CDN image URLs expire after roughly 24 hours, so saved zip images are a point-in-time snapshot — worth keeping if you want them
- `arena-config.js` is in `.gitignore` to protect your token; use `arena-config.example.js` as a template for sharing or new installs
