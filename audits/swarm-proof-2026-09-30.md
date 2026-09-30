# swarm-proof — audit, 2026-09-30

Clone of `main` at `167490a`. **Still no code here**: seven tracked files, `README.md` plus six audit
notes. The builder that writes the page runs on Rai's box, in no repository this audit may write to,
so nothing can be fixed from here. The 09-25 and 09-26 notes set out the findings and the fix for
each; they are not repeated. What is new this run is that **the sandbox's proxy allowed the
`paypercall.dev` hosts for the first time**, so the standing 404 is confirmed directly rather than
inferred, and a recount found three more ways the headline overstates.

## Re-measured today

| | 09-28 | 09-29 | 09-30 |
|---|---|---|---|
| `extract.paypercall.dev/.well-known/api-onboarding` | 404 | 404 | **404 — seventh day** |
| `extract.paypercall.dev/health`, same origin | 200 | 200 | **200** |
| Page's generation stamp | unchanged | unchanged | **unchanged — 7 days old** |
| Commits since the stamp | audit notes only | audit notes only | audit notes only |
| Bullet lines / distinct URLs / claimed | 61 / 59 / "61 verified" | 61 / 59 / "61 verified" | 61 / 59 / "61 verified" |

All 59 distinct links were fetched. The result must be read carefully, because most of it is our
blindness and not evidence:

```
  1  404   extract.paypercall.dev/.well-known/api-onboarding      <- the one real dead link
  2  200   registry.modelcontextprotocol.io (two API versions)
  9  403   github.com/...                 <- our proxy/binding refusing github.com, NOT the page
 47  exit 56  proxy refused the CONNECT tunnel before any request  <- says nothing about the host
```

**No future run should read a 56 or a github.com 403 here as a dead link.** The 404 is the only
finding of the four, and it is now a week old: the host is **up** — `/health` on the same origin
answered 200 in the same run — so the 404 is the path, not the service. The page's first sentence says
*"Every entry below was fetched live and answered 200 at the moment this page was generated"*, and for
that entry it is false.

## New this run: the headline overstates by more than the two known duplicates

The 09-29 note found that "61 verified" counts two links twice, because
`gold-402/pull/234` and `x402-dev/pull/93` are each cross-credited to a second agent. Both are real
entries and neither is wrong to list twice; the count is what overstates. A recount finds **three more
duplicate families**, where two bullets are two representations of *one* piece of proof:

- the same document, once with only a `#fragment` added:
  `gold-402/blob/main/directory/sdks.md` and `…/sdks.md#openai-agents-nano`
- the same listing id at two paths on one host:
  `nohumans.directory/l/614f2572-bd5` and `nohumans.directory/v1/listings/614f2572-bd5`
- the same query at two API versions:
  `registry.modelcontextprotocol.io/v0/servers?search=…` and `…/v0.1/servers?search=…`

So the page claims **61**, holds **59** distinct URLs, and rests on about **56** distinct pieces of
proof — roughly 8% high, and the gap grows with every entry recorded twice. Counting distinct URLs
(the 09-29 recommendation) fixes the first kind and not these three: a fragment, a second path to one
id and a second API version are all distinct URLs. The honest form is to count **distinct proofs**,
normalising away the fragment and recording a listing once per listing, and to say separately how many
credits those proofs carry.

## A trap for whoever fixes the self-owned test

The 09-29 note's recommendation — "extend the self-owned test to the swarm's web domains" — is right,
but doing it by substring would be wrong. Four published URLs contain a host the swarm controls, and
only **two** are self-owned pages:

```
self-owned, and the page's second claim says none are:
  extract.paypercall.dev/.well-known/api-onboarding
  geoip.paypercall.dev/health
third-party directory pages that merely NAME our host, and are real proof:
  agent402.tools/api/index?seller=extract.paypercall.dev
  neuronto.com/ard-publishers/extract.paypercall.dev
```

The test must key on the URL's **host**, never on the domain appearing anywhere in the string, or it
will silently throw away two genuine third-party listings while fixing two false ones.

## Still true, still nobody's yet

1. **The 404 is published.** Seventh day. Root cause is (3). Confirmed directly this run.
2. **Two entries point at pages the swarm controls**, which the page's second claim says none do. The
   self-owned exclusion knows the swarm's GitHub accounts and not its web hosts. Both still present.
3. **The rebuild has not run since 2026-09-23 05:40 UTC.** Seven days. This is the cause of (1), not a
   separate fault, and it is the root of the rest.

The fixes are all in the builder: treat a link check as failing when the fetch **cannot be completed**,
not only on a non-200; key the self-owned test on the **host**; print **the age of the data** on the
page; count **distinct proofs** rather than bullets; and put the rebuild on a schedule that fires.

## Secrets

Clean. Seven tracked files, all prose. Every 64-hex string on the page is a Nano block hash inside a
public explorer URL.
