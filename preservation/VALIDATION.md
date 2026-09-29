# Validation — 2026-09-29

Tested with installed Chromium 139.0.7230.0 using an isolated headless profile,
a 1280 × 800 viewport, and a local static server mounted at `/tape-job/` with
byte-range support. The test harness and screenshots are retained outside the
repository in the adjacent `preservation-evidence/` directory. This repository
does not require that harness or its browser-testing packages to run.

## Observed results

- Fresh historical checkout without submodule initialization: FitText returned
  404 and the page raised `$(...).fitText is not a function`. The video loaded
  with duration 66.666 seconds. The baseline screenshot therefore represents
  the broken fresh checkout, not a fully functioning historical rendering.
- Preserved checkout: no JavaScript page errors, no HTTP error responses,
  **zero external requests**, with an external-request blocking rule active
  throughout the interaction run. FitText loaded as a function. Arvo regular,
  Arvo italic and Alfa Slab One loaded locally; the unused bold face was
  verified by its file checksum, not exercised visually.
- Eleven forward clicks reached authored stops 2.1, 5.5, 8, 10.35, 13, 15.75,
  18.5, 21.6, 25, 29 and 33 seconds. Actual pauses followed shortly after each
  stop, as expected from the existing `timeupdate` implementation.
- All nine lightboxes opened through the center click target, loaded the
  corresponding `img/page1.jpg` through `img/page9.jpg`, and closed through
  backdrop clicks. Original image dimensions were confirmed after loading.
- Eleven backward clicks exercised stops 36.5, 41, 45, 48, 50.5, 53.25, 56,
  58, 61.5, 64 and then 64 again. The last behavior is a pre-existing failure,
  not evidence of intentional design or a reliable reset to the beginning.
- The close control hid the video and retained its current position.
- A historical-reference run served the original HTML and main CSS directly
  from Git, with the test harness supplying the same pinned dependencies in
  response to their old remote URLs. It reproduced the stop sequence, all
  nine lightboxes, and close behavior. The landing page and all nine settled
  lightbox screenshots were pixel-equal at the same viewport and fixed video
  times. Initial lightbox captures caught fade transitions; the final visual
  pass waited for full opacity and used identical seek positions in both runs.
  Results are in `preservation-evidence/visual/comparison.json`.
  This isolates dependency-reference
  changes; it does not establish pixel equality to a captured 2013 browser.
- One video request was recorded as `net::ERR_ABORTED` in each run. Subsequent
  decoding, seeking and playback succeeded with no media error. This request
  cancellation is reported rather than concealed as an entirely empty log.
- All 28 downloaded files matched their recorded SHA-256 checksums. All
  historical regular files except the two dependency-reference files remain
  byte-identical; authored inline scripts also match exactly. There are no
  remaining gitlinks, and no submodule was initialized.
- The full diff whitespace check reports only trailing spaces in unmodified
  upstream font FONTLOG/OFL files. Those bytes are deliberately retained to
  preserve the documented checksums; authored changes pass the whitespace check.

## Preserved issues and limits

Escape did not dismiss the first lightbox in the initial test. The historical
markup uses `tab-index`, and its keyboard handler depends on focus; this is a
possible historical focus issue, not repaired in this batch. Backdrop dismissal
worked for all nine lightboxes.

The close handler still assigns `currentTime` to a jQuery wrapper, not the
video element. The observed retained position is consistent with that code.
The last backward click near 64 seconds retains a previous stop value. These
behaviors reproduced with the historical source and remain unchanged.

At this initial local-validation stage, no Safari, Firefox, legacy IE,
touch-device, production HTTPS, or live GitHub Pages test had been performed.
The subsequent live publication checks are recorded in `PUBLICATION.md`, and
source/history investigation and repair proposals in `BEHAVIORS.md`. The chosen viewport is a repeatable comparison size,
not a claim about the author's original monitor. Font provenance limits and
dormant legacy dependencies are documented in README.md. Additional browser
coverage and any proposed navigation/focus repairs belong in a separate review.
