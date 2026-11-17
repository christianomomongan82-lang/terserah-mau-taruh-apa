# ACTIVE_DECISIONS.md — Single Source of Truth

Last reconciled: 2026-11-18

This file is the authoritative current decision ledger for the ChatGPT ↔ Android Agent / Instagram objective. Read this file before using any project history, research note, chat summary, or assistant-generated checkpoint. Historical files are evidence, not current instructions.

## Project identity and goal

- **USER-ASKED GOAL:** The user asked whether ChatGPT can take over/manage their Instagram through the installed Android app Agent. The practical objective is to make ChatGPT orchestrate Instagram work through Agent without requiring the user to switch apps or relay Agent's responses.
- **USER_CONFIRMED operating model:** Agent is the Android-side executor. ChatGPT owns task dispatch, task-scoped monitoring, result retrieval, evaluation, diagnosis, and follow-up decisions. No 24/7 monitoring is required.
- **Infrastructure is not the end goal:** the ChatGPT↔Agent bridge exists only to enable the Instagram objective.
- **OPEN:** exact Instagram account identity, login/session availability, and the full scope of tasks the user wants delegated.

## Decision ledger

| Item | Status | Current rule |
|---|---|---|
| Android Agent app (com.past9.phoneaos) | USER_CONFIRMED | Keep Agent as phone-side executor. |
| ChatGPT orchestration | USER_CONFIRMED | ChatGPT dispatches, retrieves, evaluates, diagnoses, and chooses follow-up. |
| Task-scoped monitoring | USER_CONFIRMED | Check while a task is running; no permanent 24/7 watcher requirement. |
| Instagram management through Agent | USER-ASKED GOAL | Establish capability; start read-only. The question is not blanket authorization for public actions. |
| MacroDroid | REJECTED | Never recommend, configure, or revive it as a wake adapter. |
| ntfy wake adapter | REJECTED/ARCHIVED | Do not substitute it for the existing Agent-native route. |
| Agent-native Learned App / notification routine | VERIFIED FOR TASK-SCOPED ROUND-TRIP | Existing Gmail notification wake → Agent claim → Cloudflare result return succeeded in the live Instagram read-only test recorded below. This is a relay, not native direct MCP. |
| Current live Agent pickup | VERIFIED FOR ONE LIVE TASK | Agent claimed and completed the Instagram read-only task on attempt 1; this does not prove every future task or persistent background monitoring. |
| Instagram login/session | OPEN / NOT VERIFIED | Do not claim account access until verified by a live read-only task. |
| RapiUsaha business direction | USER_CONFIRMED | Remains B2C digital products for UMKM; no business-model pivot. |

## Permission boundaries

- Read-only inspection may be used to verify account identity, session, profile, and visible content once the bridge is working.
- Posting, commenting, sending DMs, following/unfollowing, deleting, purchases, and account/profile/security changes require explicit approval for the specific action and target/content.
- Never request or store Instagram password, 2FA code, recovery code, or Agent per-install secret in GitHub or chat.

## Mandatory preflight for every continuation

1. Read CHATGPT_WORKING_RULES.md, PROJECT_CONTROL.md, and this file from the default branch.
2. Read only the technical source/checkpoint needed for the next action. Treat it as historical evidence unless this ledger explicitly marks the decision active.
3. Extract the current goal, USER_CONFIRMED decisions, REJECTED paths, OPEN items, and permission boundary before planning.
4. Compare the planned response/action against this ledger. If it conflicts, stop and correct it before replying.
5. Check live queue/device/session state before dispatching. Never duplicate pending tasks or infer live operation from historical successes.
6. After important work, update this ledger and verify by fetching it again.

## Known technical truth at this checkpoint

- Task-scoped Agent↔Cloudflare↔ChatGPT dispatch and result retrieval have been verified in multiple live runs on 2026-11-18, including a successful browser read and a reported tool-availability failure. This proves the relay can deliver and return a task result when the phone-side routine fires; it does not prove a native direct MCP connection or continuous background monitoring.
- Instagram browser page is reachable but signed out. Native Instagram app session and authenticated account access remain unverified.
- No direct connected Android device-control channel was available in the last recorded check. Recheck current tools before asserting the same blocker.
- Continue through the existing Agent-native Learned App / notification-routine path. Do not introduce another wake app or adapter without the user's explicit approval.

