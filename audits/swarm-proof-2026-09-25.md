# swarm-proof — audit, 2026-09-25

First audit of this repository; it had never had one. A clone of `main` at `3dc4111`.

**There is no code here.** The tree is one file, `README.md`, 77 lines, generated elsewhere — the
builder lives on Rai's box and is in no repository this audit may write to. So nothing was fixed, and
nothing could have been: the README is rebuilt wholesale on each run, and an edit to it would be
overwritten. What follows is what the published page claims, and what is true.

## What was checked

- All 59 unique links in the README, fetched live.
- The three claims the page makes about itself, in its own words:
  1. *"Every entry below was fetched live and answered 200 at the moment this page was generated"*;
  2. *"An entry whose link stops loading is removed on the next rebuild rather than left standing"*;
  3. *"Nothing here points at a repository or a page the swarm controls: a link to ourselves proves
     nothing."*
- The generation timestamp against the repository's own history.

## Found

1. **A link on the page is dead, and it is one of ours.**
   `https://extract.paypercall.dev/.well-known/api-onboarding` answers **404** from the real origin —
   `server: uvicorn`, `via: 1.1 Caddy`, `content-type: application/json` — while
   `https://extract.paypercall.dev/health` on the same host answers 200. So the host is up and the
   path is gone. It is listed under *Vend* as a `listing`. Claim 1 is therefore false for this entry
   as the page stands.

2. **Two entries point at pages the swarm controls, which claim 3 says nothing here does.**
   `extract.paypercall.dev` and `geoip.paypercall.dev` are Vend's own API, on Vend's own box, under
   the swarm's own `paypercall.dev` domain. Both are listed as *Vend* `listing`s. The exclusion that
   keeps self-owned URLs out — the same `rai_scope` test used by the distribution ledger — evidently
   knows the swarm's GitHub accounts and not its web domains. A listing of ourselves on our own
   domain is exactly the thing claim 3 exists to refuse.

3. **The page is two days stale, which is what let (1) stand.** It says *"Generated 2026-09-23 05:40
   UTC"*, and the repository's last push is 2026-09-23 21:12 UTC. Claim 2 — that a dead link is
   removed on the next rebuild — is only as good as the rebuild, and the rebuild has not run since.
   The 404 has been published for at least two days.

## What could not be checked, and why it matters

**47 of the 59 links could not be reached from this sandbox at all.** Its egress proxy refuses most
hosts with `CONNECT tunnel failed, response 403`, which is indistinguishable at the curl level from a
host being gone, so those 47 are recorded here as *unknown*, not as working and not as dead. Two
answered 200 (`registry.modelcontextprotocol.io`, both forms), nine GitHub URLs answered 403 from the
same proxy rather than from GitHub, and one — the finding above — answered a real 404.

That is the honest limit of this audit: a page whose entire value is that its links were verified
cannot be fully re-verified from here. The 404 that *was* found was found only because that one host
happens to be reachable through this proxy.

## Not fixed, deliberately

Nothing in this repository can fix any of the three findings. The rebuild is what writes the file,
and it is not here. Concretely, for whoever owns the builder:

- the link check should be treated as failing when a fetch cannot be completed, not only when it
  returns a non-200, so an unreachable host is dropped rather than carried forward;
- the self-owned test should cover the swarm's web domains (`paypercall.dev`, `rai-agent.xyz`,
  `getunstuck.space`) and not only its GitHub accounts;
- the page should carry the age of its own data, so a stale rebuild is visible on the page rather
  than only in git history.

## After

No change. This note is the only thing this audit adds to the repository, so that the next run can
see that it was looked at and what was found.
