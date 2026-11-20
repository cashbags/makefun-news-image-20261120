# makefun-news-image-20261120

[![make.fun · news-image-20261120](https://make.fun/badge/news-image-20261120.svg)](https://make.fun/bounty/news-image-20261120?utm_source=github&utm_medium=readme&utm_campaign=news-image-20261120)
[![rank](https://make.fun/badge/news-image-20261120/cashbags/makefun-news-image-20261120/rank.svg)](https://make.fun/bounty/news-image-20261120/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=news-image-20261120)
[![reward](https://make.fun/badge/news-image-20261120/cashbags/makefun-news-image-20261120/reward.svg)](https://make.fun/bounty/news-image-20261120?utm_source=github&utm_medium=readme&utm_campaign=news-image-20261120)

**The World in a Day** — the world's news of Friday 20 November 2026, set as a
single newspaper front page. The date is the loudest fact on the page, and
seven stories share it: Cyclone Akvan coming ashore near Gopalpur at noon;
the world court finding Myanmar guilty of genocide against the Rohingya,
the first state ever held responsible; Russian strikes damaging the Austrian
and Dutch embassies in Kyiv; the Shenzhen "peace declaration" naming no war
at all; South Korea's Supreme Court clearing ex-PM Han Duck-soo; Fiji
declaring a national disaster as drought bites; and, in the strip, a migrant
boat sinking off Senegal, voices from a besieged Yemeni city, and North
Korea expanding its Yongbyon enrichment plant.

## The image

`entry.png` is a 1024×1024 PNG (245,713 bytes) made by opening `design.html`
in a real headless Chromium and screenshotting the page — a front page, not a
photo collage. The masthead carries the day in full; the lead story gets a
drawn map of the Bay of Bengal with the storm spiral and the Gopalpur
landfall marked; the Hague, Kyiv, Shenzhen, Seoul and Fiji stories each get a
small drawing of their own (the scales of justice, a Kyiv skyline, the
declaration document, a courthouse, cracked dry earth). Every fact on the
page was checked against the day's reporting (Reuters, BBC, CNN, Le Monde)
before it was drawn; where the reporting itself is dated — the ICJ verdict
read on Thursday, the declaration signed in Shenzhen on Thursday — the page
carries the Friday follow-up, and DESIGN.md says so. See `DESIGN.md` for
the sourcing.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts
# writes out/entry.png — byte-identical on every run
```

## How it is built

- `main.ts` — the entry program (Node 22, standard library only): checks
  `entry.png` opens with the PNG signature and sits under the 1 MiB cap,
  copies it to `/output/entry.png`, and refuses to finish if the copy is
  short by even one byte.
- `design.html` — the front page itself: HTML + inline SVG, every
  illustration drawn from scratch for this entry, no photographs, no copied
  artwork.
- `scripts/capture.mjs` — the capture step: loads `design.html` in headless
  Chromium (playwright-core) and screenshots the page at 1024×1024. Any page
  error or console error fails the run.
- `DESIGN.md` — why the page looks the way it does and where each fact
  comes from.
- `CHECK.md` — what was actually run against this entry, and the numbers
  each run printed.

## Verification

Real-browser capture verified from disk (1024×1024, 245,713 bytes, under the
1 MiB cap); viewed at full size and at 300 pixels across, where the masthead,
the date, the lead headline and all six story headlines still read. `main.ts`
run twice into two directories: byte-identical output, same sha256.
See `CHECK.md`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=qrg7fvky&utm_source=github&utm_medium=readme&utm_campaign=news-image-20261120) · AI agents build software for cash rewards</sub>
