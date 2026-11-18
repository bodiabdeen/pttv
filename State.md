# State: pttv

Branch `pttv/v1.6`, created on 18 November 2026 from `pttv/v1.5`. Nothing has been pushed; the live TV page and the website widget are unchanged.

## Done on this branch

### `index-v2.html` (review copy of the TV display)

* **Special prayers** from the new `prayerTimes/specialPrayers` document appear in the Special Prayers list ("Today 13:30", "Tomorrow 02:00", or a date), count down as the next event on their date, and get the event screen at their time. They update live (Firestore listener) when the admin adds or deletes one. If the document is missing or unreadable, the page behaves exactly like the old one.
* **Layout**: daily prayers never shrink. If special prayers do not all fit, the admin-added ones furthest in the future are left off the TV (the app and website still list them). Long names shrink and are limited to two lines.
* **Reliability**: no more hourly full-page reload. Data refreshes every 30 minutes and just after midnight; if a refresh fails, the screen keeps what it shows. Today's last good data is saved in the browser, so a start-up without network shows it with a small "Offline" marker. One reload a day at 03:00, only when online.
* **Bug fixes**:
  * The highlighted next prayer is re-checked every minute (it used to stay stale until the hourly reload).
  * The event badge says PRAYER TIME at the start time and ADHAN between adhan and iqama (it said NOW PRAYING for everything).
  * English text no longer uses the Arabic font: the old selector `*[class*="ar"]` matched any class containing "ar" (card, large, bar). Arabic uses Arabic Typesetting where installed, otherwise Noto Naskh Arabic from Google Fonts.
* **Security**: every Firestore value is HTML-escaped before display.
* **Gentle animations**: cards fade up on first load, the next prayer's border breathes slowly, the countdown takes a warm tint in its last five minutes, the normal and event screens crossfade, and a newly added special prayer fades in. All are switched off when the device asks for reduced motion.

### `iaiom-v2.html` (review copy of the website widget)

* Special prayers shown under Special Prayers while visible.
* The next prayer is highlighted, with the same slow breathing border.
* Refreshes every 30 minutes and just after midnight, so a page left open stays correct.
* HTML escaping, the same gentle entrance animation, and the reduced-motion switch.

Both pages: `?emulator` on localhost uses the local Firebase emulators and shows an "EMULATOR" badge.

## Tested (18 November 2026)

* End to end in headless Edge against the local emulators: special prayers appear, update live, hide when they should, injected markup is shown as text, fonts and highlight are correct, no script errors.
* Against live data (read only): both v2 pages load with no errors and show the same special prayers as the live pages, because no admin-created ones exist yet.
* Screenshots checked at 1920 by 1080 and 1366 by 768.

## To publish (owner)

1. TV: copy `index-v2.html` over `index.html` on the branch Pages serves from (or first open `/index-v2.html` on the TV after merging, to try it on the real screen).
2. Website: send `iaiom-v2.html` to the website's admin to replace the current widget code.

## Open

* The website widget's Hijri date comes from Aladhan, while the TV computes it in the browser (improvement plan C4).
* Related branches: app `iom/v1.6`, backend `iombe/v1.6`.
