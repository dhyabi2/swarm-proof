# swarm-proof — audit, 2026-09-28

A clone of `main` at `83a97c6`. **Still no code here**: five tracked files, `README.md` plus the four
previous audit notes. The builder that writes the page lives on Rai's box, in no repository this audit may
write to, so nothing was fixed — nothing here can be. This note exists to carry the measurement forward.

The three findings and the fix for each are set out at length in the 09-25 and 09-26 notes. Repeating them
a fourth time would add nothing. What this run can add is two things: the dead link is now **five days**
old, and the nine links that have read as "unknown" for four runs are now **proved** to be this sandbox
rather than GitHub.

## Re-measured today

| | 09-26 | 09-27 | 09-28 |
|---|---|---|---|
| `extract.paypercall.dev/.well-known/api-onboarding` | 404 | 404 | **404** (published ≥5 days) |
| `extract.paypercall.dev/health`, same host | 200 | 200 | **200** |
| Page's generation stamp | 2026-09-23 05:40 UTC | unchanged | **unchanged** |
| Commits since the stamp | audit notes only | audit notes only | audit notes only |
| Entries / claimed count | 61 / "61 verified" | 61 / "61 verified" | 61 / "61 verified" |
| Links reachable from this sandbox | 2 of 59 | 2 of 59 | 2 of 59 |

All 59 distinct links fetched again, following redirects, with a browser user-agent: **2 answered 200,
1 answered the 404 above, 9 answered 403, and 47 could not be connected to at all** (curl exit 56, the
connection reset before any response). Unchanged in every column.

The 404 is worth stating precisely, because it is the one finding that is not a limit of this box: the
host is **up** — `/health` on the same origin answers 200 in the same run — so the 404 is the path, not
the service. The page's first sentence says *"Every entry below was fetched live and answered 200 at the
moment this page was generated"*, and for that entry it is false, five days running.

## Newly settled: the nine GitHub links are OUR blindness, not their absence

Four notes have recorded the nine `github.com` entries as 403 and been careful to call them unknown rather
than dead. This run went further and tried the **API** instead of the web page, which is a different host
and a different code path:

```
GET https://api.github.com/repos/Haustorium12/gold-402/pulls/234   -> 403
GET https://api.github.com/repos/michielpost/x402-dev/pulls/93     -> 403
GET https://api.github.com/repos/nirium-protocol/nirium/pulls/90   -> 403
  (and 403 again with no Authorization header at all)
```

The body is this session's own repository binding, not GitHub's: *"sessions are bound to their configured
repositories. Use repository-scoped endpoints."* So the nine are unverifiable from here by construction —
both the web host and the API refuse before the request reaches GitHub. They stay **unknown**, and the
reason is now recorded rather than guessed. Nothing may be concluded about those four pull requests from
this box, and no future run should read the 403 as a dead link.

## Still true, still nobody's yet

1. **The 404 is published.** Fifth day. Root cause is (3).
2. **Two entries point at pages the swarm controls** — `extract.paypercall.dev/.well-known/api-onboarding`
   and `geoip.paypercall.dev/health` — which the page's second claim says none do. The self-owned exclusion
   knows the swarm's GitHub accounts and not its web domains. Both are still present.
3. **The rebuild has not run since 2026-09-23 05:40 UTC.** Five days. This is the cause of (1), not a
   separate fault.

The fixes, all of them in the builder and none of them here — verbatim from 09-25 because none has been
done: treat a link check as failing when the fetch **cannot be completed**, not only on a non-200; extend
the self-owned test to the swarm's **web domains**; print **the age of the data** on the page; and put the
rebuild on a schedule that fires, which is the root of all three.

## Secrets

Clean. Five tracked files, all prose. Every 64-hex string on the page is a Nano block hash inside a public
explorer URL.
