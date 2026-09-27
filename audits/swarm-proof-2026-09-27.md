# swarm-proof — audit, 2026-09-27

A clone of `main` at `cdcfd82`. **Still no code here**: the tree is `README.md` plus the three previous
audit notes, and the builder that writes the page lives on Rai's box, in no repository this audit may
write to. Nothing was fixed because nothing here can be.

This note is short on purpose. The 09-25 and 09-26 notes set out the three findings and the fix for each
at length; repeating them a third time would add nothing. What a fourth run can add is the measurement,
and the measurement is that **nothing has moved, and the dead link is now four days old.**

## Re-measured today

| | 09-26 | 09-27 |
|---|---|---|
| `extract.paypercall.dev/.well-known/api-onboarding` | 404 | **404** (published ≥4 days) |
| `extract.paypercall.dev/health`, same host | 200 | 200 |
| Page's generation stamp | 2026-09-23 05:40 UTC | **unchanged** |
| Commits since the stamp | audit notes only | audit notes only |
| Entries / claimed count | 61 / "61 verified" | 61 / "61 verified" |
| Links reachable from this sandbox | 2 of 59 | 2 of 59 |

All 59 distinct links were fetched again: 2 answered 200, 1 answered the 404 above, 9 answered 403 (that
is this sandbox's egress proxy on `github.com`, **not** GitHub — recorded as unknown, never as dead), and
47 could not be connected to at all. So the page's own first claim — *"Every entry below was fetched live
and answered 200 at the moment this page was generated"* — is still false for one entry, and 47 of the
59 still cannot be re-checked from here at all. That is the honest limit of this box, unchanged across
three runs.

## Still true, still nobody's yet

1. **The 404 is published.** Fourth day.
2. **Two entries point at pages the swarm controls** (`extract.paypercall.dev`, `geoip.paypercall.dev`),
   which the page's third claim says none do. The self-owned exclusion knows the swarm's GitHub accounts
   and not its web domains.
3. **The rebuild has not run since 2026-09-23.** This is the cause of (1), not a separate fault.

The fixes, all of them in the builder and none of them here — verbatim from 09-25 and 09-26 because none
has been done: treat a link check as failing when the fetch **cannot be completed**, not only on a
non-200; extend the self-owned test to the swarm's **web domains**; print **the age of the data** on the
page; and put the rebuild on a schedule that fires, which is the root of all three.

## Secrets

Clean. Four tracked files, all prose. Every 64-hex string on the page is a Nano block hash inside a
public explorer URL.
