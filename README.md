# London Flat Hunt (fork)

A fork of [mikepapadim/london-property-hunt-public](https://github.com/mikepapadim/london-property-hunt-public), reworked for **whole-flat rentals** (default: 2-bed) instead of rooms and studios.

Twice a day, a scheduled Claude task uses Claude in Chrome to check Rightmove, Zoopla and OnTheMarket for new listings. It throws out duplicates and flats above shops, ranks what's left, appends it to a Google Sheet you already use, and sends you a short phone-friendly summary with ready-to-send enquiries.

## How it differs from upstream

- Whole flats only. The bedroom count, budget, areas and allowed postcodes all come from config.
- Rightmove, Zoopla and OnTheMarket. SpareRoom is dropped (rooms only), and OpenRent because it shows a human-verification page to automated browsing. Note that OnTheMarket's robots.txt disallows automated search; it's included by choice and can be removed in config.
- Writes to a **Google Sheet** through the Sheets connector, appending rows in your existing column order, instead of a local `.xlsx`.
- **No email sending.** Upstream had Chrome open Gmail and click Send. Here the run's summary arrives as the scheduled-task notification.
- Dedups by listing ID **and** by street + postcode, so the same flat on two portals, or an older row without a URL, isn't added twice.
- Hardening for unattended runs:
  - Listing text is treated as untrusted (prompt injection).
  - It never logs in, never submits forms, and never contacts agents.
  - It stops on a site that shows a CAPTCHA rather than trying to get past it.
  - It caps the number of listing pages it opens per run.

## Files

- `skill.md` — the prompt Claude runs
- `config.example.md` — config template. Put your real config in the scheduled task prompt, not in this public repo.
- `tracker/README.md` — what the sheet needs

## Running it

Create a scheduled task (needs your computer on, with Chrome and the Claude extension) whose prompt is:

> Clone https://github.com/0770exe/london-property-hunt-public, read skill.md, and run it with this config: <your config>

## Caveats

- Property portals' terms generally restrict automated access. This runs in your own browser at a low rate (two runs a day, about 25 listing pages a run), but use it at your own discretion.
- It reads only the first results page per area, so it relies on running often enough to catch new listings.

Original concept and the case study are in [`case-study.md`](case-study.md) (upstream's numbers, not this fork's).
