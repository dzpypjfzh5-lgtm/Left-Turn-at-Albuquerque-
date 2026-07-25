# Left Turn at Albuquerque

A cartoon word game. One self-contained `index.html` — no build step, no
dependencies, no network calls. Open it, or host it anywhere static.

Two files: `index.html` is the whole game, and `apple-touch-icon.png` is the
Home Screen icon. Keep them side by side when hosting.

## Categories

Seven of them, 30 answers each — 210 words, six at every length from 4 to 8.

Descriptions follow one pattern: who or what it's about, in Title Case, two
subjects joined by `&`, a single subject left bare.

| Category | What it's about |
| --- | --- |
| 🛸 Q36 | Marvin |
| 💥 Happiness of Pursuit | Road Runner & Wile E. Coyote |
| 🥕 Pismo Beach | Bugs & Daffy |
| 🤠 Boba Fettucine | Star Wars & Spaghetti Westerns |
| 🎺 Slanky | Mike Doughty & Soul Coughing |
| 🎭 Carefully Taut | Rodgers and Hammerstein & Hamilton |
| 🚀 Infinity and Beyond | Pixar |

Within Pixar the focus is Toy Story, Monsters Inc, A Bug's Life and Finding
Nemo; within Boba Fettucine, Boba Fett and Sergio Leone.

### Adding another one

Append an entry to `CATEGORIES` and a matching array to `WORDS` under the same
`id`. That's it — nothing else is hardcoded:

- The available word lengths, the height of the guess-distribution chart and the
  column counts of both pill rows are all derived from the data at load.
- **Saved progress is never touched.** Stats and found words are keyed by
  category id, so existing categories keep their numbers and the new one starts
  empty. Stats for an id that is no longer listed are *kept* rather than
  deleted, so a category can be pulled and restored without losing its history.
- A start-up audit logs `console.warn` for the mistakes that are easy to make:
  a category with no word list, a word repeated inside a category or shared
  between two, anything that isn't plain A–Z, and categories whose totals don't
  match. It never blocks play.
- A category listed without a word list is skipped rather than crashing the
  page, and its absence is one of the warnings above.

## How it plays

- **Word lengths vary.** The board resizes to the answer, and longer words earn
  more guesses: 6 tries for 4–5 letters, 7 for 6–7, 8 for 8.
- **Pin a length, or take them at random.** The picker at the bottom of the
  category sheet — reachable by tapping the length chip under the title — pins
  answers to one length, or leaves it on *Any*. A pinned length shows a red dot
  on the chip, and stays pinned even once you've cleared every word at it. A
  length a category has no words at is greyed out.
- **Any word of the right length is a legal guess.** The answers are mostly
  proper nouns, so a dictionary would reject more than it caught.
- **Nothing repeats until you've cleared the set.** Each new word is drawn from
  the ones you haven't found yet; the last word of a category earns a
  "That's all, folks!"
- **Both endings tell you where you stand.** A win floods the screen with props;
  a loss shows the answer and a *Next Word* button. Either way there's a way on
  from the panel rather than a dead board.
- **Stats are global *and* per category.** The score card's top row switches
  between *All* and any one category, each with its own played / win % /
  current & max streak / guess distribution. A category keeps its own streak
  across switching away and back.
- **Progress persists** in `localStorage` — found words per category, both tiers
  of stats, the pinned length, and your light/dark choice. Serve it over
  `http(s)` rather than opening the file directly: browsers restrict storage for
  `file://` pages, so a hosted copy is what makes the scoreboard stick. If
  storage is unavailable the game says so up front instead of quietly losing
  your streak.

## Notes

- **Add to Home Screen** gives it a proper app icon and a full-screen, no-browser-chrome
  launch. Safari won't accept a `data:` URI for the touch icon, which is why the
  PNG is the one companion file rather than being inlined like everything else.
- Phone-first. Tile size is computed from the answer length, the number of
  tries and the live viewport (`dvh`), so a 4×6 board and an 8×8 board both sit
  above the keyboard without JavaScript measuring anything. Safe-area insets are
  respected, so the notch and home indicator stay clear.
- The display face is [Luckiest Guy](https://fonts.google.com/specimen/Luckiest+Guy)
  (Apache License 2.0), embedded as base64 so the game works offline. Body text
  uses the device's own UI font.
- Every graphic is hand-written inline SVG. An affectionate homage; no
  copyrighted assets, no money changing hands.
