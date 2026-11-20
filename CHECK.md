# CHECK.md — "The World in a Day" (news-image-20261120)

What was actually run against this entry, and what each run said. All of it
was run in this session, against the committed repo, not assumed from the
build: the numbers below are the ones the runs printed.

## The repo and the kit

- Repo `cashbags/makefun-news-image-20261120`, public, created
  2026-11-20T01:01:41Z — after the competition opened (2026-11-20T00:00:00Z).
  License MIT, SPDX-License-Identifier line on top (copied from the
  news-image-20261119 LICENSE, which passed license_osi). README carries the
  three make.fun badges and the referral `qrg7fvky` (the account's own code
  is `6sknhk5z`, used ONLY for bring-the-humans surfaces, never here — user
  decision stands). About + topics set (editorial, makefun, news, news-image)
  — topics via the dedicated set-topics endpoint, since PATCH alone leaves
  them empty.
- Kit downloaded fresh from the live API and unpacked at
  `../kit/news-image-20261120-kit` (bundle sha256 `63afd6ef…` per kit.json);
  the starter's `main.ts` was read, not copied — this entry's `main.ts` is
  written fresh in this session with its own structure (PNG signature check
  plus 1 MiB cap plus byte-length proof, wrapped in a handOverImage helper
  with a named abort path).
- Competition verified live before building: $250, entryFeeUsd null (free),
  acceptingSubmissions true, 1,014 submissions at check time (842 when the
  task arrived), deadline 2026-11-22T00:00:00Z (07:00 WIB Sat 22 Nov),
  published 2026-11-20T00:00:00Z. No escrow field in the API response
  (escrow: null) — the submit step decides whether a signed entry is needed;
  the keypair is on hand if so.
- Local HEAD `527d9ef8` (1 commit); GitHub HEAD
  `04040e8feb2bc5e7c855078a2698c63fb91c9327` (confirmed by get-ref). The local
  commit is NOT in the repo — files went up via the Contents API, one commit
  per file — but the trees are identical: all 7 blob SHAs match local
  `git hash-object` exactly (LICENSE 06e1da51…, README.md 99e5067c…,
  DESIGN.md 4e37e527…, design.html 0719c0ee…, entry.png 0004992d…,
  main.ts b63bd7f8…, scripts/capture.mjs 63f3253b…).
  SUBMIT THE GITHUB HEAD, not the local commit.

## The image: real browser capture, verified from disk

- `node scripts/capture.mjs` (playwright-core, headless Chromium, resolved
  from `~/workspace/makefun/bounties/node_modules`) — first capture
  245,734 bytes; two SVG defects found by viewing the capture (PEACE
  DECLARATION overflowing its document box; OVERTURNED colliding with the
  courthouse columns) and fixed in design.html; re-captured.
- Final `entry.png`: 1024×1024, 245,713 bytes, under the 1 MiB cap, sha256
  `ef194f1587c2960967e5dce012f6d261755c0685d727fa669ad39596ad1b71b2`.
  Viewed at full size (masthead, date, lead headline, all six story
  headlines and the strip read; both SVG defects gone) and at 300 px across
  (masthead, date band, lead headline and story headlines still read).
- `main.ts` run twice into two directories: byte-identical output, same
  sha256 `ef194f15…` as entry.png itself.

## Kit check (no transcripts — this session, 2026-11-20 ~01:03 WIB)

- Command: `node cli/makefun.mjs check --kit <kit> --repo <entry>`
  (no --transcripts: no bundle could be written from this session, see
  Transcripts below).
- Result: **4 passed, 1 failed, 14 skipped, 5 at submission** —
  `not ready: fix transcripts_present, then check again`.
- Passes: competition_open (now between opening 2026-11-20T00:00:00Z and
  deadline 2026-11-22T00:00:00Z), snapshot (7 files, 275,328 bytes, within
  the intake's caps), license_osi (SPDX-License-Identifier: MIT,
  OSI-approved), entrypoint (main.ts, node22).
- The one failure, transcripts_present, is structural to this subagent run
  and expected: bundle refuses to write while the parent-chain link is
  missing (see Transcripts). Nothing needs rebuilding.
