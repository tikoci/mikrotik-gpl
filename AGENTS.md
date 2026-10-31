# AGENTS.md — maintaining mikrotik-gpl

This file is the maintainer/agent runbook for this repo. The public, visitor-facing explanation lives in [README.md](README.md) — keep the split: the README is for someone looking for the GPL sources; this file is how the repo is run. Do not move mechanics into the README.

## What the repo is

A mirror of MikroTik's GPL disclosure for RouterOS, plus the automation that requests it. The disclosure content is MikroTik's, mirrored as received — never edit files under a disclosure folder (e.g. `2025-03-19/`); those folders are evidence, append-only.

## How the request automation works

- Workflow: `.github/workflows/gpl-nag-email.yaml` ("Request GPL Source").
- Schedule: twice weekly (Wednesday and Saturday, 03:00 UTC), plus manual `workflow_dispatch` (input `mailto`, default `support@mikrotik.com,mt@mikrotik.com`).
- The check job reads the current stable version from `https://upgrade.mikrotik.com/routeros/NEWESTa7.stable` and tests whether a GitHub Release exists with that version as its tag. (A testing-channel check exists in the workflow but is commented out.)
- If no Release exists, the sendmail job emails the request — subject `Request for open source code included in "RouterOS v<version>"`, sent from the TIKOCI GitHub Organization, cc `tikoci.repos@gmail.com` — via Gmail SMTP (`dawidd6/action-send-mail`; credentials are the repo Actions secrets `MAIL_USERNAME` / `MAIL_PASSWORD`), and then creates the Release for that version.
- **The Release is the dedupe marker**: a version that has a Release is never re-requested. Deleting a Release re-arms the request for that version.

## Where replies land, and what they look like

- Replies arrive in `tikoci.repos@gmail.com` (the cc on every request) as MikroTik support tickets.
- The pattern so far is invariant: support replies with a `box.mikrotik.com` (Seafile) link to the **same 2022-01-04 disclosure** already mirrored here, plus a statement that an updated version is "being worked on", with no ETA (first said 2025-03-20).

## Verifying a reply before believing it

Do not update the mirror on a support reply alone — verify the payload:

1. Download the linked share.
2. Compare it against the repo copy. Reference check (verified 2026-10-22): the share's `linux-5.6.3.patch.gz` decompresses **byte-identical** to `2025-03-19/linux-5.6.3.patch`. A future share that matches the same way is the same old disclosure — nothing to check in; just log the ticket below.
3. The only share item not mirrored in the repo so far is `nrv2b-08-02-2002.tgz` (124 bytes, a stub).

## If a genuinely new disclosure arrives

1. Unpack it as received; do not modify its contents.
2. Check it in under a new top-level folder named by the date received, `YYYY-MM-DD` (the current one is `2025-03-19/`), and note which RouterOS version was requested.
3. Update the README status section (newest source provided, dates) and the reply log below.
4. Keep the original archive available — attach it to the requested version's Release where practical.

## Reply log

| Date | Ticket | Requested | Result |
|---|---|---|---|
| 2025-03-20 | — | RouterOS 7.18.2 | Disclosure dated 2022-01-04 received; mirrored in `2025-03-19/`. Support: a newer patch is "being worked on". |
| 2026-10-05 | SUP-225433 | RouterOS 7.24.5 | Same 2022-01-04 disclosure via box link (hash-verified identical). Updated version promised, no ETA. |
| 2026-10-25 | SUP-226773 | RouterOS 7.25 | Same kernel patch via box link. Updated version in work, no ETA. |

## House rules

- README claims stay evidence-backed: dates, ticket numbers, hashes. No speculation about MikroTik's intent.
- Never print or commit the mail secrets; they live only in repo Actions secrets.
- Keep the visitor/maintenance split: if a fact is about *how we run this*, it belongs here, not in the README.