## Conflict resolution

This ledger supersedes conflicting assistant-generated proposals and historical notes. A new assistant-authored document or commit cannot change a USER_CONFIRMED decision. Only an explicit later user instruction can change the user's decision. When the user changes a decision, update this ledger first, then reconcile affected project records.

## Latest execution checkpoint — 2026-11-18

- User-approved read-only Agent browser test was queued: task ID `agent-chatgpt-web-readonly-20261118-01`. Scope: open `https://chatgpt.com/` in Agent's own browser and inspect only page metadata/headings; no sign-in, conversation reading, messages, clicks, or account changes.
- Cloudflare KV preflight immediately before dispatch found no broker pending or active task. One 48-character ticket was created and stored only in KV; it is intentionally not recorded here.
- The task metadata and `agent:broker:pending` command were written and read back successfully. One Gmail wake email was sent to the user's existing configured mailbox; Gmail returned message ID `1a1eb2d5d73b09ee`.
- Last readback after dispatch: exact command remained in `agent:broker:pending`; `agent:broker:active`, matching result, and matching status were absent. Therefore Agent had **not yet claimed or completed** the task at that check. The Gmail message remained UNREAD; that alone does not establish why the routine has not claimed it.
- No duplicate wake/task was sent. Do not dispatch another task until this task is resolved or expires and queue state is rechecked. Do not claim the ChatGPT Web test succeeded.
- Remote Desktop Commander still reports no connected devices. No phone-side settings or Agent routines were changed.

## Result checkpoint — Agent ChatGPT Web read-only test — 2026-11-18

- Task `agent-chatgpt-web-readonly-20261118-01` completed successfully on attempt 1. Broker readback at 2026-11-17T20:28:09Z showed pending and active keys absent and a stored result/status marked complete.
- Agent opened and read `https://chatgpt.com/` using its own browser. Reported title: `ChatGPT: Chat, Work, Create & Code with AI`; visible headings included `Dapatkan respons yang disesuaikan dengan Anda` and `Sedang mengerjakan apa?`; visible sign-in buttons and login-oriented text indicate the page was a signed-out/login screen.
- No sign-in, conversation opening/reading, message sending, clicks, or account changes were performed.
- Conclusion: Agent's browser navigation and read-only page inspection work end-to-end through the current bridge. This does **not** verify an authenticated ChatGPT session, Instagram login/session, or Instagram management capability.
- Next useful step: use the same Agent browser path for a narrowly scoped, read-only inspection of the Instagram login/session landing state; do not log in or change account state. Check broker pending/active before dispatching to avoid duplicates.
- Monitoring clarification: ChatGPT can inspect broker state when it performs tool calls in an active turn; it cannot silently maintain a continuous background poll between messages unless a separate scheduled/automation mechanism is explicitly configured and available. Never imply continuous realtime monitoring is running when it is not.

## User correction — monitoring hasil Agent — 2026-11-18

