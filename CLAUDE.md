# pttv: working rules

GitHub Pages site. `index.html` is the LIVE masjid TV display. `iaiom.html` is a widget the masjid website's admin copies by hand into the website.

* New work goes on a new branch. Never commit to existing branches, and never push to the branch GitHub Pages serves from without the owner's approval: it changes the screen in the masjid immediately.
* Do not edit `index.html` or `iaiom.html` for new work. Make the change in the review copies `index-v2.html` and `iaiom-v2.html`; the owner replaces the live files after reviewing.
* The pages read Firestore `prayerTimes/*` with the public web key. Any new data must be optional: a page must keep working if a document is missing or cannot be read.
* Every value inserted with `innerHTML` goes through `escapeHtml`.
* Keep animations gentle and keep the `prefers-reduced-motion` rule that switches them off.
* Test locally: run `python -m http.server 8002 --bind 127.0.0.1` in this folder, then open `http://127.0.0.1:8002/index-v2.html` (live data, read only). Add `?emulator` to use the local Firebase emulators started from the PrayerTimesBackEnd repo.
