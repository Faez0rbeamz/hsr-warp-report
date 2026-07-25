# hsr-warp-report

Single-page Honkai: Star Rail warp statistics report. Static, no build step.

- **Live:** https://faez0rbeamz.github.io/hsr-warp-report/
- **Repo:** https://github.com/Faez0rbeamz/hsr-warp-report (public)
- **Pages:** deploy from branch `main`, folder `/` (root)

## Files

| | |
|---|---|
| `index.html` | The entire page — markup, CSS, inline SVG, data. Self-contained. |
| `char-*.jpg` | Character art for the favorites cards (700x1023, JPEG q84) |
| `social-preview.png` | OG card, exactly 1280x640. Re-shoot after visual changes. |

Only external dependency is Google Fonts (Anybody, Chivo, Space Mono) via CDN.

## Deploy loop

    git add -A && git commit -m "..." && git push

Pages takes **~35-45 seconds** to serve the new version. Verify before reporting
done — poll the URL for a string unique to the change rather than assuming:

    Invoke-WebRequest -Uri "https://faez0rbeamz.github.io/hsr-warp-report/" -UseBasicParsing

Git prints an LF/CRLF warning on every commit. It's harmless.

## Data integrity — important

Every figure on the page comes from the account's real warp records, which live
in the page itself (the "Full manifest" table and the timeline SVG). The
"Personal favorites" stats are derived from those same rows.

**Never invent or estimate a number.** If a new stat is needed, compute it from
the existing table. Cross-check: eidolon level + 1 should equal that character's
pull count.

## Design system

Palette is CSS custom properties in `:root`. The page was deliberately retuned
from a navy/gold scheme to a unified violet one — keep new colours in that family.

| Var | Value | Role |
|---|---|---|
| `--ink` | `#000000` | page base (true black) |
| `--panel` | `#171327` | card/panel fill |
| `--rail` | `#332B52` | borders, grid gaps |
| `--paper` | `#E9E5DA` | body text |
| `--dim` | `#9A92B4` | secondary text |
| `--brass` | `#D8A75A` | highlights, "won" |
| `--jade` | `#5FC4C0` | "guaranteed" |
| `--rust` | `#D06A8C` | "lost 50/50" |

Character accents (inline `--acc` per card): Silver Wolf `#7E7BF5`,
The Herta `#B77CE0`, Fu Xuan `#E86A80`.

**Semantic colours also appear hardcoded inside the timeline SVG.** If you change
one, change it globally or the chart desyncs from the UI.

### Layered background (all `position:fixed`, behind content)

`.bgsky` neon glows → `.bggrid` perspective grid floor → `.bghz` horizon line →
`.bgpat` emblem weave (6.5% opacity) → `.bgscan` scanlines.

`.bghz` is transparent through the middle 16% on purpose — it used to cross body
text and read as a strikethrough. Don't "fix" that gap.

### SVG symbols

Two tiers, both defined once and reused via `<use>`:

- `pt-sw` / `pt-th` / `pt-fx` — detailed multi-colour portraits (cast strip)
- `em-sw` / `em-th` / `em-fx` — simple monochrome emblems (headings, background)

Small emblems drop their rings via `--ring1`/`--ring2` custom properties, which
inherit into `<use>` shadow DOM. Detailed art turns to mud below ~30px, which is
why both tiers exist.

## Character art

Generated locally with ComfyUI. See `C:\Users\Fae Sign In\AI\IMAGE-GEN-GUIDE.md`
for the full pipeline, prompt conventions, and LoRA rules.

The three cards are composed as a **triptych**: Silver Wolf turned toward centre,
The Herta frontal, Fu Xuan turned inward from the other side. Preserve that if
regenerating any of them.

Only original or generated art is used — no official HoYoverse assets and no
third-party fan art are committed to this public repo.

## Open items

- Cast strip at the top still uses SVG portraits, not generated art (deliberate:
  detail is unreadable at 74px, but it could be swapped)
- GitHub repo "Social preview" image must be uploaded by hand in Settings —
  there is no API for it
