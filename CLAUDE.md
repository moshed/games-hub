# Games hub — games.dancykier.com

A one-page static hub linking the three browser games. No build step, no JS —
everything is in `index.html`.

## Structure
- `index.html` — the whole page: header, three game cards, footer. Self-contained
  (inline CSS, inline SVG favicon as a data URI, no external requests).
- `CNAME` — `games.dancykier.com`.

## The three games it links
| Game | URL | Repo |
|---|---|---|
| Spelling Bee | spellingbee.dancykier.com | `moshed/spellingbee` |
| Anagrams | anagrams.dancykier.com | `moshed/anagrams` |
| Pinpoint | geo.dancykier.com | `moshed/pinpoint-geo` |

Each card is a plain `<a>` — if a game moves, edit the `href` and the card text.

## Design
Deliberately matches the Spelling Bee family: yellow `#f7da21` accent, flat-top
hexagon logo mark (same `clip-path` polygon the game uses for its tiles), 16px
rounded cards, subtle radial glow behind the page. Each game gets its own icon
colour (bee yellow / anagrams blue / pinpoint green) so the cards read apart at a
glance. Light and dark are both supported via `prefers-color-scheme`, with a
`[data-theme]` override hook if a toggle is ever added.

Uses `100svh` and `env(safe-area-inset-*)` so it sits correctly under the iOS
notch / home indicator — same treatment as Spelling Bee.

## Hosting
- GitHub Pages, repo `moshed/games-hub`, served from `main` at root.
- Custom domain `games.dancykier.com` — Namecheap CNAME → `moshed.github.io.`.
- **DNS landmine:** `namecheap.domains.dns.setHosts` REPLACES the entire record
  set and does NOT include MX. Any change must `getHosts` first, re-send every
  record, and pass `EmailType=OX` — omit it and Moshe's email breaks. Verify with
  `dig +short MX dancykier.com` (must return mx1/mx2.privateemail.com) afterwards.
- **TLS cert:** GitHub often leaves the cert at `state: none` on first setup. Fix
  is to clear the cname, re-set it, then POST a new build — same quirk hit while
  setting up Pinpoint. Enforce HTTPS afterwards:
  `gh api -X PUT repos/moshed/games-hub/pages -F https_enforced=true`

Deploy = edit `index.html`, commit, push. Pages rebuilds in ~30–60s.
