# Busy Work / Tape Job preservation

Preservation began in 2026 on `modern`, based on historical commit
`a976375897de5a73483731646420abdb10e4210c` (2015-09-09).
`master` and the existing `gh-pages` history remain untouched. This batch is
local only: nothing was pushed, published, or configured for deployment.

The artwork and its website are both preserved objects. No redesign, library
upgrade, formatting sweep, media conversion, or navigation repair is included.

## Dependency interventions

- Replaced the FitText gitlink and `.gitmodules` entry with ordinary files at
  the same `js/FitText.js/jquery.fittext.js` path. FitText 1.2 is copied exactly
  from `davatron5000/FitText.js` revision
  `07374d7ade72e7c340a267c6285f96be57435792`. Its source retains Dave Rupert's
  copyright and WTFPL declaration; the pinned upstream contains no separate
  license file. The upstream README is included.
- Pointed the page directly at its existing `js/vendor/jquery-1.9.1.js`.
  This file is byte-identical to `https://code.jquery.com/jquery-1.9.1.js`;
  SHA-256 is `7bd80d06c01c0340c1b9159b9b4a197db882ca18cbac8e9b9aa025e68f998d40`.
- Copied the exact CSS served by
  `https://code.jquery.com/ui/1.10.3/themes/smoothness/jquery-ui.css`, all
  fourteen referenced image files, and the jQuery UI 1.10.3 MIT license.
  Relative image paths and stylesheet bytes are unchanged. The CSS SHA-256 is
  `9c286c1a80773a8c752ffc323aec348776f86ab242a4e58636b87f376e0853b1`.
- Localized Arvo normal 400/700 and italic 400, plus Alfa Slab One normal 400,
  as unmodified TTFs with their OFL licenses, FONTLOGs and upstream metadata.
  `css/fonts.css` preserves the requested families, weights and styles.
  It replaces both active Google Fonts imports and the IE-only font links.

`dependencies.json` records exact retrieval URLs, revisions and SHA-256 hashes
for every downloaded dependency file. Downloaded files were verified against
that manifest. Locally authored CSS and license notes are not upstream copies.

## Font provenance and its limits

The font source is `google/fonts` revision
`283c560b1112e6b7eeb04c3661fbc9c561a6e65c` (2015-08-25), before this site's
historical tip. This is the last Arvo path revision before that tip. Its history
includes `35244154c985150ae1ff2850c9bd4a1b3200ed23`, explicitly reverting
Regular/Bold to the production files, and
`7d6402317f034601120fa2f7ae476d1316adfa8f`, correcting italic vertical metrics.
Alfa Slab One is unchanged from the Google Fonts initial import
`90abd17b4f97671435798b6147b698aa9087612f` (2015-03-06).

This establishes a historical, pre-tip source; it does not prove which font
binaries Google's user-agent-dependent endpoint delivered in 2013. No captured
2013 font response was available. These are documented 2015 preservation
choices, not a claim of exact original CDN-byte recovery. Upstream Arvo metadata
also describes BoldItalic; that unused face is intentionally not downloaded.

## Existing libraries and authored behavior

All pre-existing regular files except `index.html` and `css/main.css` remain
byte-identical to the historical tip. All authored inline scripts, timing
values, original images and video are unchanged.

Comparisons against upstream found:

- `bootstrap.js` matches the Bootstrap v2.3.0 release exactly. The minified
  file differs only in its opening version/license comment.
- `jquery-ui.js` has a 2013-06-12 build header and reordered modules relative
  to the 2013-05-03 CDN build. A comparison disregarding whitespace and
  function-block order found matching content. This is not a semantic proof;
  the local build is retained unchanged.
- `video.js` matches the 4.0.4 CDN copy apart from an appended analytics
  trailer present only in that CDN copy. Do not replace the local file with
  the CDN copy. The CDN request for `video.dev.js` returned 403, so that
  development file's upstream equivalence remains unverified.
- Bootstrap Lightbox, Modernizr/Respond, the resize helper, other unused
  libraries and old QuickTime exports have not been exhaustively matched to
  upstream revisions. Their existing bytes are preserved.

The page uses its native HTML video element for navigation, without a Video.js
player initialization. The dormant Video.js Flash fallback still contains its
historical CDN URL. Legacy `flipflop/` QuickTime exports and commented links
are retained; this batch does not restore obsolete plugin playback.

## Running and reviewing

Serve this directory with an ordinary static HTTP server. No build, package
installation, or submodule initialization is required. For a project-site
test, mount the directory at `/tape-job/`. Use a server that supports byte-range
requests for video seeking. Do not infer GitHub Pages deployment from local
tests; no deployment was performed.

See `VALIDATION.md` for observed results and remaining behavior questions.
