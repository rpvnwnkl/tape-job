# Behavior investigation — 2026-09-29

This investigation changes no application behavior. Pre-existing behavior is
provenance, not evidence that a failure was intended. References below use
`index.html` at `af5da5127025ee34e6ee5950052507f59c078cf4` (unchanged by publication).

## Close retains the position and can continue playing invisibly

**Likely defect; high confidence for the failed reset.** `index.html:189–194`
explicitly assigns zero to `$('#tag').currentTime`, a temporary jQuery wrapper,
instead of the HTMLVideoElement. The attempt to assign zero is positive evidence
for reset intent. It arrived in `ab68ddef60a6fdac736b8908d3a42d4d5db61857`
(2013-06-16, “a whole lotta stuff. video nav works on desktop browser”) and the
handler has not subsequently changed. No commit comment establishes intentional
position retention.

Live reproduction: close while paused at 8.1 retained 8.1. Close while playing
at 8.125 left the video playing and hidden; 0.5 seconds later it was at 8.628.
There is no pause call, so it can continue to the existing stop target.

Minimal reset correction: use `tag.currentTime = 0` (or `$('#tag')[0].currentTime
= 0`). This fixes the assignment but alone does not stop playback. A coherent
stop-and-reset proposal is `tag.pause(); tag.currentTime = 0; theStopTime = 2.1;`
in the existing close handler. That would reopen at the start and prevent hidden
playback. Reset intent is supported; whether close should also pause, and what
closing the introductory overlay should do, are author-intent questions not
settled by this handler. No proposal is applied.

## Backward navigation stalls near 64 seconds

**Likely missing boundary condition; high confidence.** `index.html:447–454`
ends its decision chain at `time < 63.75`, assigning stop 64. At or above that
boundary it calls play with the old target. The `marker()` handler
(`index.html:458–463`) pauses again as soon as current time exceeds it. There
is no supported authored loop at this boundary: no seek or new target is set.

The omission originated in `ab68dde`; `591d1b7` only reindented this part of the
chain. Positive evidence for an omitted final segment exists in the dormant
“22nd pause” branch (`index.html:547–550`), which names stop 67, and the active
“back to beginning” branch (`index.html:361–365`), which seeks to 64 with stop
67. The media ends at 66.666, so 67 is a deliberate beyond-end stop target in
that active path; changing media duration or authored timestamps is unnecessary.

Live reproduction: at 64.1 with previous stop 64, backward paused at 64.355 and
retained stop 64. Backward from 2.2 reached the native media end at 66.666 with
stop 67. Forward from that end sought to 0.1 and paused at 2.217 with stop 2.1,
as explicitly implemented by `index.html:252–255`. Thus an end-to-start forward
transition is supported, but a blanket last-page-to-first loop is not. Forward
from 33.1 also retained stop 33 and paused at 33.364; whether that final-page
control should stop, wrap, or be disabled is unresolved and outside these repairs.

Minimal proposal: add the omitted terminal backward condition after the stop-64
branch, using the existing `time < 66.75` / stop-67 values. This lets the closing
segment finish. Behavior for another backward click after native end requires
an explicit design choice; do not invent wrapping. No navigation values change
in this publishing batch.

## Escape does not dismiss a lightbox

**Likely markup defect; high confidence.** All nine dialogs use `tab-index`
instead of `tabindex` (`index.html:121–161`). The loaded lightbox explicitly
sets `keyboard: true` (`js/vendor/bootstrap-lightbox.js:221–225`), registers an
Escape keyup handler on the dialog itself (113–127), and tries to focus it after
showing (75–79). The invalid attribute leaves the plain div unfocusable, so
focus stays on the opening link and keyboard events do not reach that handler.
There is no application override that disables keyboard dismissal.

The markup arrived in `ab68dde`. The plugin was introduced in `3ee07b2` on
2013-06-10 and has never changed in this repository; its custom-looking modal
copies are part of the imported file, not evidence of a later local decision
to disable Escape. Its exact upstream provenance remains unverified, but the
loaded code is sufficient evidence of enabled keyboard behavior.

Live reproduction: keyboard option true, focus on `javascript:litterbox()`
link, Escape left the lightbox open. A temporary browser-only `tabindex="-1"`
and focus assignment made Escape close it. No file was edited for this test.

Minimal proposal: correct `tab-index` to `tabindex` on all nine containers,
then test focus acquisition after the fade, Escape on every dialog, backdrop
clicks and keyboard focus restoration to the opening control. No library upgrade
or replacement key handler is needed. The existing programmatic opening path
may need an explicit return-focus improvement; that is a separate accessibility
choice, not silently included here.
