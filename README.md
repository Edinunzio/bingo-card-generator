# Bingo Card Generator

A single-file, static **Bingo Card Generator** for cozy community game nights.  
Built with **Tailwind CSS (CDN)**, **vanilla JavaScript**, and **html2canvas** — no backend, no storage.

## Features

- **Keyword pool editor**
  - Add phrases one at a time or with `;`-separated input.
  - Live list of keywords as removable chips.
  - Case-insensitive deduplication with inline duplicate warnings.
  - Live counter: **X / 50 keywords added**.
  - Enforces **25–50 unique keywords**:
    - Under 25 → cannot generate (gray/neutral status).
    - 25–50 → ready to generate (green/positive status).
  - Input is disabled once 50 keywords are added.

- **Bingo card generation**
  - Click **Generate Bingo Card** to:
    - Shuffle the keyword pool.
    - Pick **24 unique keywords**.
    - Fill a **5×5** grid with the center as a distinct **FREE SPACE**.
  - Each card is stored as a plain JS object:
    ```js
    {
      id,          // unique string id
      createdAt,   // Date instance
      grid,        // 5x5 array of strings
      keywords,    // 24 keywords actually used on this card
    }
    ```
  - In-memory history of up to **10 cards per session**.
  - After the 10th card:
    - A banner warns that you’ve hit the limit and that refreshing clears everything.
    - The **Generate** button is disabled.

- **Card rendering**
  - Prominent display of the **most recently generated card**.
  - Classic `B I N G O` header row using **Playfair Display**.
  - FREE SPACE cell has a distinct amber background and bold text.
  - Card label shows **Card #** and a human-readable timestamp.
  - Text inside cells automatically uses smaller font sizes for longer phrases.

- **Download as PNG**
  - Uses **html2canvas** to capture the rendered card.
  - Downloads as a PNG with filename:
    - `bingo-card-{id}.png`

- **Session card list**
  - Scrollable panel listing all cards in the current session.
  - Each item shows:
    - Card number.
    - Timestamp.
    - **View** button to load that card into the main display.
    - A toggle to show/hide the 24 keywords used on that card.

- **Design & layout**
  - Clean, slightly playful “community game night” aesthetic.
  - **Two-column layout on desktop**, stacked on mobile:
    - Left/top: keyword management.
    - Right/bottom: current card + history.
  - Warm off‑white background, deep green accents, amber FREE SPACE.
  - Rounded corners, soft shadows, and smooth hover/focus transitions.

## Tech Stack

- **HTML**: single `index.html` file.
- **CSS**: [Tailwind CSS CDN](https://cdn.tailwindcss.com) (no build step).
- **Fonts**: Google Fonts – `Playfair Display` + `DM Sans`.
- **JavaScript**: plain browser JS (no frameworks or bundlers).
- **Image capture**: [html2canvas](https://html2canvas.hertzen.com/) via CDN.
- **State**: all in-memory JS; no localStorage, cookies, or network calls.

## Running the App

Because it’s a fully static page, you have options:

### Option 1: Open directly from the filesystem

1. Clone or download this repository.
2. Open `index.html` in a modern browser (Chrome, Firefox, Edge, Safari).
   - On macOS you can usually double‑click `index.html` in Finder.

Everything should work via the `file://` URL — no server required.

### Option 2: Serve with a simple static server (optional)

If you prefer an `http://` URL (for example, for dev tooling), you can run any static file server you like and point it at this directory (e.g. `python -m http.server`, `npx serve`, or Dockerized equivalents).  
This is **optional**; the app does not require a backend.

## Deploying to GitHub Pages

Since this is a single static file, GitHub Pages setup is straightforward:

1. Push this project to a GitHub repository.
2. In your repo, go to **Settings → Pages**.
3. Under **Source**, select:
   - **Branch**: `main` (or your default branch)
   - **Folder**: `/ (root)`
4. Save.

GitHub Pages will serve `index.html` from the root.  
Your app will be available at a URL like:

- `https://<your-username>.github.io/<your-repo-name>/`

## Behavior Notes & Edge Cases

- **No persistence**: Refreshing the page **resets everything** (keywords, cards, history).
- **Keyword pool vs cards**:
  - If you remove or change keywords after generating cards, **existing cards are unaffected**.
  - New cards will be generated from the current pool only.
- **Session limit**:
  - Hard cap of 10 cards per browser tab session.
  - When the limit is reached, a banner appears and the Generate button is disabled.
- **Accessibility**:
  - Semantic elements (`button`, `section`, `header`, etc.).
  - `aria-live` regions for error messages and the session limit banner.
  - Tailwind focus styles preserved for keyboard users.

## Development Notes

- All behavior lives in `index.html` (inline `<script>`).
- There are no build tools, package managers, or external dependencies beyond the CDNs.
- If you extend the project (e.g., presets, printing layouts, alternative card sizes), consider:
  - Keeping the state object shape consistent.
  - Adding small, focused render/update functions to maintain clarity.

## License

MIT or your preferred open-source license (update this section as needed).

