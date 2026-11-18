# State: pttv

Branch `pttv/v2.0`, created on 18 November 2026 from `pttv/v1.5` (it was called `pttv/v1.6` until the owner made this release 2.0 everywhere). GitHub Pages serves `https://pttv.abdeen.uk/` from this branch from 18 November 2026; before that it served `pttv/v1.5`, whose last Pages build (August 2026) had failed.

Files: `index.html` is the TV (Classic look), `index-emerald.html` the TV in the Emerald look, `iaiom.html` the website widget (Classic) and `iaiom-emerald.html` the widget in Emerald. Each file carries `<meta name="version" content="2.0">`. A change to how a page works must be made in both its Classic and Emerald files. `.nojekyll` makes Pages serve the files as they are.

## Done on this branch

### `index.html` and `index-emerald.html` (TV display)

* **Special prayers** from the new `prayerTimes/specialPrayers` document appear in the Special Prayers list ("Today 13:30", "Tomorrow 02:00", or a date), count down as the next event on their date, and get the event screen at their time. They update live (Firestore listener) when the admin adds or deletes one. If the document is missing or unreadable, the page behaves exactly like the old one.
* **Layout**: daily prayers never shrink. If special prayers do not all fit, the admin-added ones furthest in the future are left off the TV (the app and website still list them). Long names shrink and are limited to two lines.
* **Reliability**: no more hourly full-page reload. Data refreshes every 30 minutes and just after midnight; if a refresh fails, the screen keeps what it shows. Today's last good data is saved in the browser, so a start-up without network shows it with a small "Offline" marker. One reload a day at 03:00, only when online.
* **Bug fixes**:
  * The highlighted next prayer is re-checked every minute (it used to stay stale until the hourly reload).
  * The event badge says PRAYER TIME at the start time and ADHAN between adhan and iqama (it said NOW PRAYING for everything).
  * English text no longer uses the Arabic font: the old selector `*[class*="ar"]` matched any class containing "ar" (card, large, bar). Arabic uses Arabic Typesetting where installed, otherwise Noto Naskh Arabic from Google Fonts.
* **Security**: every Firestore value is HTML-escaped before display.
* **Gentle animations**: cards fade up on first load, the next prayer's border breathes slowly, the countdown takes a warm tint in its last five minutes, the normal and event screens crossfade, and a newly added special prayer fades in. All are switched off when the device asks for reduced motion.
* **Tomorrow's times**: once the day's last event has passed (normally Isha's iqama), the list switches to tomorrow's times under the amber title "Tomorrow · Thu 19 Nov", and the countdown runs to tomorrow's first event. Just after midnight it goes back to "Daily Prayers". If tomorrow's times cannot be loaded, the page behaves as before.

### `iaiom.html` and `iaiom-emerald.html` (website widget)

* Special prayers shown under Special Prayers while visible.
* The next prayer is highlighted, with the same slow breathing border.
* Refreshes every 30 minutes and just after midnight, so a page left open stays correct.
* HTML escaping, the same gentle entrance animation, and the reduced-motion switch.
* **Today | Tomorrow switch** beside the Daily Prayers title. Tomorrow shows the next day's times under "Tomorrow · Thu 19 Nov", without the next prayer highlight. The page returns to Today just after midnight. The switch stays hidden if tomorrow's times cannot be loaded.

### Emerald look (owner decision, 18 November 2026)

* Published as separate files so the Classic pages stay as they are. Same data, same behaviour; only the look changes.
* TV: deep teal background, one row per prayer with Begins, Adhan and Iqama labelled once, and the next event inside a ring that fills from the previous event to the next. On short screens (under 820 pixels high) the ring is smaller and the settings strip is hidden.
* Website: teal header with a small ring showing the next prayer and the time left, then one row per prayer.

All pages: `?emulator` on localhost uses the local Firebase emulators and shows an "EMULATOR" badge.

## Tested (18 November 2026)

* End to end in headless Edge against the local emulators: special prayers appear, update live, hide when they should, injected markup is shown as text, fonts and highlight are correct, no script errors.
* Against live data (read only): both v2 pages load with no errors and show the same special prayers as the live pages, because no admin-created ones exist yet.
* Screenshots checked at 1920 by 1080 and 1366 by 768.
* Emerald pages (live data, read only): no script errors; the ring matches the time between events (13:30 is 19.5% of the way from Dhuhr's iqama at 13:25 to Asr at 13:51); nothing overflows at 1920 by 1080 or 1366 by 768; the widget works at phone width.
* Tomorrow, with the page clock moved (emulator and live data): at 15:00 the TV shows today; at 21:00 it shows "Tomorrow · Thu 19 Nov" with Thursday's times (Fajr 05:44, Maghrib 16:14) and counts down to Fajr. The widget's switch shows Thursday's times and switches back to exactly the same Today list. No script errors.

## Published (18 November 2026)

* GitHub Pages now serves this branch. The TV keeps opening `https://pttv.abdeen.uk/` (Classic); to use Emerald on the TV, open `https://pttv.abdeen.uk/index-emerald.html` instead.
* Website: the site's admin copies `iaiom.html` (Classic) or `iaiom-emerald.html` (Emerald) into the site.
* To go back: in the repository's Pages settings, choose the branch `pttv/v1.5` again.

## Open

* The website widget's Hijri date comes from Aladhan, while the TV computes it in the browser (improvement plan C4).
* Related branches: app `iom/release/v2.0`, backend `iombe/v2.0`, Wear OS watch `v2.0`.
