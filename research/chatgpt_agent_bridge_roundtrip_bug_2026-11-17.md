# ChatGPT ↔ Agent bridge — diagnosis of result-ticket loss — 2026-11-17

## Current status
**PARTIAL ROUND-TRIP PROVEN; RESULT CALLBACK FAILED.** The user reports that Agent received and executed the read-only task: it opened https://example.com/ and returned the page title "Example Domain", no H1, and the final URL. Agent then reported that sending the result failed because the task ticket was not found. Therefore Gmail wake-up, notification permission, routine firing, task dispatch, and at least task execution worked in this second test. The end-to-end bridge is still NOT CONNECTED because ChatGPT did not receive the result.

## Root cause confirmed from Agent source
Inspected current upstream source in https://github.com/Past-da-king/agent :

- `app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt`: on a new allowed Gmail notification, the app calls `Routines.runNow(..., context_ = "A notification just arrived from ... title: text ...")`. The ticket marker in the Gmail notification is supplied as routine context.
- `app/src/main/java/com/past9/phoneaos/triggers/Routines.kt`: `RoutineWorker.runBody()` constructs the routine prompt as `inputData.context + t.prompt`. Thus the notification title/text is available to the routine's agent prompt.
- `app/src/main/java/com/past9/phoneaos/connections/WebAppTools.kt`: learned-app `EnginePageRunner.ensure()` checks whether the current browser URL is still on the learned app host. If the task has navigated to an external site, it calls `SiteAdapt.open(..., app.startUrl, ...)` to reopen the learned app at its static start URL. `app.startUrl` here is `https://www.kreavo.id`, with no per-task hash.
- The current bridge Learned App definitions (user-confirmed) obtain the ephemeral ticket from `location.hash` (the prior config was `#agent=<ticket>`). Since a read-only task browses `example.com`, the browser leaves Kreavo; the later `send_result` tool reopens plain `https://www.kreavo.id` and its JS cannot find `location.hash`. This matches the exact reported failure and is not a Worker ticket expiry or Gmail trigger failure.

## Corrective design (no Worker redeploy required)
Update the existing Learned App tools to accept the ephemeral ticket explicitly as an argument, removing every dependency on `location.hash`:

- `get_command`: required param `ticket: string`; validate 40–64 URL-safe chars; GET `https://kreavo-bridge.usahalab-id.workers.dev/agent/command` with header `X-Agent-Ticket: args.ticket`.
- `send_result`: required params `ticket: string`, `id: string`, `ok: boolean`, `result: string`; validate ticket; POST to `https://kreavo-bridge.usahalab-id.workers.dev/agent/result` with the same header and JSON body containing id/ok/result (result max 3500 chars).
- Update the routine prompt to extract ticket from its notification context (the subject marker `CHATGPT-AGENT-` followed by the 40–64 character URL-safe ticket) and pass it explicitly to both tools. It may still open Kreavo for learned-app origin, but must not rely on browser URL state for the ticket.
- Retain strict read-only task scope. No sending, posting, buying, deleting, account changes, or form submission.
- Existing Worker routes and ephemeral KV ticket scheme remain unchanged. No change to legacy Kreavo endpoints or other Workers.

## Verification
- The current second test established the Worker-issued task got to Agent and the read-only task was executed. It did not prove successful result POST.
- No new Worker deployment has been made as part of this diagnosis.
- The current Agent app and routine already exist; the next local Agent change is only to update the existing Learned App definitions and its routine prompt, then run one short round-trip test.
- Do not queue another task or send another Gmail wake email until the tool definitions no longer depend on `location.hash`.
- Do not mark bridge connected until ChatGPT reads back the matching task ID and result from KV.

## User authority / project boundary
The user explicitly granted the assistant full authority for this technical bridge work in the existing Cloudflare/GitHub scope. This does not authorize unrelated business, account, payment, or project changes. RapiUsaha remains the primary B2C digital-products business for UMKM. No RapiUsaha, Projects.co.id, RentalOps, SignageQuote, SiapKirim, or Backtest business decisions were changed.
