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