- **USER_CONFIRMED:** Saat ChatGPT mengirim tugas ke Agent, ChatGPT sendiri harus memeriksa status dan hasil Agent; pengguna tidak seharusnya diminta memberi tahu bahwa Agent sudah selesai atau mengirim hasil secara manual.
- **Operational requirement:** selama giliran kerja aktif setelah dispatch, ChatGPT harus melakukan polling broker secara proaktif dan mengambil hasil begitu tersedia, lalu melaporkan status berhasil/gagal beserta bukti. Jangan berhenti setelah hanya mengirim wake email atau menyatakan sedang menunggu.
- **Technical limitation verified:** tool Automations yang tersedia hanya mendukung jadwal sekali, harian, mingguan, atau bulanan; Railway scheduled automations paling sering sekali per jam dan tidak mengirim notifikasi. Keduanya tidak memenuhi pemantauan realtime antar-giliran. ChatGPT tidak memiliki proses yang tetap berjalan dan menerima event KV secara spontan setelah giliran berakhir.
- **Do not misrepresent:** jangan menyebut pemantauan realtime/background sudah aktif. Jangan membuat jadwal lambat sebagai pengganti tanpa persetujuan pengguna. Untuk tugas yang sedang dikerjakan, gunakan polling aktif dalam giliran yang sama; jika giliran berakhir sebelum hasil tersedia, nyatakan secara jujur bahwa pemantauan terhenti dan lanjutkan pengecekan saat percakapan aktif kembali.
- **Current live broker check (2026-11-18):** `agent:broker:pending` dan `agent:broker:active` tidak ditemukan; hasil dan status tes ChatGPT Web sebelumnya masih tersimpan dan berstatus `complete`. Tidak ada tugas Agent baru yang sedang berjalan pada saat pemeriksaan ini.


## Result checkpoint — live Agent Instagram read-only integration test — 2026-11-18

- Dispatched exactly one read-only task: `agent-instagram-readonly-smoke-20261118-01`. Before dispatch, Cloudflare KV showed no broker pending or active task; no duplicate was created.
- Sent one Gmail wake marker through the existing configured mailbox. Gmail confirmed message ID `1a1eb3b27b30edff`.
- ChatGPT polled the broker itself. The command moved from `pending` to `active` with a matching task ID, then the matching result/status appeared. Final readback: pending absent, active absent, result present, status `complete`, attempt 1.
- Agent used its own browser to open and read `https://www.instagram.com/`. Result: page loaded; title `Instagram`; visible login prompts included “Login”, “Masuk”, “daftar”, and “Buka Instagram”. Based only on visible page text, the Agent browser session appears signed out.
- Agent explicitly reported no clicks, typing, sign-in, prompts, messages, or state-changing actions. No Instagram account data was accessed.
- **Verified:** the existing task-scoped Gmail notification → Agent execution → Cloudflare result → ChatGPT retrieval path worked live for this task. ChatGPT did not require the user to relay completion or copy the Agent result.
- **Not verified:** native direct ChatGPT↔Agent MCP connection; authenticated Instagram session; any Instagram write/action capability; monitoring after the active ChatGPT turn ends.
- Upstream source inspection found Agent's `LocalMcpServer` and `AutomationServer` bind to `127.0.0.1`; these endpoints are not directly reachable from Cloudflare. A truly direct remote MCP connection would require a secure app-side outbound tunnel/relay and a way for ChatGPT to register/use that endpoint. No Android device is connected to Remote Desktop Commander, so this session cannot install or modify the phone app remotely.
- No Agent source, Worker code, phone settings, routines, or permissions were changed. The test was read-only and completed without retry.


## Continuation checkpoint — native app and connected Instagram status — 2026-11-18

