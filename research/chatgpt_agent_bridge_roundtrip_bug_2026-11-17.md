# ChatGPT ↔ Agent bridge — diagnosis and successful round-trip — 2026-11-17

## Current status
**CONNECTED — first full ChatGPT → Gmail notification → Agent → Cloudflare result → ChatGPT round-trip verified on 2026-11-17.** The bridge now works for the tested read-only task. It is a relay through Gmail + the existing Cloudflare Worker, not a native ChatGPT MCP connection.

## Successful end-to-end test — 2026-11-17
- The user confirmed the updated Learned App and existing Routine #2 were saved. Learned App reports `user trusts its changes`. Routine #2 is a notification routine for `com.google.android.gm|CHATGPT-AGENT`; its prompt passes the ticket explicitly as a tool argument and no longer depends on `location.hash`.
- Confirmed the Agent KV prefix was empty before issuing a new test.
- Issued read-only task ID: `bridge-test-20261117-766cfd9a7b4f`.
- Created a 48-character ephemeral ticket, wrote the task to `agent:task:<ticket>` with a 300-second TTL, and sent a Gmail wake-up to the user's own mailbox.
- Gmail send succeeded; message ID `1a1e90730d16a99a`; sender and recipient both `christianomomongan82@gmail.com`. Gmail metadata showed `UNREAD`, `SENT`, and `INBOX` labels.
- The wake email included the unique `CHATGPT-AGENT-<ticket>` marker in both subject and body. This also avoids Agent's notification deduplication behavior, which compares package/title/text over a 10-minute window.
- Cloudflare KV bulk-get returned a matching result record:
  - `receivedAt`: `2026-11-17T10:22:53.391Z`
  - `id`: `bridge-test-20261117-766cfd9a7b4f`
  - `ok`: `true`
  - `result`: `Judul: Example Domain. Jumlah H1: 0. URL akhir: https://example.com/. Tidak ada tindakan lain.`
- A follow-up bulk-get showed the associated task record status changed to `complete`, and result ID exactly matched the task ID.
- Both temporary KV keys (`agent:task:<ticket>` and `agent:result:<ticket>`) were deleted after retrieval. A final prefix listing for `agent:` returned an empty array. No ephemeral ticket is stored in GitHub.
- Direct single-key KV GET returned 404 even though the key appeared in the key listing; Cloudflare's supported POST `/bulk/get` successfully returned the values. Use bulk-get for result retrieval in this connector context.
- This test proves the round trip for a short, read-only browsing task. It does not prove write actions or long-running tasks.

## Root cause of the preceding failure
Inspected current upstream source in https://github.com/Past-da-king/agent :

- `app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt`: on a new allowed Gmail notification, the app calls `Routines.runNow(..., context_ = "A notification just arrived from ... title: text ...")`. The ticket marker in the Gmail notification is supplied as routine context.
- `app/src/main/java/com/past9/phoneaos/triggers/Routines.kt`: `RoutineWorker.runBody()` constructs the routine prompt as `inputData.context + t.prompt`. Thus the notification title/text is available to the routine's agent prompt.
- `app/src/main/java/com/past9/phoneaos/connections/WebAppTools.kt`: learned-app `EnginePageRunner.ensure()` checks whether the current browser URL is still on the learned app host. If the task has navigated to an external site, it calls `SiteAdapt.open(..., app.startUrl, ...)` to reopen the learned app at its static start URL. `app.startUrl` is `https://www.kreavo.id`, with no per-task hash.
- The previous Learned App definition obtained the ephemeral ticket from `location.hash`. Since a read-only task browses `example.com`, the browser leaves Kreavo; before a later learned-app call, Agent reopens plain `https://www.kreavo.id`, losing the URL-fragment ticket. This matched the user's prior "ticket not found" result callback failure.

## Applied fix
- Updated the existing Learned App tools to accept an explicit ephemeral `ticket` argument:
  - `get_command(ticket)` validates 40–64 URL-safe characters and GETs `/agent/command` with `X-Agent-Ticket`.
  - `send_result(ticket,id,ok,result)` validates the same ticket and POSTs the result to `/agent/result` with `X-Agent-Ticket`.
- Updated existing Routine #2 prompt to extract the ticket from notification context and pass it explicitly to both tools. Neither tool nor routine depends on `location.hash` or a ticket in the URL.
- User verified Learned App tools and routine were updated; Trust remained active. The prior existing Worker routes and ephemeral KV ticket scheme were unchanged. No Worker redeploy was needed for this fix.
- The email body and subject both now include the unique marker. Agent source shows notification de-duplication: it compares app package, notification title and text over a 10-minute interval before firing a routine. This marker placement makes each wake notification text unique too.

## Operating limits and safety
- Initial routine scope is strictly read-only: research, browsing and reading only. No sending messages, posting, purchases, deletions, account changes, or form submissions.
- Task and result KV records expire after 300 seconds; ChatGPT must retrieve short-task results promptly. Long-running or durable result delivery remains unimplemented.
- The bridge relies on Gmail notification access and the Agent routine being enabled. If it stops firing, inspect Agent Connections > Notifications and the status of Routine #2 before sending repeated tests.
- Do not mark write actions as approved by this successful read-only test. Expand scope only with explicit task-specific authorization and a separate safety review.
- Do not claim a native ChatGPT MCP connection. This is a working Gmail/Cloudflare relay.

## User authority / project boundary
The user explicitly granted the assistant full authority for this technical bridge work in the existing Cloudflare/GitHub scope. This does not authorize unrelated business, account, payment, or project changes. RapiUsaha remains the primary B2C digital-products business for UMKM. No RapiUsaha, Projects.co.id, RentalOps, SignageQuote, SiapKirim, or Backtest business decisions were changed.
