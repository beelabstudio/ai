# Credits & Attribution

This skill is a **wrapper** around two upstream open-source works. Neither the scraping engine
nor the original kit was written by Bee Lab Studio — full credit goes to their authors.

## Upstream kit (vendored here)

- **Project:** google-maps-scraper-kit
- **Author:** **Mahan** ([@mahanjafari-dev](https://github.com/mahanjafari-dev))
- **Repository:** https://github.com/Mahanaicoach/google-maps-scraper-kit
- **License:** MIT License — Copyright (c) 2026 Mahan
- **What was vendored:** `SKILL.md`, `scripts/`, `docker-compose.yml`, `.env.example`, `examples/`,
  the four `.claude/commands/*.md` slash commands (kept under `commands/` here), and the permission
  snippet from `.claude/settings.json` (kept as `settings.snippet.json`). `README.md`, `SETUP.md`,
  `CLAUDE.md`, `assets/banner.svg`, and the repo's own `.gitignore` were **not** vendored — they're
  project-scaffolding for a standalone kit repo and don't apply to how skills are consumed here (see
  `SKILL.md` for the equivalent setup instructions, adapted for this catalog).
- **Deviations from upstream (found by running the vendored skill end-to-end before merging it —
  real container, real curl/`scrape.py`/`scrape.sh` runs, real Google Maps results, 2026-09-21):**
  1. `docker-compose.yml` pins `v1.18.1` instead of upstream's `v1.15.0`. `v1.15.0`'s Playwright
     dependency fetches its browser driver from Azure CDN mirrors
     (`playwright[-akamai/-verizon].azureedge.net`) that now 404 on every driver build we tried —
     confirmed from the host directly too, so it's a dead upstream endpoint, not this-machine-specific.
     `fast_mode:true` jobs are unaffected (no browser needed), but `fast_mode:false` — this skill's own
     documented default — hangs at `Status: working` forever and never transitions to `failed`.
     `v1.18.1` (latest available at test time) downloads its browser cleanly and, as a bonus, ships a
     native `linux/arm64` manifest, so Apple Silicon no longer needs QEMU emulation either.
  2. `scripts/scrape.sh` wraps every `"${AUTH[@]}"` as `"${AUTH[@]+"${AUTH[@]}"}"`. Upstream's version
     crashes immediately with `AUTH[@]: unbound variable` on macOS's default `/bin/bash` (3.2, which
     Apple has shipped unchanged since 2007) whenever `SCRAPER_API_KEY` is unset — i.e. every default
     run, since this kit needs no key for localhost. Bash ≥4.4 doesn't have this bug; 3.2 is still what
     `env bash` resolves to on an unmodified Mac.

  Re-verify both against current upstream before assuming they're still necessary — upstream may fix
  either independently.

## Upstream scraping engine (not vendored — pulled as a Docker image at runtime)

- **Project:** google-maps-scraper
- **Author:** **Georgios Komninos** ([@gosom](https://github.com/gosom))
- **Repository:** https://github.com/gosom/google-maps-scraper
- **License:** MIT License — Copyright (c) 2023 Georgios Komninos
- **Docker image used:** `gosom/google-maps-scraper:v1.18.1` (see `docker-compose.yml`; upstream kit
  pins `v1.15.0` — see "Deviations from upstream" above for why this vendor tracks a newer tag)

This skill's `docker-compose.yml` pulls and runs the official published image. It does **not** modify
or redistribute the upstream source — all scraping capability and field extraction come from the
`gosom/google-maps-scraper` project itself.

## Upstream licenses (reproduced as required by MIT)

### Kit license — Mahan, 2026

```
MIT License

Copyright (c) 2026 Mahan (github.com/mahanjafari-dev)

This license covers the "Google Maps Scraper Kit" wrapper (Docker Compose setup,
scripts, documentation, and Claude skill) ONLY. The underlying scraping engine,
"google-maps-scraper", is a separate work by Georgios Komninos, licensed under the
MIT License (Copyright (c) 2023 Georgios Komninos) — see below.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

### Engine license — Georgios Komninos, 2023

```
MIT License

Copyright (c) 2023 Georgios Komninos

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

If you find this useful, please **star both upstream repos**:
https://github.com/Mahanaicoach/google-maps-scraper-kit and
https://github.com/gosom/google-maps-scraper