- Re-read current project rules, control, and this decision ledger from main before continuing. Current operating model remains USER_CONFIRMED: Agent executes on Android; ChatGPT dispatches, monitors task-scoped work, retrieves/evaluates results, and chooses follow-up. No 24/7 watcher. MacroDroid and ntfy remain REJECTED.
- Live broker preflight before each dispatch showed no pending or active task. Existing browser test result was re-read from KV and remained `complete` on attempt 1: Instagram web landing page loads, but visible login prompts indicate the browser profile is signed out.
- Task `agent-instagram-native-app-session-readonly-20261118-01` was dispatched once to inspect the installed Instagram Android app using Agent background-screen tools. Agent returned `ok:false`, status `needs_chatgpt_review`, attempt 1: `screen_open_app` and `screen_look` were unavailable in that task's runtime. The native app was not opened and no account state was changed. Do not retry the same unavailable tools.
- Source inspection of Agent main branch confirms that source code defines `screen_open_app` / `screen_look` and the background-screen tool family, but the task runtime did not expose them. This is a runtime capability mismatch, not proof that Instagram is absent from the phone.
- Agent source also defines `apps_connected`, which returns connected toolkit names/statuses, and `apps_find_tools` / `apps_run`. These are the next relevant read-only path to determine whether Instagram is already connected through Composio, independently of the signed-out browser profile.
- Dispatched exactly one read-only Composio status task: `agent-instagram-composio-connection-readonly-20261118-01`. Scope is only whether Instagram is present and ACTIVE; it explicitly prohibits exposing account IDs/labels, connecting apps, or running actions. One Gmail wake email was sent (message ID `1a1eb406785f8918`).
- Latest broker readback at `2026-11-17T20:44:08.659Z`: exact task remains in `agent:broker:pending`; `agent:broker:active`, matching result, and matching status are absent. Thus this task is queued but not yet claimed/completed at that readback. Do not duplicate it; continue checking the same task when the ChatGPT turn resumes.
- No direct MCP tunnel was built or deployed. Agent's local MCP and Automation servers still bind to loopback only. A native remote MCP integration requires a secure outbound app-side connection plus app installation/update; no authorized Android device-control channel is available in this session. Do not claim direct integration exists.
- No phone settings, Agent routines, permissions, secrets, or Instagram account state were changed. No login, posting, messaging, follow/unfollow, or other write action was performed.


## Result checkpoint — Instagram native-app / Composio capability checks — 2026-11-18

- Task `agent-instagram-native-app-session-readonly-20261118-01` completed on attempt 1 with `needs_chatgpt_review`: the run did not expose `screen_open_app` or `screen_look`; the native Instagram app was not opened. No state-changing action occurred.
- Task `agent-instagram-composio-connection-readonly-20261118-01` completed on attempt 1 with `needs_chatgpt_review`: the run did not expose `apps_connected`; no connection or account changes occurred. The result does not prove Instagram is disconnected; it proves the tool was unavailable in that background run.
- Source inspection of Agent `AgentRuntime.kt` explains the conditional tool exposure: Composio tools are added only when `composioEnabled` is true (with a separate tool set for consumer keys); background-screen tools are added only when `Screens.on(context)` is true. The two task failures are consistent with those optional capabilities not being available in the current routine runtime. Actual phone setting values are not remotely observable from this session.
- Agent's browser tools are always included in the worker tool set. Browser access to Instagram's public landing page is verified, but the browser profile is signed out. Agent's own Browser UI supports a user taking over a profile to sign in and then handing the browser back. ChatGPT must not request or collect the password/2FA code in chat.
- Latest broker readback after both tasks: pending absent; active absent; both matching result/status records present. No duplicate tasks remain queued.
- **Current practical route:** use the already-working Agent browser profile, with a one-time user-controlled sign-in/takeover in Agent's Browser UI. After the user hands the browser back, ChatGPT can continue with read-only inspection via the existing bridge. Do not claim authenticated access until a fresh browser_read confirms it.
- **Direct MCP status unchanged:** no remote tunnel or app-side code has been installed/deployed. Source servers bind to loopback. The current bridge is task-scoped dispatch/result relay, not native direct MCP.


## Result checkpoint — Agent-connected Instagram profile read — 2026-11-18

- User correction: the Instagram connection is already configured inside Agent. Do not confuse Agent's connected Instagram integration with Agent's separate browser profile; the earlier browser login screen was not evidence that the connected integration was signed out.
- Dispatched one narrowly scoped read-only task: `instagram-connected-profile-readonly-20261118-01`. It explicitly required using the existing Agent Instagram connection (not browser/public website), reading only the authenticated account's own username, display name, and bio, and making no changes.
- ChatGPT independently polled Cloudflare KV. Final readback: `agent:broker:pending` absent; `agent:broker:active` absent; matching status `complete`, attempt 1; matching result present. No user relay was needed.
- Agent reported success using `INSTAGRAM_GET_USER_INFO` on authenticated account `me`: username `rapiusahatools`; display name `RapiUsaha | Tools UMKM`; bio was not returned by the connected profile tool.
- No other accounts, posts, DMs, comments, or private messages were accessed. No login, connection, profile, or account changes were made.
- **VERIFIED:** Agent's existing connected Instagram integration can read the authenticated account's own basic profile. This is the first direct evidence that the connected integration is active; the previous browser smoke test is a separate access path and must not be used to contradict it.
- **OPEN:** profile bio retrieval is not yet confirmed; broader Instagram read/write capabilities are not yet tested. Continue with the user's requested work through Agent, inspect available connected tools and results, and keep all actions read-only unless the user explicitly approves a specific state-changing action.