- NOTE on the follow-up spec's "Entry tree ≤8192 bytes" line: that cap does
  not apply to this competition. The kit's own check reports the snapshot
  "within the intake's caps" (each file at most 100 MiB, repository at most
  512 MiB), the competition RULES state no 8,192-byte tree cap (that cap
  belonged to tiny-chess), and the news-17/18/19 entries passed the same
  check at ~275–284 KB. The tree was NOT shrunk to 8,192 bytes: doing so
  would mean dropping entry.png, which is the entry itself.

## Transcripts — bundle blocked at the parent-chain rule, parent must re-bundle

- Trial bundle attempted in this session with
  `--session <this session file> --no-family --no-discover --harness muse-app
  --harness-version 2ade5b007ae0 --provider ipnext
  --model ipnext/avocado-5.17-v2 --effort default --mcp none
  --prompt-file prompt-user.txt --prompt-origin handed-off --prompt-via
  "parent agent handoff carrying the user's own opening instruction"
  --redact "Damai Darmawan" --redact "darmawandamai@gmail.com"`.
  Dry run found: 1 session, 38 turns, 130 lines kept (19 earlier records
  from before the repository was first named left out); instruction files
  collected (AGENTS.md); MCP none. Prompt recorded correctly:
  "Makefun kompetisi baru langsung join" (handed-off).
- First dry run (system-prompt = withheld stub): bundle ALSO refused the
  system prompt — a one-line "withheld" stub holds no system prompt for
  entries received from 18 Nov 2026 (12:00 UTC). Second dry run with
  `--system-prompt ~/workspace/system/system_prompt.md`: "system prompt:
  uploaded". Use that file at re-bundle, NOT the withheld stub.
- Bundle still writes NOTHING, verbatim: "missing: the parent chats: its
  prompt is marked handed-off, so the upload must carry its parent chats,
  every session of the chain that started the run up to the one whose opening
  is the person's prompt, but no chain it carries reaches that one:
  sessions/muse-app/7e2da640-ed3f-4245-9f97-4c02d92b1add.jsonl carries its
  parent chat at depth 1, and the one at depth 1
  (context-sessions/muse-app/sessions/muse-app/f6a7d2cb-b279-4e66-b580-1ecea551982d.jsonl)
  has no opening message". The parent session is a scheduler.cron-spawned
  orchestrator session (agent f6a7d2cb-b279-4e66-b580-1ecea551982d), not a
  main-chat session with the user's typed opening.
- Fix (parent, after this run): run bundle from the top-level agent once
  this run completes, with `--session <builder file> --session <parent file>
  --no-family --no-discover --muse-usage
  ~/.makefun/muse-usage-news-image-20261120.jsonl` and the same
  harness/prompt/system-prompt flags (system prompt =
  `~/workspace/system/system_prompt.md`), after SELECTing usage rows from
  muse.db for agent_ids `7e2da640-ed3f-4245-9f97-4c02d92b1add` (builder)
  and `f6a7d2cb-b279-4e66-b580-1ecea551982d` (parent) and appending them to
  `~/.makefun/muse-usage-news-image-20261120.jsonl` — the parent row is
  already in that file (1 row: parent, 654,256 in / 9,648 out); the builder
  row lands in muse.db only after this run completes, so it must be
  SELECTed and appended by the parent. Then re-run the kit check with
  --transcripts. NOTHING needs rebuilding; the entry, repo and image are
  final.
- Prompt file `prompt-user.txt` holds ONLY the user's verbatim words:
  "Makefun kompetisi baru langsung join". No prompt file with words the user
  did not say was written.

## News sourcing

Seven stories plus three strip items, each checked against the day's
reporting before it was drawn (see DESIGN.md for the per-story sourcing):
Cyclone Akvan landfall near Gopalpur about noon, 90–100 km/h gusting 110,
1,012 emergency teams, schools shut in six districts; ICJ finds Myanmar
guilty of genocide against the Rohingya 9–6, first state ever held
responsible, reparations ordered (verdict read Thu 19 Nov, reactions Fri);
Russian strikes damage Austrian and Dutch embassies in Kyiv; the Shenzhen
"peace declaration" names no war — no Ukraine, Iran or North Korea — Macron
pushes back, Trump press conference Friday; Seoul court overturns Han
Duck-soo's conviction; Fiji declares a national disaster as drought bites;
strip: migrant boat sinks off Senegal (dozens missing), voices from a
besieged Yemeni city, IAEA says North Korea is expanding Yongbyon
enrichment. Cut: US–Venezuela deal (signed Wed) and the Van Gogh $190.4m
sale (Wed night) — both yesterday's news.
