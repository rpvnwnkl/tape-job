# GitHub Pages publication — 2026-09-29

Live URL: **https://rpvnwnkl.github.io/tape-job/**

Pages serves the `modern` branch at repository root (`/`) using GitHub's branch
publishing mechanism. No application build system or custom workflow was added.
The previous source was `gh-pages` at root. The API reported no custom domain,
so there was no domain or DNS configuration to change.

The original five preservation commits were pushed without rewriting history.
Initial published application revision:
`af5da5127025ee34e6ee5950052507f59c078cf4`.
Pages build `1248165366` completed successfully; its associated deployment is
https://github.com/rpvnwnkl/tape-job/actions/runs/36599700561 .
The documentation commit containing this report follows that revision and is
published through the same branch. Application files are unchanged by that
follow-up. The final deployed documentation commit is recorded in the worker's
completion report rather than inserting a self-referential commit hash here.

Remote `master` and `gh-pages` were both checked before and after publication:
`a976375897de5a73483731646420abdb10e4210c`. Neither branch was pushed, reset or
modified. Other remote branches were left alone. The reused checkout was clean
before work began; only preservation documentation and test evidence were added.

## Live checks

- Served HTML matched the preserved source by SHA-256:
  `62f5b1838e247312daeefed0085c25d387ff407b8cca4fbfb9387518226a77b9`.
- All 28 dependencies in `dependencies.json`, plus HTML, main CSS, local font
  CSS, jQuery, original video and all nine page images matched local SHA-256
  values: **42 successful asset checks**, all HTTP 200.
- A request for video bytes 1000000–1001023 returned HTTP **206**,
  `Content-Range: bytes 1000000-1001023/16836134`, `Accept-Ranges: bytes` and
  a 1024-byte body. Browser seeking and decoding succeeded with duration 66.666.
- Chromium 139.0.7230.0, isolated headless profile, viewport 1280 × 800:
  eleven forward steps, all nine lightbox images through the center control,
  backdrop dismissal, eleven backward steps and close were exercised on HTTPS.
  The known backward boundary failure and retained close position reproduced.
- The final browser run recorded no JavaScript page errors, HTTP error responses,
  mixed-content failures or third-party requests. External requests were blocked
  and logged if attempted. Arvo regular and italic and Alfa Slab One loaded
  locally. Bold was verified through asset hashing rather than visually used.
- One media request was cancelled (`net::ERR_ABORTED`) during the interaction
  run; subsequent playback, seeking and decoding succeeded without media error.
- An early browser run began while the initial modern deployment was still
  building and loaded the old HTML. Its mixed-content/CDN failures are not
  presented as modern results. A fresh run after deployment supplied the final
  evidence. The deployment was also confirmed independently through the API
  and live HTML/asset checksums.

`live-browser.json`, `live-assets.json` and `behavior-probes.json` retain the
machine-readable results in this directory. Screenshots and reusable harness
copies remain in the publication worker's outputs/work directories. The prior
local reference comparison remains in the adjacent preservation-evidence folder.

## Coverage and hosting limits

Safari 26.6.1 is installed, but its WebDriver refused a session because “Allow
remote automation” is disabled. That setting was not changed. Firefox Developer
Edition is installed, but no compatible preinstalled Firefox automation driver
was found; no browser or driver was installed. These browsers, legacy IE, mobile
and touch behavior remain unverified. Chromium results are not claims of
cross-browser equivalence or a recovered 2013 rendering.

The live HTTPS URL responds with valid certificate verification in both HTTP
and browser clients. However, GitHub's Pages API repeatedly rejected enabling
`https_enforced` with HTTP 404, “The certificate does not exist yet”. The source
change succeeded, but enforcement remains false. This is an outstanding hosting
setting, not an HTTPS access failure. A repository administrator can revisit
Settings → Pages → Enforce HTTPS when GitHub makes it available (or retry
`PUT /repos/rpvnwnkl/tape-job/pages` with `{"https_enforced":true}`). No DNS,
custom domain, account-level site or unrelated repository was changed.

No application behavior repair was included. See `BEHAVIORS.md` for source and
history evidence, confidence levels, live probes, and minimal unapplied proposals.
Historical font provenance uncertainty remains documented in `README.md`.