## User correction — handoff workflow for Agent results — 2026-11-18

- **USER_CONFIRMED:** Stop attempting realtime/continuous polling while the user waits. The user is frustrated by progress claims that do not produce a retrievable result.
- **USER_CONFIRMED operating sequence:** (1) ChatGPT sends one scoped task to Agent through the existing broker; (2) Agent executes and returns its full response through the broker result channel; (3) the user sends a short signal such as "udah" only after Agent has finished; (4) only then ChatGPT reads the exact task status/result from Cloudflare KV, evaluates it, and chooses the next step.
- ChatGPT must not ask the user to copy/paste Agent's output if the broker result exists. The user's "udah" is a completion signal, not the result payload.
- Every task instruction must explicitly require Agent to return a complete, structured result through the existing broker result mechanism, including exact errors/reasons if it cannot execute. Do not rely on a verbal/UI rejection with no broker result.
- No realtime/background monitoring claim. Do not poll repeatedly while the user is waiting. Check once after the user's completion signal.
- **OPEN:** whether the existing Agent routine actually persists rejected/failed responses into the broker result channel. Verify this on the next controlled task; if rejection still produces no result, diagnose the Agent routine/result-write path before sending further work.
- **Current stale task:** `instagram-aesthetic-audit-readonly-20261118-01` is still present in `agent:broker:pending` at the latest readback; no matching active/status/result keys exist. Do not dispatch a duplicate. Resolve or let this stale command expire before sending a new one.



## Corrective Agent dispatch — Instagram result-return repair — 2026-11-18

- User correction: when Agent shows a completed/rejected task but the broker remains pending, ChatGPT must repair the task/result path directly rather than merely report the mismatch or promise a future fix.
- Live evidence before correction: task `instagram-aesthetic-audit-readonly-20261118-01` remained in `agent:broker:pending`; matching active/status/result keys were absent. The original broad visual-audit instruction was still queued. Therefore the Agent UI rejection had not been returned through the broker. Exact UI rejection text is not available in KV; do not claim its precise cause.
- Corrective action: replaced the stale pending command and same ticket task record with a minimal, single-operation task using the already-verified connected Instagram action `INSTAGRAM_GET_USER_INFO` for authenticated account `me`. It is read-only, avoids browser/visual inspection, and explicitly requires POST to `/agent/result` with either the full successful result or the exact error. Agent must confirm the POST response.
- Sent one fresh Gmail wake email using the same ticket marker. Gmail message ID: `1a1eb52d6b21c5e8`. KV write and readback both succeeded; the pending command and task record contain the revised instruction. No duplicate task ID or new ticket was created.
- **Current status:** corrected task is queued in `agent:broker:pending`; execution and broker result are NOT YET VERIFIED. No matching active/status/result key was observed in the post-write readback. Do not claim Agent has executed it yet. User's established handoff remains: user says "udah" when Agent has finished; then ChatGPT checks the exact broker result once.
- No Instagram profile/content changes, posts, messages, follows, likes, or other account mutations were made.


## Corrective Worker result-ingress fix — 2026-11-18

