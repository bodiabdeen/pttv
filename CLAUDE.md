# pttv: working rules

GitHub Pages site (`https://pttv.abdeen.uk/`, served from branch `pttv/v2.0` since 18 November 2026). `index.html` is the LIVE masjid TV display (Emerald look since 25 November 2026), `index-emerald.html` an identical copy kept for old links, and `index-classic.html` the old Classic display. `iaiom.html` and `iaiom-emerald.html` are the website widget (Classic and Emerald), which the masjid website's admin copies by hand into the website.

* New work goes on a new branch. Never commit to existing branches, and never push to the branch GitHub Pages serves from without the owner's approval: it changes the screen in the masjid immediately.
* For new work, make review copies first (for example `index-v3.html`); the owner replaces the live files after reviewing. A change to how a page works must go into both its Classic and Emerald files.
* The pages read Firestore `prayerTimes/*` with the public web key. Any new data must be optional: a page must keep working if a document is missing or cannot be read.
* Every value inserted with `innerHTML` goes through `escapeHtml`.
* Keep animations gentle and keep the `prefers-reduced-motion` rule that switches them off.
* Test locally: run `python -m http.server 8002 --bind 127.0.0.1` in this folder, then open `http://127.0.0.1:8002/index.html` or `index-emerald.html` (live data, read only). Add `?emulator` to use the local Firebase emulators started from the PrayerTimesBackEnd repo.