- User asked ChatGPT to repair the Agent→broker result failure directly, not merely describe it.
- Confirmed deployed Worker behavior before the fix: POST /agent/result required agent:broker:active. If the Agent-side runner completed/displayed a task without first claiming agent:broker:pending, the result endpoint returned HTTP 409 no active broker task, even when the ticket-authenticated task record and matching pending command existed. This was a concrete bridge-side gap.
- Patched the deployed kreavo-bridge Worker result ingress: it now accepts a result when either (a) the matching task is active, or (b) no active task exists but the ticket-authenticated pending command has the exact same task ID. It rechecks active state before accepting the pending fallback, validates the ticket/task ID, writes the same result/status records, and deletes the matching pending key. Mismatched IDs remain rejected; no auto-retry was added.
- Cloudflare upload succeeded; the Worker API returned success with deployment ID da967861349a4799baa37b8a3af626e8. Immediate source readback confirmed the new pending-result branch is present.
- NOT YET RUNTIME-VERIFIED: no Agent POST has yet exercised the new pending-result branch. The currently queued Instagram task remains pending; matching active/status/result records were absent at the last readback. This fix repairs the bridge's rejection of a pending-task result, but cannot manufacture the Agent UI response if the Android routine never sends a POST.
- No Instagram account changes were made. No phone settings, Agent routines, secrets, or permissions were changed.
- Next: verify the Agent's next result POST reaches the Worker and is stored; if no POST arrives, the remaining blocker is the Android Agent routine not invoking /agent/result, which cannot be modified from this session without a connected Android device-control channel.


## Agent result-delivery diagnosis and isolated transport test — 2026-11-18

- Re-read the canonical project rules, control file, and this ledger from main before continuing. The USER_CONFIRMED operating model remains: Android Agent executes; ChatGPT controls dispatch and retrieves results after the user's completion signal. Read-only Instagram scope remains in force. MacroDroid and ntfy remain REJECTED.
- Inspected the Agent app source at `Past-da-king/agent` main, including `AgentRuntime.kt`, `CoreTools.kt`, `BrowserTools.kt`, and `CodeTools.kt`.
- Verified available routine tools: `routineTools()` includes `tools(forHelper = true)`, which includes `RunCodeTool`. `RunCodeTool` runs Node 24 and supports JavaScript `fetch`, including HTTP POST. `WebFetchTool` only performs GET. `BrowserEvalTool`'s own tool description says sending/posting/deleting requires `request_approval`; a background routine uses `interactive=false`, so it is not a dependable result-return route.
- Found a concrete contradiction in the prior corrective task: it said to call exactly one Instagram tool while also requiring a separate HTTP POST to `/agent/result`; it did not specify `run_code`, the exact Worker URL, the `X-Agent-Ticket` header, or the required allowed `Origin` header. The Agent therefore was not given a clear executable result-return procedure. This is the strongest source-backed explanation for the missing result, but the exact prior UI rejection text is still unavailable, so do not present it as certain proof of what the Agent actually attempted.
- Confirmed the Worker result endpoint checks `Origin` against `https://www.kreavo.id` / `https://kreavo.id`, authenticates `X-Agent-Ticket`, and requires matching task ID. The deployed Worker now also accepts a matching result while the task is still pending (the previous active-only gap was patched in deployment `da967861349a4799baa37b8a3af626e8`).
- Dispatched one isolated, non-Instagram transport smoke task: `broker-result-post-smoke-20261118-01`. The instruction explicitly calls `run_code` with the exact POST URL `https://kreavo-bridge.usahalab-id.workers.dev/agent/result`, `X-Agent-Ticket`, allowed `Origin`, and a fixed harmless result body. It forbids Instagram access and any account changes. KV task/pending records were written and read back. Gmail wake email sent: `1a1eb663b7d25f41`.
- Post-dispatch readback: exact smoke task remains in `agent:broker:pending`; `agent:broker:active`, matching `status`, and matching `result` are absent. Therefore execution/result delivery is NOT YET VERIFIED. Ticket is intentionally not recorded in GitHub.
- No changes were made to the Agent app source or the user's phone. No Instagram profile, content, DM, follow, or other account state was changed.
- Next: after the user says "udah", read the exact smoke task result/status once. If successful, use the same explicit `run_code` POST procedure alongside the Instagram read tool in a new scoped task. If it fails, diagnose the exact runtime/HTTP error before dispatching another Instagram task. Do not poll while the user waits.
