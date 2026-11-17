## 0BJ. RESTORE CHATGPT-OWNED FAILURE DECISIONS IN WORKER — 2026-11-18

**Status: DEPLOYED AND SOURCE-READBACK VERIFIED; failure-path runtime test not yet performed.**

- The deployed `kreavo-bridge` Worker previously used Cloudflare Workers AI (`@cf/google/gemma-4-26b-a4b-it`) to classify Agent failures and could automatically enqueue retries. This contradicted the user's confirmed Option A, where ChatGPT owns diagnosis and retry decisions.
- Corrected the Worker source through its content-only upload endpoint. The failure path now records `needs_chatgpt_review` and a clear reason; it no longer calls the Workers AI triage function or enqueues retries. Success handling, ticket validation, pending/active claim flow, matching task-ID validation, result storage, and the existing Kreavo client routes were preserved.
- Deployment API returned success: deployment ID `85a244f23a644e1a8563c31d1642d390`; deployed script source was fetched again and verified to contain `needs_chatgpt_review` and not contain `inspectAgentFailure`, `allowAutoRetry === true`, or `retryQueued`.
- Worker settings readback confirms existing `BRIDGE_KV` and `AI` bindings remain present. The AI binding is now unused by the failure path; it was left intact to minimize unrelated configuration changes.
- The already-completed read-only smoke task remains successful. No new task was dispatched, no KV task state was modified, and no Agent routine or phone setting was changed.
- Runtime test of the new failure branch has not been performed; do not claim it has been exercised. On the next genuine failed task, ChatGPT must read the stored result/status, diagnose it, and decide whether a new task is safe and appropriate.
- Next: use the proven task-scoped Gmail wake → Agent execution → KV result retrieval cycle; verify each exact task ID. Keep the user out of the result-relay loop. No 24/7 monitoring is required.

---
## 0BI. TASK-SCOPED BRIDGE EXECUTION AUDIT — 2026-11-18

**Status: SUCCESS. Existing broker smoke task completed and its matching result was retrieved by ChatGPT.**

- User's confirmed operating requirement: ChatGPT dispatches a task, retrieves and evaluates Agent's result, and chooses any follow-up. The user does not need to monitor Agent or relay its responses. No 24/7 live monitoring requirement.
- Fresh Cloudflare KV inspection before wake found the existing read-only task `broker-readonly-smoke-20261118-01` pending, with no active claim or result. No duplicate task was created.
- A Gmail wake email was sent to the user's own connected mailbox using the existing task ticket in the subject. The send succeeded. After the wake, Cloudflare KV showed the pending and active keys cleared and a matching result/status record appeared.
- Retrieved result: task ID exactly matched `broker-readonly-smoke-20261118-01`; `ok:true`; Agent reported `browser_open` and `browser_read` succeeded on `https://example.com/`, title `Example Domain`, no visible H1, and no state change. Attempt 1; Worker status `complete`. This verifies one task-scoped Agent → Cloudflare Bridge → ChatGPT result round-trip. It does not prove arbitrary Android actions or continuous background monitoring.
- A previous legacy task `bootstrap-poller-20261118-01` also has a matching claim and result; Agent reported a successful read of example.com. This is additional historical round-trip evidence, not evidence that a recurring poller/routine was configured.
- Remote Desktop Commander still reports no connected devices; there is no direct Android control channel in this session.
- Direct read of the deployed Worker source confirms `/agent/command` claims the pending broker task and `/agent/result` stores matching results. It also confirms the Worker invokes Cloudflare Workers AI (`@cf/google/gemma-4-26b-a4b-it`) to classify failures and may enqueue a retry automatically when the task retry policy allows it. **This is an implementation conflict with the user's confirmed Option A**, where ChatGPT owns failure diagnosis and retry decisions. Do not treat Workers AI auto-retry as user-approved architecture. Review and correct this before relying on broker auto-retries for future work.
- No Worker code, secrets, Agent routine, or phone settings were changed. No task beyond the already-existing read-only smoke task was dispatched.
- Next: align the Worker failure path with ChatGPT-owned diagnosis/retries, then use this task-scoped dispatch → wake → result retrieval → evaluation cycle for future tasks. No 24/7 watcher is required.

---
## 0BH. USER CLARIFIES AGENT OPERATING MODEL — 2026-11-18

**Status: USER_CONFIRMED operating requirement; implementation remains BLOCKED at the Android wake/execution boundary.**

- User does not require ChatGPT to monitor the Android Agent 24/7 or maintain a permanent real-time watch.
- Desired workflow is task-scoped orchestration: ChatGPT sends a task to Agent, checks/waits for the task result while the work is in progress, evaluates the returned output, and decides whether to send a follow-up task. The user should not need to open Agent repeatedly, copy its responses, or relay corrections manually.
- ChatGPT should ask the user only when a genuine human decision/approval is needed.
- This clarification does **not** authorize a permanent architecture change. Preserve the last explicit Option A decision: ChatGPT owns task dispatch, result retrieval, diagnosis and retry decisions; Agent executes on Android.
- Distinguish bounded task-scoped checking from 24/7 monitoring. A persistent always-on watcher is not a requirement.
- Current blocker remains: no connected remote Android device, and no verified phone-side wake mechanism. Existing pending read-only task must not be duplicated or replayed. No task was dispatched by this clarification.
- Next: design/verify the smallest task-scoped dispatch-and-result cycle compatible with Agent's actual trigger constraints, without asking the user to relay Agent output or configure MacroDroid again.

---

## 0BG. FRESH REMOTE BRIDGE CHECK — 2026-11-18 03:07 +0800

**Status: BLOCKED at the Android-device boundary; cloud-only completion is not possible with currently connected devices.**

- Re-read canonical rules and project checkpoints. Directly fetched deployed Cloudflare Worker `kreavo-bridge`: broker routes exist, but there is no phone-wake endpoint. Bindings are `BRIDGE_KV` and `AI`; secret list is empty.
- Current KV state: existing read-only task `broker-readonly-smoke-20261118-01` remains pending; no active broker task. It was not claimed, duplicated, replayed, or modified.
- Remote Desktop Commander again reports no connected devices. ChatGPT has no remote Android control channel in this session.
- No Worker/KV/Agent/phone changes and no new dispatch were made.
- MacroDroid and ntfy troubleshooting remain paused. A possible fallback is an Agent-native 15-minute interval poller. This is not configured and is not approved as a permanent architecture switch; it would be a limited compromise because Agent runs the fixed poll routine, while ChatGPT can still own task creation, result validation, diagnosis and retries.
- Next: ask for authorization only if proceeding with that specific 15-minute compromise; otherwise the exact blocker is a missing phone-side enablement or supported remote device connection. See `research/android_agent_bridge_autonomous_recovery_2026-11-18.md` for evidence.

---

## 0BF. USER DIRECTS ASSISTANT TO TAKE OVER BRIDGE RECOVERY — 2026-11-18

**Status: USER_CONFIRMED instruction to take over technical work and use available methods; route change not yet locked to a specific architecture.**

- User is fed up with repeated MacroDroid UI instructions, cannot send more screenshots, and explicitly directed ChatGPT to handle the bridge itself and use any available method. Stop asking the user to perform repetitive MacroDroid setup or send screenshots. Exhaust remote investigation first; if an on-device step is truly unavoidable, request only the single minimum action and explain why.
- Active project remains ChatGPT ↔ Android Agent Bridge. RapiUsaha remains the separate B2C digital-products business direction.
- Last explicit architecture decision remains Option A (ChatGPT owns dispatch, result retrieval, diagnosis and retries; Android Agent executes; MacroDroid Free was intended only as a wake adapter). The new broad instruction authorizes exploring alternate implementations but does not explicitly select Agent-owned reasoning/polling as the new architecture.
- Re-read `CHATGPT_WORKING_RULES.md`, this file and the latest Agent Bridge checkpoint. New audit is recorded in `research/android_agent_bridge_autonomous_recovery_2026-11-18.md`.
- Remote Desktop Commander again returned **no connected devices**. This ChatGPT session has no direct Android control channel.
- Cloudflare read-only inspection: Worker `kreavo-bridge` and KV namespace exist; Worker bindings include `BRIDGE_KV` and `AI`; its Worker secrets list is empty. Existing pending commands are `broker-readonly-smoke-20261118-01` and `bootstrap-poller-20261118-01`. Do not duplicate, overwrite, claim, or replay them until state is rechecked.
- Worker already has `/agent/command` and `/agent/result` broker routes, but no external phone-wake route/secret is configured. No Agent claim or valid result was verified in this audit.
- Upstream Agent source at commit `8594dfadc3f728d0123990179dfc4361d55dd69c` confirms:
  - External routine broadcasts require an on-device sender (or supported linked computer), package `com.past9.phoneaos`, action `com.past9.phoneaos.RUN_ROUTINE`, and extras `token` + `routine`.
  - Agent HTTP automation listens only on `127.0.0.1:47321` while the app is running; Cloudflare cannot call it directly from the internet.
  - Agent routines support interval triggers (minimum 15 minutes), email triggers checked on a 15-minute cadence, and notification triggers for allowed apps. A fixed broker-poll routine is a possible free fallback, not yet configured/tested and not real-time.
- MacroDroid manual UI setup is paused. Do not continue telling the user to create variables or send screenshots. Do not pay for MacroDroid Pro, API billing, subscriptions or any other service without explicit approval. Keep the Agent token on the phone.
- Next: continue remote-only investigation and inspect the Worker/broker implementation. If no remote route exists, clearly identify the one unavoidable phone-side enablement instead of pretending the bridge is complete. Preserve ChatGPT as task owner wherever possible. Verify any eventual round trip using an Agent claim and a matching valid result, not an HTTP 2xx alone.

---

## 0BD. USER LOCKED CHATGPT-OWNED ORCHESTRATION — OPTION A — 2026-11-18

**Status: USER_CONFIRMED.**

- User explicitly selected Option A: ChatGPT remains the brain/orchestrator; Android Agent remains the phone-side executor; MacroDroid Free is only the wake adapter. Do not substitute Agent-owned polling as the primary architecture.
- No paid tools, MacroDroid Pro, subscriptions, or new API billing are authorized by this choice.
- Active project: ChatGPT ↔ Android Agent Bridge. RapiUsaha remains the separate business direction (B2C digital products for UMKM).
- Current verified implementation status: existing Cloudflare Worker/KV broker exists; one read-only smoke task `broker-readonly-smoke-20261118-01` remains pending; no active claim was found in the latest check. MacroDroid is not yet installed/configured; no webhook URL or Agent token has been provided; no Worker wake route/secret exists. Do not duplicate or replay the pending task while state is uncertain.
- Next: user configures one free MacroDroid macro using basic Webhook (URL) trigger → explicit Send Intent/Broadcast to Agent. User must never share the Agent token; enter it locally on the phone. User may provide the generated webhook URL for secure storage as a Cloudflare Worker secret only. Then inspect Worker/KV again, implement the narrow wake route, and verify Agent claim plus valid result. A webhook HTTP success alone is not proof.
- The architecture choice is settled; do not ask the user to choose between ChatGPT-owned orchestration and Agent polling again. Research details and security boundaries are in `research/chatgpt_agent_action_execution_2026-11-17.md`, section 0BB.

---

---

---

## 0AZ. FREE NTFY PUSH WAKE PATH; PAID MACRODROID DEPENDENCY REJECTED — 2026-11-18

**Status: ASSISTANT_PROPOSAL / source-verified, not yet configured or end-to-end tested.** User explicitly rejected a paid MacroDroid dependency, even though its Pro payment is one-time. Do not ask the user to purchase MacroDroid or any paid automation tool. The 0AY MacroDroid proposal is superseded/rejected for this project.

- Active technical project: ChatGPT ↔ Android Agent Bridge. User's approved intent remains ChatGPT-owned dispatch, result retrieval, diagnosis and safe retries; user should not relay Agent output. RapiUsaha remains B2C digital products for UMKM.
- New no-cost candidate: **ntfy Android app + free ntfy.sh topic + existing Agent notification routine + existing Cloudflare Worker**.
  1. ntfy.sh is documented as free to use without signup or fees for normal use; paid plans are optional. Android app is open-source and free on Google Play/F-Droid.
  2. Use the Google Play build on the main ntfy.sh host so the app uses FCM for push delivery. F-Droid build omits Firebase and keeps a foreground connection instead; do not recommend F-Droid for this low-background-cost setup.
  3. Subscribe the Android app to one high-entropy, hard-to-guess topic. ntfy topics are public/unlisted rather than private; anyone who knows the topic can publish/subscribe. Therefore the topic URL is a bearer capability and must be kept out of GitHub. Never send task contents, private data, or Agent credentials through the notification; only send the marker and ephemeral ticket.
  4. Android package name is `io.heckel.ntfy` (verified in official ntfy Android build configuration and F-Droid package).
  5. Agent `NotificationReader` already supports package+keyword matching. It reads only explicitly allowed apps and fires a notification routine for a live matching notification. Existing ChatGPT Bridge routine #2 can be updated in place to `io.heckel.ntfy|CHATGPT-AGENT`, preserving its prompt and Learned App tools. Add ntfy to Agent's allowed notification apps and ensure Android Notification Access is enabled.
  6. Existing Cloudflare Worker can later send a POST to `https://ntfy.sh/<secret-topic>` with title `CHATGPT-AGENT` and body containing only the ephemeral ticket. This will wake the existing Agent notification routine. ChatGPT continues reading the task/result through existing KV broker keys.
- User-side one-time setup still unavoidable: install ntfy from Google Play, subscribe to the unique topic ChatGPT supplies, then in Agent allow ntfy notifications and update existing routine #2 trigger to the exact package+keyword. The Agent's main chat can be instructed to perform the routine update in place and verify it with routine_list; do not create a duplicate routine or alter its prompt unnecessarily.
- Security: ntfy public topics are not confidential. Generate a random high-entropy topic and treat it as a secret URL. Store it only as a Cloudflare Worker secret, not in GitHub. Notification body must contain no task details or private data. Existing Agent AppGuard, Trust boundaries and approvals remain unchanged.
- Limitations: ntfy.sh best-effort uptime, no SLA; Google Play build uses FCM and may be delayed by network/Doze. F-Droid flavor uses a persistent foreground service instead. A successful HTTP publish is not proof Agent ran; only an observed Agent claim and valid result prove end-to-end success.
- Current status: no ntfy app installation/subscription confirmed; no topic created or disclosed; no Agent notification allowlist/routine changes; no Worker route/secret changes; no new test task or duplicate wake sent. This is a researched free alternative, not yet operational.
- Sources:
  - ntfy upstream README states ntfy.sh is free without signup/fees: https://github.com/binwiederhier/ntfy
  - ntfy Android docs on Play/F-Droid FCM differences and instant delivery: https://docs.ntfy.sh/subscribe/phone/
  - ntfy Android F-Droid listing / package: https://f-droid.org/packages/io.heckel.ntfy/
  - ntfy Android app package/build flavors: https://github.com/binwiederhier/ntfy-android/blob/main/app/build.gradle
  - Agent NotificationReader and routines source: https://github.com/Past-da-king/agent/blob/main/app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt
  - Agent routine tool schema: https://github.com/Past-da-king/agent/blob/main/app/src/main/java/com/past9/phoneaos/tools/AgentTools.kt

---

## 0AY. INTERNET-TO-ANDROID AGENT WAKE PATH IDENTIFIED VIA MACRODROID — 2026-11-18

**Status: ASSISTANT_PROPOSAL / technically source-verified, not yet configured or end-to-end tested.** This supersedes the earlier statement that no practical internet-to-phone entry point had been found; the old Gmail wake path remains unconsumed, but a separate push-to-broadcast path is now identified.

- Active technical project: ChatGPT ↔ Android Agent Bridge. Last user-confirmed intent remains that ChatGPT owns dispatch, result retrieval, diagnosis and safe retries; the user should not relay Agent output. RapiUsaha remains B2C digital products for UMKM.
- New path:
  1. MacroDroid's **Webhook (URL)** trigger accepts an HTTPS call from anywhere and delivers a trigger to the Android device through Firebase Cloud Messaging (FCM). Query parameters can update pre-created MacroDroid variables. It requires Google Play Services and internet; HTTP acceptance does not prove on-device execution, and FCM/Android may delay delivery.
  2. MacroDroid runs a single local **Send Intent → Broadcast** action, explicitly targeting package `com.past9.phoneaos`.
  3. The current upstream Agent source exposes an exported `IntentTriggerReceiver`. Action `com.past9.phoneaos.RUN_ROUTINE` accepts extras `token`, `routine`, and optional `note`; Android starts Agent for this explicit-package broadcast even when Agent is not already running. The Agent's own `automationAllowed` switch and per-install token remain mandatory; no safety/approval bypass is introduced.
  4. Reuse existing notification Routine #2 by id, passing a note formatted like the existing Gmail notification context with `CHATGPT-AGENT-<ticket>`. This should preserve its existing Learned App/task/result logic; it remains a hypothesis until a harmless live test confirms the installed app and the exact routine prompt behave as expected.
  5. ChatGPT continues to create the existing ticket/task in Cloudflare KV. The existing Worker will need a narrowly scoped wake adapter that calls the MacroDroid webhook with the ticket variable; store the full webhook URL as a Worker secret, never in GitHub. After the broadcast runs, ChatGPT reads the existing Agent claim/result records and handles safe diagnosis/retry.
- Minimal on-phone setup required once:
  - Install MacroDroid if it is not already installed. Its official Google Play listing says Free supports up to 5 macros; one macro is sufficient, so Pro is not required for this design.
  - In Agent Settings → Automation from other apps, copy the Agent token directly into MacroDroid and turn on **Allow outside runs**. Do not send the Agent token to ChatGPT.
  - Create one MacroDroid macro: Webhook trigger with a pre-created string variable `bridge_ticket` (whitelist only that variable); Send Intent action set to Target=Broadcast, Package=`com.past9.phoneaos`, Action=`com.past9.phoneaos.RUN_ROUTINE`, extras `token`=<Agent token>, `routine`=`2`, `note`=`A notification just arrived from Gmail: "CHATGPT-AGENT-{bridge_ticket}".`
  - Share the generated MacroDroid webhook URL with ChatGPT so it can be stored as a Cloudflare Worker secret. Treat that URL as a password/capability URL; do not publish it.
- Security and scope:
  - Agent token stays on the phone inside the local macro. Only the MacroDroid webhook URL is supplied for the Worker secret.
  - Use a high-entropy webhook identifier; no IP allowlist is assumed because Worker egress IPs are not a stable allowlist.
  - Existing Agent approval/AppGuard rules remain in force. Background actions requiring user approval must still stop.
  - No new domain, Queue, OpenAI API key, paid Cloudflare plan, or model switch is required.
- Current state: MacroDroid is not yet verified installed/configured; no webhook URL exists in ChatGPT context; no Worker wake route has been added; no Agent settings/routine have been changed; no task or email has been duplicated. Existing pending bridge tasks are preserved. The Worker adapter and one harmless read-only end-to-end verification are next after the webhook URL is available.
- Important limitations: FCM delivery has no guaranteed latency/SLA and requires Google Play Services plus internet. The exact Agent app build installed on the phone has not been remotely inspected; current upstream source supports the receiver, but the first live test must confirm the installed build includes it. This is an event-driven bridge, not guaranteed real-time.
- Sources inspected:
  - Agent current main `IntentTriggers.kt`: https://github.com/Past-da-king/agent/blob/main/app/src/main/java/com/past9/phoneaos/automation/IntentTriggers.kt
  - Agent current main `AndroidManifest.xml`: https://github.com/Past-da-king/agent/blob/main/app/src/main/AndroidManifest.xml
  - Agent current main `Routines.kt`: https://github.com/Past-da-king/agent/blob/main/app/src/main/java/com/past9/phoneaos/triggers/Routines.kt
  - MacroDroid Webhook trigger: https://www.macrodroidforum.com/wiki/index.php?title=Trigger:_Webhook_(URL)
  - MacroDroid Send Intent action: https://www.macrodroidforum.com/wiki/index.php?title=Action:_Send_Intent
  - MacroDroid official Google Play listing: https://play.google.com/store/apps/details?id=com.arlosoft.macrodroid

---

## 0AX. AGENT PICKUP BLOCKER CONFIRMED; NO REMOTE PHONE ENTRY POINT — 2026-11-18

**Outcome: NOT COMPLETED.** The current ChatGPT session cannot reach the Android Agent process. The Worker/broker is deployed, but the Agent has not claimed or executed the pending test.

- Active project: ChatGPT ↔ Android Agent Bridge. User instruction: complete the loop end-to-end without turning work into user-managed step-by-step tasks; report success only with evidence. RapiUsaha remains B2C digital products for UMKM.
- Re-read working rules, control, and active research file from main.
- Fresh KV inspection:
  - Bootstrap task `bootstrap-poller-20261118-01` remains stored and unclaimed.
  - No claim marker and no result exist.
  - Broker pending task `broker-readonly-smoke-20261118-01` remains queued; no active task/result/status.
- Fresh Gmail read of message `1a1ea741aa45401c`: labels remain `UNREAD,SENT,INBOX`. This supports that the wake was not consumed, but does not prove whether the notification was delivered to Android or why the listener did not fire.
- Fresh Remote Desktop Commander device list: no connected devices.
- Audited upstream Agent source at commit `8594dfadc3f728d0123990179dfc4361d55dd69c`:
  - `NotificationReader` only triggers a notification routine when (a) the Android notification listener is connected, (b) the app package is enabled in Agent notification settings, and (c) a new live notification arrives. Notifications already present when listener reconnects are stored with `live=false` and intentionally do not fire routines.
  - Gmail routine kind `email` is a separate 15-minute WorkManager poll using Composio `GMAIL_FETCH_EMAILS`; it records the newest message ID on first run and only runs on a later new matching email. It does not use Android Gmail notifications.
  - `PhoneAutomation` local HTTP server binds only to `127.0.0.1`; documentation explicitly says nothing outside the phone can reach it. The alternative Android broadcast requires a local sender app and a per-install secret token plus "Allow outside runs". No connected computer/device or authorized remote channel is available in this ChatGPT session.
  - `routine_create/list/update` are main-agent tools, not background routine tools; already corrected in 0AW.
- Thus the current blockers are device-side and cannot be fixed remotely through Gmail/Cloudflare alone: the only configured wake is a Gmail notification trigger, but Agent has not consumed it; ChatGPT cannot inspect or change Agent's notification access, allowed-app list, routine row, or invoke the local loopback/broadcast interface from here.
- No duplicate wake email was sent, no additional side effect attempted, no Worker/KV/Routine configuration changed in this continuation. Existing task tickets are left intact for later recovery; do not generate more wake emails or duplicate tasks.
- The Worker + Workers AI broker remains deployed, but **the end-to-end loop is not working and must not be represented as successful**. Daily ChatGPT automation is only an interim check, not a substitute for the 15-minute phone poller.
- To resume, one real phone-side access path is required: either a connected authorized device-control channel, or a local Agent notification/broadcast integration enabled on the phone. Without one of those, there is no supported way for this ChatGPT session to wake or inspect the Android Agent. Do not imply that Desktop is the only possible architecture; the essential requirement is any authenticated phone-side entry point.
- User explicitly asked to stop with step-by-step reports and finish or state failure. This checkpoint records the failure boundary, not a request to make the user relay Agent output.

Sources:
- `app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt` at upstream commit `8594dfadc3f728d0123990179dfc4361d55dd69c`
- `app/src/main/java/com/past9/phoneaos/triggers/Routines.kt` at the same commit
- `app/src/main/java/com/past9/phoneaos/automation/PhoneAutomation.kt` at the same commit
- `docs/automation-macrodroid.md` at the same commit

---

## 0AW. BOOTSTRAP CORRECTED AFTER AGENT ROUTINE TOOL-SCOPE AUDIT — 2026-11-18

**Status: BRIDGE TEST STILL NOT CLAIMED; ROUTINE CREATION VIA BACKGROUND ROUTINE IS NOT SUPPORTED.**

- Active project: ChatGPT ↔ Android Agent Bridge. Last user-approved direction remains a practical no-Desktop route, with broad authority to complete safe technical work. RapiUsaha B2C digital products for UMKM remains unchanged.
- Re-read working rules, project control, and the active research checkpoint from main before this continuation.
- Current KV readback:
  - Bootstrap task `bootstrap-poller-20261118-01` exists.
  - No `agent:claim:<ticket>` marker and no `agent:result:<ticket>` exist.
  - Broker has no active task/result; read-only task `broker-readonly-smoke-20261118-01` remains in `agent:broker:pending`.
- Gmail message `1a1ea741aa45401c` was fetched directly. It is still labeled `UNREAD`, `SENT`, and `INBOX`; the exact body and subject are the wake marker plus the bootstrap ticket. This is consistent with the Agent not having consumed the wake message, although unread state alone does not prove why the routine did not run.
- **Important correction found by auditing upstream Agent source** at commit `8594dfadc3f728d0123990179dfc4361d55dd69c`:
  - Main-agent `tools()` includes `ScheduleCreateTool`, `ScheduleUpdateTool`, and `ScheduleListTool`.
  - Background `routineTools()` is built from `tools(forHelper = true)` plus task/memory/people/wake tools; it does **not** include schedule/routine create/update/list tools.
  - Therefore the original bootstrap instruction asking a background routine to call `routine_list` / `routine_create` / `routine_update` was invalid. It could not have completed as written even if fetched. Do not repeat that instruction or assume background routines can create other routines.
- Corrected the existing bootstrap KV task in place (same ticket and task ID; no duplicate email) to a strictly read-only `example.com` browser_open/browser_read smoke test that returns via the existing Learned App result tool. Readback confirms the corrected task is stored. No website writes or routine changes are requested by the corrected task.
- Existing Gmail wake message was not resent. Existing notification Routine #2 was not modified. Worker, AI binding, KV namespace, and broker code were not changed in this continuation.
- The previously deployed broker/Workers AI architecture remains deployed and health-checked, but the broker cannot be considered operational until an Agent poller claims and completes its pending test.
- Next: determine why the existing Gmail-triggered routine has not consumed the unread wake message; verify its enabled state and exact trigger configuration through a supported Agent-side path. Do not create duplicate wake emails, do not assume a Desktop connection, and do not claim the 15-minute poller exists.

Source inspected:
- `https://raw.githubusercontent.com/Past-da-king/agent/8594dfadc3f728d0123990179dfc4361d55dd69c/app/src/main/java/com/past9/phoneaos/agent/AgentRuntime.kt`

---

## 0AV. USER APPROVED NO-DESKTOP AI BROKER; WORKER DEPLOYED, AGENT BOOTSTRAP PENDING — 2026-11-18

**Status: PARTIAL IMPLEMENTATION.** User explicitly approved the Workers AI + Agent interval architecture and granted broad authority to complete the technical setup without asking for routine micro-approvals. This does not authorize bypassing Agent AppGuard/user approval, or money, banking, payment, legal acceptance, account-security, password, permanent deletion, or uncertain consequential actions.

- Active technical project remains ChatGPT ↔ Android Agent Bridge. The business direction remains RapiUsaha B2C digital products for UMKM.
- User-confirmed architecture: Agent on Android executes commands; Cloudflare Worker + KV brokers a single active/pending command; Workers AI Gemma 4 performs conservative failure triage and may enqueue a bounded retry only for explicitly idempotent read-only tasks; ChatGPT remains the user's instruction/review surface. The external model is not ChatGPT.
- Live model smoke test succeeded using `@cf/google/gemma-4-26b-a4b-it`; it returned valid JSON, 57 total tokens, approximately 0.736 Neurons.
- Cloudflare Worker `kreavo-bridge` was updated and deployed:
  - Added Workers AI binding `AI`; preserved `BRIDGE_KV` → namespace `ab71a201c13f48fcbf533e9b83e6c19d`.
  - Added persistent broker route branch on the existing `/agent/command` and `/agent/result` endpoints. It serializes one active command and one pending command using the dedicated `agent:broker:*` KV prefix, preserving legacy Kreavo keys and existing ephemeral-ticket route.
  - Added conservative AI failure triage. It defaults to review on model errors, malformed output, partial/uncertain outcomes, authentication/permission/user-approval blockers and consequential actions.
  - Auto-retry is limited to a maximum of two attempts and only when task metadata explicitly says `idempotent=true`, `allowAutoRetry=true`, `actionClass="read_only"`, and the model clearly classifies a transient failure before a side effect. No auto-retry for writes or uncertain actions.
  - Added `agent:claim:<ticket>` markers to the legacy ephemeral fetch route to diagnose whether a task was picked up.
  - First broker/AI deployment ID: `cbb0c7d1c8d0413a90fde3117ac2b5c7`. Follow-up claim-marker deployment ID: `e6eb16cb0900490c962e2f6874b8cbd4`, ETag `1ccb1a2e5c4f569cc3afbdab47c944d2fef24fe59d0848924179d4390fa6b716`.
  - Fresh Worker source readback confirmed the broker and AI triage code plus the claim marker. Settings readback confirmed `AI` and `BRIDGE_KV`. Public `/health` endpoint was independently fetched and returned `{"ok":true,"service":"kreavo-bridge","version":"1.0"}`.
- Bootstrap dispatch:
  - Stored a one-use bootstrap task with ID `bootstrap-poller-20261118-01`, plus one read-only broker smoke task `broker-readonly-smoke-20261118-01`. The random capability tickets are intentionally omitted from GitHub.
  - Sent one Gmail wake message to the user's own mailbox; message ID `1a1ea741aa45401c`; sender and recipient are the same mailbox; subject/body contain the matching `CHATGPT-AGENT` trigger marker.
  - At last KV readback, the bootstrap task still existed, but neither a claim marker nor result existed. The broker pending task still existed, with no active command and no broker result. Thus **Agent has not yet been proven to have fetched or executed the bootstrap**, and the new interval routine is NOT confirmed active.
- Existing notification Routine #2 was not modified. The bootstrap instruction asks that Agent create/update a separate routine named `Agent Bridge Poller` with `kind="interval"`, `spec="15"` (15 minutes), while preserving Routine #2. Routine creation and readback remain pending.
- A live smoke test via the public Worker health route succeeded. A direct Cloudflare-execute fetch to the Worker domain was blocked by the tool network sandbox; this is not an Agent Bridge failure.
- Agent interval uses Android WorkManager with network constraints and may be delayed by Doze/power management; 15 minutes is a minimum cadence, not a real-time guarantee.
- No OpenAI API key, Cloudflare Paid plan, new Queues, new domain, email service, or payment configuration was created. No business model or other project changed.
- Next: inspect KV for bootstrap claim/result and broker active/pending/result. Do not duplicate the wake message or the active command. If bootstrap is fetched, verify the routine's ID/name/kind/spec/enabled state and saved prompt from Agent's routine_list output, then verify the read-only example.com task result. If not fetched, diagnose the Android Gmail notification/routine execution blocker before changing the design.

---

## 0AU. PRACTICAL NO-DESKTOP AGENT ORCHESTRATION OPTION IDENTIFIED — 2026-11-18

**Status: RESEARCH COMPLETE; PROPOSAL ONLY. No routine, Worker, KV, billing, or account changes.**

- ChatGPT automation scheduling in this account cannot poll more frequently than daily because supported recurring cadences are daily, weekly, and monthly.
- Live Worker settings check confirmed `kreavo-bridge` currently has only `BRIDGE_KV`, no AI binding.
- Practical candidate: Agent interval routine (minimum 15 minutes, Android WorkManager may delay) + existing Cloudflare Worker + Workers AI free daily allocation (10,000 Neurons). Candidate model: Gemma 4 26B A4B (reasoning/function-calling/vision). It is a different model, not ChatGPT.
- Exact ChatGPT/OpenAI reasoning via API remains possible but is billed separately from ChatGPT Free. Current API-credit benefit is for eligible Pro/Business plans, not Free.
- Recommendation for low-cost autonomous loop: option 2 in `research/chatgpt_agent_action_execution_2026-11-17.md`; this is only an assistant proposal. Obtain explicit user approval before changing Worker code/bindings or Routine #2. Preserve per-action authorization and AppGuard restrictions.
- Detailed findings and official sources are appended to the research file. No Desktop/PC dependency is proposed.

---

## 0AT. DAILY BRIDGE MONITOR ENABLED; EVENT-DRIVEN ALTERNATIVES RESEARCHED — 2026-11-18

**Status: DAILY MONITOR ENABLED; REAL-TIME CHATGPT ORCHESTRATION NOT YET IMPLEMENTED.** User explicitly approved replacing the weekly B2B software partner monitor with an Agent Bridge monitor, and requested research into a stronger method.

- Active project: ChatGPT ↔ Android Agent Bridge. RapiUsaha B2C digital products for UMKM remains unchanged.
- User-approved replacement applied to existing automation `6af47a587e788191ab32ee88d34d47fe`:
  - Old title: "Pantau program partner software B2B"
  - New title: "Monitor Agent Bridge"
  - Enabled: true
  - Schedule: daily, Asia/Makassar, first run 2026-11-19 09:00
  - Prompt reads Agent results from Cloudflare KV, validates result/task ID, avoids duplicate or unsafe retries, writes handled markers, and notifies parent ChatGPT only for new results/material blockers.
  - Re-read using automation peek confirmed complete prompt, schedule and enabled state.
- The other two automations were left unchanged: SignageQuote uptime daily, and one-time SignageQuote evaluation. User authorized only replacing weekly B2B partner monitor.
- Cloudflare Worker `kreavo-bridge` Agent task/result TTL was changed and readback verified in this turn:
  - `agent:result:<ticket>` and completed `agent:task:<ticket>` retain 604800 seconds (7 days).
  - Unrelated Kreavo client data TTL remains 300 seconds.
  - Ticket validation, origin allowlist, task/result ID matching, and KV binding unchanged.
  - Deployment `025f4829a44c48ecac59487ecd463e44`, tag `3b5f08489c874b27a873a27455fc62aa`.
- New Agent implementation finding from upstream `Past-da-king/agent` commit `8594dfadc3f728d0123990179dfc4361d55dd69c`:
  - `Routines.kt` and `AgentTools.kt` support `kind="interval"` with a minimum interval of 15 minutes.
  - Interval routine uses Android WorkManager periodic work with a network-connected constraint. It can be delayed by Android Doze/power scheduling; it is not guaranteed exact-time/real-time.
  - It can provide more frequent phone-side polling without a separate model API, but the Agent's own model performs its routine reasoning, not ChatGPT's model. Changing Routine #2 is NOT performed or approved yet.
  - Agent also supports `kind="email"` polling, but code shows it wakes on a 15-minute schedule; this likewise has Agent, not ChatGPT, as the decision maker.
- Other architecture options researched:
  1. **Agent browser → dedicated ChatGPT Web controller thread:** potentially avoids separate API billing and lets ChatGPT remain the reasoning endpoint, but UI navigation/login/session/automation reliability has not been tested. Only an option to test, not a verified integration. BrowserTools has browser_open/read/type/press_key/wait and browser_handoff; do not send or post messages during testing without a narrowly scoped approval. The ChatGPT app/web conversation cannot be assumed to expose a supported external webhook or API to start a new ChatGPT turn.
  2. **Cloudflare event-driven orchestrator:** Worker `POST /agent/result` can enqueue or start a Workflow, which can persist, retry, and wait between steps. Requires a model call for diagnosis and a way to wake Agent again. OpenAI API has separate billing from ChatGPT subscription and requires API project/key setup and payment arrangement; no API key created.
  3. **Workers AI alternative:** Cloudflare docs currently describe 10,000 Neurons/day free allocation and multiple reasoning/function-calling models. It avoids separate OpenAI API usage charges inside the allowance, but it is a different model from ChatGPT, has finite daily allocation, and would require an AI binding. Existing Worker has only `BRIDGE_KV`; no AI binding changed.
- Live Cloudflare account check found no zones and no Queues in the account. Cloudflare Email Service requires onboarding a sender domain with Cloudflare DNS; arbitrary recipient sending requires Workers Paid, while verified destination addresses can be free. No domain, queue, email service, OAuth, or payment settings have been changed.
- Recommended next decision for user:
  - If strict ChatGPT-model ownership and no separate API charge are priorities, test Agent's browser against a dedicated ChatGPT web controller in read-only mode first. This may prove brittle and must not be presented as working until tested.
  - If reliable event-driven behavior is the priority, build Cloudflare Worker → Queue/Workflow → model API (OpenAI API or Workers AI). Explain model, cost and credential differences before enabling.
  - If only faster phone-side monitoring is required, consider converting existing Agent Routine #2 to interval 15 minutes, but this delegates reasoning to Agent and Android may delay runs. Preserve its complete prompt and original scope if later approved.
- No Agent Routine was edited, no Queue/Workflow was created, no new AI binding/model key/API key/Gmail OAuth was configured, and no new Agent task dispatched.
- Never state the daily monitor is real-time. The user should not be asked to read/relay Agent replies. Keep per-action authorization and AppGuard restrictions.

---

## 0AS. BRIDGE RESULT RETENTION EXTENDED; AUTOMATED CHATGPT MONITOR BLOCKED BY TASK QUOTA — 2026-11-17

**Status: PARTIAL IMPLEMENTATION.** User authorized building autonomous monitoring so they do not have to inspect Agent replies or relay them to ChatGPT.

- Active project: ChatGPT ↔ Android Agent Bridge. Business direction remains RapiUsaha B2C digital products for UMKM.
- Successfully updated deployed Cloudflare Worker `kreavo-bridge` (account `d7e9ae2527e96a140d325f60b15cdce3`) through the Worker content API:
  - `agent:result:<ticket>` retention changed from 300 seconds to **604,800 seconds (7 days)**.
  - Completed `agent:task:<ticket>` retention changed from 300 seconds to **604,800 seconds (7 days)**.
  - Existing ticket validation, origin restriction, task/result ID matching, and result schema validation were preserved.
  - Unrelated Kreavo client snapshot/result TTLs remain 300 seconds.
  - KV binding preserved: `BRIDGE_KV` → namespace `ab71a201c13f48fcbf533e9b83e6c19d`.
  - Deployment confirmation: Worker `kreavo-bridge`, deployment ID `025f4829a44c48ecac59487ecd463e44`, tag `3b5f08489c874b27a873a27455fc62aa`. A fresh GET of deployed Worker content confirmed both Agent TTL values are 604800 and client TTLs remain 300.
- Attempted to create a daily ChatGPT automation to inspect Agent results and notify the parent thread. Creation was **rejected** because the account already has 3 active scheduled tasks (plan limit 3). Existing tasks observed: daily SignageQuote uptime monitor, weekly B2B software partner research, and one-time SignageQuote 30-day evaluation. None was paused, deleted, or repurposed because user did not authorize changing those tasks.
- Therefore no scheduled ChatGPT monitor was created. ChatGPT still cannot autonomously resume itself in real time when Agent posts a result. The 7-day retention makes later retrieval possible, but does not itself trigger ChatGPT.
- Next viable options: user explicitly chooses one existing automation to pause/replace to free a slot, or a separately engineered always-on external orchestrator using an authorized model/API credential and explicit cost approval. Do not silently substitute daily polling or imply it is real-time.
- No Agent Routine, Gmail settings, Learned App, or business scope was changed. No Agent task was dispatched in this implementation turn.
- Safety: future retry only within original per-action authorization; never repeat uncertain create/send/publish/delete/payment/book actions; never bypass Agent AppGuard, sign-in, CAPTCHA, 2FA, or user approvals.

---

## 0AR. USER CLARIFICATION: CHATGPT MUST OWN THE AGENT LOOP — 2026-11-17

**Status: USER_CONFIRMED intent.** The user's "not having to go back and forth" means they do NOT want to read Agent's replies, copy them into ChatGPT, or decide the next instruction. They are comfortable seeing ChatGPT's own notifications/messages. They want ChatGPT to dispatch work to Agent, retrieve the Bridge result itself, diagnose failures, and send corrective/retry tasks within the granted scope.

- Correction to 0AQ: The previous interpretation (make Agent notify the phone directly) was wrong. **Do not modify Routine #2 to add `notify_user` for this purpose.** Agent-native notifications are not the requested solution.
- Desired operating loop: ChatGPT prepares task + unique ticket → writes it to the existing `kreavo-bridge` Agent KV route → sends Gmail wake marker → ChatGPT reads result from KV → validates task ID/result → if `ok:false`, diagnose and decide a safe corrective task → repeat as needed → report only meaningful progress/final result to the user. User should not be asked to inspect Agent's response or relay it.
- Current infrastructure verified read-only in this turn:
  - Cloudflare account available; Worker `kreavo-bridge` exists.
  - KV namespace `ab71a201c13f48fcbf533e9b83e6c19d` exists; current namespace key listing returned empty.
  - Deployed Worker has `GET /agent/command` and `POST /agent/result`, authenticated by 40–64 character ephemeral `X-Agent-Ticket`, task/result key prefixes `agent:task:` and `agent:result:`.
  - Agent result payload is validated against matching task ID; Worker stores result and marks task complete.
  - Current Worker code stores result and completed task with **300-second TTL**. Agent task is fetched from KV by ticket.
  - Gmail connector can send wake email to the user's own mailbox; use the established marker `CHATGPT-AGENT` plus ticket.
  - Current KV listing was empty; no pending task or result was found. No task was dispatched in this clarification turn.
- Verified outcome evidence: the 2026-11-17 Google Keep test returned a matching Bridge result to ChatGPT; Agent reported partial execution (`ok:false`, title seen, body not saved/verified). Therefore at least one Agent→Bridge→ChatGPT result return was observed. Do not characterize the Google Keep write itself as success.
- Hard capability limit: ChatGPT can poll/read the Bridge and run retries while actively handling a task in the current turn. ChatGPT cannot keep a live process running or autonomously resume itself after the turn/session ends. Existing scheduled automation supports no more frequent than daily, so it is not real-time task orchestration. Do not promise persistent background monitoring.
- Minimum next engineering improvement to consider: increase task/result retention and add an explicit stage/heartbeat/claim state so a missing result can be distinguished (not triggered vs fetched vs running vs failed-to-post), while preserving the existing Agent app and Routine. Do not change Worker or Routine until the required change is reviewed and explicitly authorized; never weaken ticket authentication or authorization scope.
- User should only be asked for an unavoidable one-time action or explicit authorization for an external/destructive action. For routine safe read-only work, ChatGPT should manage dispatch, retrieval, diagnosis, and retries itself.
- Business scope unchanged: RapiUsaha remains B2C digital products for UMKM.

---

## 0AQ. APPROVED: PHONE NOTIFICATION ON EVERY BRIDGE TASK RESULT — 2026-11-17

**Status: USER_CONFIRMED direction; implementation NOT YET APPLIED.** The user approved direct Android phone notifications whenever an Agent task finishes successfully or fails, so they no longer need to repeatedly open Agent/ChatGPT to check status.

- Active technical project: ChatGPT ↔ Android Agent Bridge. Business boundary unchanged: RapiUsaha remains the primary B2C digital-products business for UMKM.
- Existing verified baseline: Gmail notification → Agent Routine #2 → Cloudflare KV result relay works; first Gmail draft browser write was independently verified. The subsequent Google Keep test was partial: Agent reports title visible, requested body not saved/verified. Do not retry by creating another note.
- New source discovery in upstream `Past-da-king/agent` at commit `8594dfadc3f728d0123990179dfc4361d55dd69c`: Agent already includes `notify_user`, described as “Send the user a phone notification” and intended for background-work results/time-sensitive items. `routineTools()` is built from the helper tool set, and the runtime's routine tool path includes `NotifyTool()`. This supports using Agent's native phone notification; no new app, Worker, KV, or scheduled polling service is needed.
- Proposed minimum change: update existing Routine #2 in place, preserving ID, name, notification trigger/spec, enabled state, and all current bridge/action/safety instructions. Add only a completion/failure notification rule:
  - notify success only after the matching task result was successfully posted to the Bridge; include task ID and a concise outcome;
  - notify failure when ticket extraction/fetch, task execution, or result posting fails; include the stage and concise reason;
  - do not claim success before the result relay confirms it; do not change task authorization boundaries.
- Routine editing must follow Agent's own `routine_list` → retain the complete current prompt → `routine_update(id=2, prompt=<full preserved prompt plus notification clause>)` pattern. Agent source explicitly warns that a cut prompt would lose the rest. Since this ChatGPT session has no remote routine-edit tool, the user must ask Agent's foreground chat to delegate a helper to perform the in-place update.
- After update, read `routine_list` again and verify ID/name/kind/spec/enabled plus full prompt. Then use the existing `notify_user` once as a test notification; verify Android accepts it. Do not modify Worker, Learned App, Gmail trigger, KV namespace, or any other routine.
- Current implementation state: **NOT CHANGED**. No notification has been sent by this proposed setup, and no config was modified in this turn.
- Next step: provide the user one concise instruction to paste into Agent's foreground chat; Agent must inspect and preserve the complete existing prompt before updating it. If it cannot retrieve the full prompt or cannot access `notify_user` from Routine #2, stop and report the exact blocker rather than replacing the routine prompt blindly.

---

## 0AP. GOOGLE KEEP BROWSER WRITE TEST — PARTIAL RESULT — 2026-11-17

**Status: PARTIAL / BODY NOT SAVED OR VERIFIED. The bridge round-trip worked and Agent reports the new note title is visible, but the requested body is absent. Do not count this as a successful note-write test.**

- User explicitly approved exactly one new Google Keep note through Agent's browser UI; no sharing or changes to other notes.
- Task ID: `bridge-action-keep-note-20261117-01`.
- Target: Google Keep, Agent's Personal browser profile.
- Requested title: `[TES BRIDGE] Catatan dibuat oleh Agent`.
- Requested body: `Ini adalah catatan uji koneksi ChatGPT Bridge. Catatan ini dibuat oleh Agent melalui browser UI dan tidak perlu dibagikan kepada siapa pun.`
- Restrictions: create/save one new note only; do not share it; do not edit, archive, or delete any other note.
- Cloudflare KV returned the matching task ID with `ok:false` at `2026-11-17T15:18:53.379Z`. Agent's exact report: it opened Keep in Personal (`christianomomongan82@gmail.com`), created one note with the exact title, and saw that title persisted/found by search; however, the requested body was not visible in the note/result and Agent could not verify or save it. Agent stopped without modifying, sharing, archiving, or deleting any other note.
- The task payload in KV matches the exact authorized title/body and the result ID matches the dispatched task. This verifies command delivery and result relay, not full destination persistence.
- No independent Google Keep connector/readback is available in this session; title persistence is currently Agent-reported, not independently inspected by ChatGPT.
- No Worker, Learned App, or Routine configuration was changed.
- **Next:** do not dispatch another create task (that risks duplicate notes). Ask the user whether to authorize one narrowly scoped follow-up: open the already-created note with this exact title and set its body to the exact text above, without touching any other note. Until approved, do not edit it.
- Scope remains unchanged: ChatGPT ↔ Android Agent bridge is the technical project; RapiUsaha remains the primary B2C digital-products business for UMKM.

---

## 0AO. FIRST VERIFIED AGENT BROWSER WRITE ACTION — 2026-11-17

**Status: SUCCESS. The existing ChatGPT ↔ Android Agent bridge completed its first verified browser UI write action.**

- Task ID: `bridge-action-draft-20261117-48char`.
- Cloudflare result key returned `ok: true`, matching task ID, and Agent reported browser UI creation and save in Gmail Personal.
- ChatGPT independently read Gmail Drafts after receiving the result and verified exactly one matching test draft:
  - To: `christianomomongan82@gmail.com`
  - Subject: `[TES BRIDGE] Draft dibuat oleh Agent — jangan dikirim`
  - Body: `Ini adalah draft uji koneksi ChatGPT Bridge. Draft ini dibuat oleh Agent melalui browser Gmail dan TIDAK boleh dikirim.`
- Draft ID: `r63581173553420803`.
- Gmail Drafts readback confirms it is saved. It was not sent. Existing unrelated drafts remain unchanged.
- The new task used a 48-character ticket and 24-hour TTL. Cloudflare showed task status `complete` and the result record; this verifies the end-to-end wake → ticket fetch → browser UI action → result relay.
- Root-cause correction: the prior 44-character ticket was incompatible with the Agent routine's expected 48-character ticket format even though its KV record was still present. The format correction resolved the repeated ticket-not-found/expired symptom.
- No Worker or Learned App code was changed. Existing Routine #2 and bridge infrastructure were sufficient once the ticket format was corrected.
- Next step: preserve this successful configuration as the baseline. Do not change the Worker, Learned App, or routine unnecessarily. For future tasks, use exact 48-character tickets, sufficiently long TTL, explicit task IDs and per-action authorization. Continue verifying results in the target app before reporting success.
- Scope remains unchanged: Android Agent ↔ ChatGPT bridge is the technical project; RapiUsaha remains the primary B2C digital-products business for UMKM.

## 0AN. TICKET FORMAT CORRECTED; FRESH ACTION TASK PENDING — 2026-11-17

**Status: fresh 48-character task is stored and its Gmail wake message is verified delivered; Agent result has not arrived yet. No test draft exists yet.**

- Root-cause finding: the immediately previous task was issued with a **44-character** ticket, while the bridge's established ticket format in the earlier successful round-trip is **48 characters**. Its KV key was still present and unexpired when Agent reported “ticket expired/task not found”. Therefore the old error did not reflect actual KV expiry; most likely the Agent routine's ticket extraction/format contract rejected or misread the 44-character value. This diagnosis is strongly supported by the observed mismatch but remains pending confirmation from the next successful fetch.
- The 44-character task and any matching result key were deleted to prevent a stale/duplicate command from running.
- A new 48-character ephemeral ticket was generated. It is intentionally omitted from GitHub. New task ID: `bridge-action-draft-20261117-48char`.
- New task expiry: 86,400 seconds (24 hours). Cloudflare KV readback confirmed the exact task payload exists, status `pending`, and no result key exists yet.
- A new wake email was sent to the user's own Gmail. Message ID: `1a1ea0ce20ea2bd6`. Gmail readback confirmed the exact `CHATGPT-AGENT-<48-char-ticket>` marker in both subject and body, and labels `UNREAD`, `SENT`, `INBOX`.
- Exact authorized action remains: create one Gmail draft in `christianomomongan82@gmail.com`, addressed to the same account, subject `[TES BRIDGE] Draft dibuat oleh Agent — jangan dikirim`, with the explicit test body in the task. Do not send the draft or alter any other mail.
- Gmail Drafts readback still shows only the user's three pre-existing unrelated drafts; no matching test draft exists. Those drafts were not changed.
- A subsequent Cloudflare bulk-get again confirmed the new task is `pending` and no result is present. Current status: **DISPATCHED / PENDING; action not yet verified**.
- Next: wait for Agent to fetch this exact 48-character ticket and return the matching task ID; then immediately bulk-get the result and verify the exact saved draft in Gmail Drafts. Do not create another task while this one remains pending.
- No Worker or Learned App configuration has been changed. RapiUsaha remains the active B2C digital-products business for UMKM.

## 0AM. GMAIL LOGIN VERIFIED BY AGENT REPORT; FRESH DRAFT TEST DISPATCHED — 2026-11-17

**Status: Agent reported Gmail Inbox open and its Personal browser session logged in. This is user-pasted Agent output, not independent browser inspection by ChatGPT.** After receiving that report, ChatGPT dispatched one fresh, explicitly authorized test through the existing bridge:
- Task ID: `bridge-action-draft-20261117-loginverified`.
- Intended effect: create exactly one draft in the user's own Gmail account, To `christianomomongan82@gmail.com`, subject `[TES BRIDGE] Draft dibuat oleh Agent — jangan dikirim`, body states it is a bridge test and must not be sent.
- Explicit restrictions: do not send; do not modify/delete any other email or draft; use Agent's Gmail browser UI, not the Gmail connector.
- Task stored in existing KV namespace under an ephemeral ticket with 3600-second TTL. **The ticket is intentionally not recorded in GitHub.**
- Wake notification email sent to the same mailbox; Gmail send returned message ID `1a1ea08520baf9f5`.
- Immediate Cloudflare readback confirmed the task record is present and `status=pending`; no matching result record existed at that read.
- Gmail Drafts readback showed only three pre-existing unrelated drafts; the requested test draft was not present yet. Existing drafts were left untouched.
- Current status: **DISPATCHED / PENDING AGENT RESULT; draft creation NOT VERIFIED.** Poll for the matching result and inspect Gmail Drafts before claiming success. Do not send the draft.
- Next step: retrieve `agent:result:<ticket>` via KV bulk-get while TTL is active; verify matching task ID and result; then inspect Gmail Drafts for exact subject/body. If the Agent routine does not fire, diagnose notification delivery/routine execution before creating another task.
- Project scope unchanged: this is the ChatGPT ↔ Android Agent technical bridge. RapiUsaha remains the primary B2C digital-products business for UMKM.

## 0AL. USER REQUESTED REAL ACTION CAPABILITY — COMMAND FETCH VERIFIED; AGENT BROWSER GMAIL LOGIN REQUIRED — 2026-11-17

**Status: ROUTINE #2 prompt reported updated and active. First test expired at 5 minutes. The 1-hour retry successfully fetched the task, but Agent stopped before action because Gmail in Agent's own browser profile requested sign-in. No draft was created or email sent. Next step: one-time Gmail sign-in in Agent browser via foreground browser handoff; then dispatch a fresh action test.** The user provided a screenshot of Agent confirming the prompt update, while name, Gmail notification trigger/spec, and active status were retained. This is user-provided screenshot evidence, not independent routine-list readback. The tested Gmail/Cloudflare bridge remains connected; action execution capability is now enabled by the routine prompt, but no real external write/action has yet been verified.

- Upstream Agent source was inspected. `AgentRuntime.workerTools()` provides browser, learned-app, connected-app and Android tools to routines (screen tools only when Background screen is enabled).
- Browser has actual action tools: `browser_click`, `browser_type`, `browser_press_key`, `browser_upload`, plus browser navigation. These can interact with website fields/buttons, upload files, and submit UI flows. No new app/Worker is required for this browser execution path.
- **Background approval constraint:** routines run with `ChatContext("routine", interactive=false)`; `ctx.ask()` returns null, so a routine cannot wait for an in-app Approve/Decline card. Connected-app `AppGuard` enforces per-account read/change rules; default change rule is Ask me. In background, Ask me writes are refused because no user is present; Never is a hard refusal. Never route around these denials. User may optionally set a specific connected account's change rule to Allowed for API actions, but do not change account rules automatically.
- **Correct action authorization design:** ChatGPT acts as the authorization gate and only dispatches externally consequential actions when the current user's instruction explicitly specifies the exact action, target/account, and content/effect (or the user has approved an exact proposal in this conversation). This is per-action authorization, not blanket future consent. Routine may prepare an action and return its details when authorization is absent, but must stop before final submission.
- Routine #2 prompt update: Agent reported success in the screenshot supplied by the user; name, trigger/spec, and active state reportedly retained. First Gmail draft test expired at 300 seconds. The 3600-second retry fetched its command successfully, then returned `ok:false` because Gmail in Agent's browser asked for login. No draft was created or email sent. Upstream Agent source confirms `browser_handoff` is the intended one-time user sign-in flow and that browser sessions persist for next time. Login must be completed in Agent's own browser profile through foreground Agent chat; then dispatch a fresh test. See `research/chatgpt_agent_action_execution_2026-11-17.md`.
- **No Worker change is needed.** Existing Learned App, explicit ticket args, Trust state, Gmail notification permission and Cloudflare routes remain as verified.
- **One local sign-in action remains:** in Agent's foreground/main chat, open `https://mail.google.com/` with `browser_open` and the correct browser profile (Personal), then use `browser_handoff` so the user can sign into Google directly in Agent's browser. Do not ask the user to disclose password or 2FA to ChatGPT. After Agent confirms inbox is visible, dispatch a fresh one-hour draft test.
- Full source findings and proposed action policy: `research/chatgpt_agent_action_execution_2026-11-17.md`.
- No real external write/action has yet been verified through Agent. The 1-hour retry fetched the command but stopped at Gmail sign-in; no draft exists. Do not claim success until a fresh matching result and saved draft are confirmed. Checkpoint: `research/chatgpt_agent_action_execution_2026-11-17.md`.
- Business boundary unchanged: RapiUsaha remains the primary B2C digital-products business for UMKM. No other project or business strategy changed.

## 0AK. CHATGPT ↔ AGENT BRIDGE — FIRST FULL READ-ONLY ROUND-TRIP VERIFIED — 2026-11-17

**Status: CONNECTED for short read-only research/browsing tasks.** On 2026-11-17, ChatGPT successfully dispatched a task through Gmail notification, Agent executed it, and ChatGPT retrieved the matching result from Cloudflare KV. This is a working relay bridge (Gmail + Cloudflare Worker), not a native MCP connection.

- User confirmed existing Learned App `ChatGPT Bridge` and Routine #2 were updated. Learned App Trust remained `user trusts its changes`. Routine #2 trigger: `notification`, `com.google.android.gm|CHATGPT-AGENT`. Its updated prompt passes the ephemeral ticket as an explicit argument to both tools; no use of `location.hash` or ticket-in-URL.
- Test task ID: `bridge-test-20261117-766cfd9a7b4f`.
- New 48-character ephemeral ticket stored in `agent:task:<ticket>` (300-second TTL). Ticket deliberately omitted from this public checkpoint.
- Gmail wake email sent to the same connected mailbox, message ID `1a1e90730d16a99a`. Gmail metadata verified labels `UNREAD`, `SENT`, `INBOX`; subject AND body contained the unique `CHATGPT-AGENT-<ticket>` marker.
- Agent successfully browsed `https://example.com/` and returned: title `Example Domain`, H1 count `0`, final URL `https://example.com/`, no other action.
- Cloudflare KV POST `/bulk/get` retrieved the result:
  - received at `2026-11-17T10:22:53.391Z`
  - ID `bridge-test-20261117-766cfd9a7b4f` (exact match)
  - `ok=true`
  - result text: `Judul: Example Domain. Jumlah H1: 0. URL akhir: https://example.com/. Tidak ada tindakan lain.`
- A second bulk-get confirmed task record status `complete` and the corresponding result. Both test keys were deleted; final KV key listing with prefix `agent:` returned an empty array.
- One individual KV GET returned 404 even while key listing showed the result; POST `/bulk/get` successfully retrieved the value. Use bulk-get for readback through the current Cloudflare connector.
- Root cause of the previous failed callback: Agent's `EnginePageRunner.ensure()` navigates back to the Learned App's static start URL after browsing an external domain. The earlier tools depended on `location.hash`; this lost the ticket after visiting example.com. Updated tools now accept `ticket` explicitly. No Worker redeploy was needed.
- Agent NotificationReader de-duplicates notifications by package/title/text within a 10-minute lookback. To avoid an identical wake notification being suppressed, each wake email includes the unique marker in both subject and body.
- **Scope proven:** short read-only browsing/research. This test does NOT authorize or validate sending, posting, purchasing, deletion, account changes, or form submission. Keep the routine's read-only restrictions.
- **Limits:** ephemeral task/result TTL 300 seconds; ChatGPT must retrieve results promptly. Durable long-running queue delivery is not implemented.
- Full evidence/diagnosis: `research/chatgpt_agent_bridge_roundtrip_bug_2026-11-17.md`.
- Project boundaries unchanged: RapiUsaha remains the primary B2C digital-products business for UMKM. No other project or business was changed.

## 0AJ. AGENT BRIDGE — TASK EXECUTION WORKS; RESULT TICKET LOST WHEN BROWSER LEAVES KREAVO — 2026-11-17 (SUPERSEDED BY 0AK)

**Historical status at this checkpoint: PARTIAL ROUND-TRIP VERIFIED; NOT YET CONNECTED. Superseded by successful test 0AK below.** The Gmail notification permission issue recorded in 0AI has been resolved by the user. The second test reached Agent: Agent reported opening https://example.com/, title "Example Domain", no H1, and final URL https://example.com/. Agent then reported result submission failed because the ticket could not be found. This proves the Gmail notification and routine fired and the read-only task executed; the callback still failed.

- **Root cause confirmed from current Agent source:** `NotificationReader.kt` passes notification title/text into routine context; `Routines.kt` prepends that context to the routine prompt. But `WebAppTools.kt`'s `EnginePageRunner.ensure()` reopens the Learned App's static `startUrl` whenever the browser has left the learned app's host. The task navigates to example.com; before a later Learned App call, Agent reopens plain `https://www.kreavo.id`, losing the ephemeral ticket stored only in `location.hash`. This matches the reported "ticket not found" at result send.
- **Fix:** update the current Learned App tools to require an explicit ephemeral `ticket` argument. Both `get_command(ticket)` and `send_result(ticket,id,ok,result)` must read the ticket from arguments and place it in `X-Agent-Ticket`; neither tool may depend on `location.hash`.
- Update the existing Gmail routine prompt to extract the 40–64 character URL-safe ticket from notification context (subject marker `CHATGPT-AGENT-`) and pass it explicitly to both tools. Keep the existing strict read-only scope.
- **No Cloudflare Worker change/redeploy is needed.** The existing `/agent/command` and `/agent/result` routes already accept the ticket header. No current task/result remains active; do not enqueue or email another test until the Agent tool definitions are updated.
- Existing Learned App and routine do not need to be recreated, only updated. The browser may still open Kreavo for origin, but must not use its URL fragment as the ticket store.
- Canonical detailed diagnosis: `research/chatgpt_agent_bridge_roundtrip_bug_2026-11-17.md`.
- Previous 0AI Gmail-permission blocker is superseded: user later enabled Gmail notifications and the second task demonstrably reached Agent.
- RapiUsaha remains the primary B2C digital-products business for UMKM. This technical bridge work does not change business direction or any other project.

## 0AI. AGENT BRIDGE BLOCKER IDENTIFIED — GMAIL NOTIFICATIONS NOT ALLOWED — 2026-11-17

**Status: Learned App saved and trusted; notification routine recorded; Gmail notifications not allowed in Agent, so bridge remains NOT CONNECTED.** User-provided Agent diagnostics confirm both learned tools exist and Trust is on. Routine exists with kind=notification and spec=com.google.android.gm|CHATGPT-AGENT; diagnostic did not expose enabled/paused state. notifications_read explicitly reports no apps allowed.

- Root cause for first live-test failure: Agent has no allowed notification apps, so Gmail notification events are not read and the notification routine cannot fire.
- Minimum user action: Agent > Connections > Notifications > allow Gmail; grant Android Notification Access when prompted.
- Once allowed, do NOT expect the earlier wake email to fire retroactively. Agent NotificationReader.onListenerConnected handles existing active notifications with live=false; notification routines fire only for live incoming notifications. Send a fresh wake email after the permission is enabled.
- No need to recreate Learned App, re-enable Trust, or recreate the routine. Do not issue a second task/email until user confirms Gmail notification access is enabled.
- Full diagnostic and source-backed behavior: research/chatgpt_agent_bridge_2026-11-14.md.

## 0AH. AGENT BRIDGE LIVE TEST 1 TIMED OUT — 2026-11-17

**Status: NOT CONNECTED.** Learned App and notification routine were reported stored by the user's Agent screenshot; user then reported enabling Trust. A read-only example.com task was queued and a Gmail wake-up email was sent to the user's own mailbox. Gmail confirmed the message was UNREAD + INBOX. No result arrived within the task's 300-second TTL; task expired and KV namespace returned to empty. This does not prove whether the routine failed to fire or the Agent failed later in the chain because command fetch is not currently logged. Do not claim success or send another test until Agent's routine_list lastRunAt/lastResult and webapp_list trust state are inspected.

- Gmail message ID: 1a1e8d00939640fb.
- Task ID (non-secret): bridge-test-20261117-a06783cd23a9.
- Full evidence: research/chatgpt_agent_bridge_2026-11-14.md.
- Existing Worker remains deployed with Agent routes and old Kreavo routes preserved. No other Cloudflare Worker/project changed.

## 0AG. AGENT ROUTINE TOOL VISIBILITY DIAGNOSED — 2026-11-14

**Status: Learned App save awaits approval/verification; notification routine is not yet created; end-to-end round trip not yet tested.** Current user-reported Agent response says tools passed local simulation, app save submitted for approval but not confirmed, and routine tool unavailable in that session.

- Agent source shows this is expected main/helper separation, not absence of the feature: main-agent tool selection excludes routine_create; main-agent prompt tells it routines must be created by a helper; helper worker tools contain ScheduleCreateTool registered as routine_create.
- Next instruction: main Agent must use delegate to ask a helper to call routine_create, then verify using routine_list. It must not call routine_create itself.
- The app's webapp_save requires a distinct approval interaction and only reports 'Saved...' after approval; use webapp_list to establish persistence.
- No Cloudflare task/ticket has been created and no Gmail wake email has been sent yet.
- Detailed implementation and current Worker deployment status: research/chatgpt_agent_bridge_2026-11-14.md.

## 0AF. CHATGPT ↔ AGENT ANDROID BRIDGE — EPHEMERAL TICKET ROUTES DEPLOYED; ROUND TRIP PENDING — 2026-11-14

**Status: PARTIAL IMPLEMENTATION VERIFIED.** The current task is the technical bridge between ChatGPT and the user's Android Agent app. It does not replace or alter the primary business. **RapiUsaha remains the primary B2C digital-products business for UMKM.**

- User has now explicitly granted the assistant authority to carry out the bridge work. This authorizes bounded technical changes to the existing `kreavo-bridge` Worker; it is not blanket approval for unrelated account, business, Worker, or financial changes.
- Existing Worker `kreavo-bridge` was updated only by adding separate Agent endpoints `GET /agent/command` and `POST /agent/result`, protected by an ephemeral per-task `X-Agent-Ticket` capability. No long-lived Worker secret is embedded in the Learned App definition.
- Deployment returned ID `0ecddcd0152a4c08be47c35c2254d043`; deployed script tag `3b5f08489c874b27a873a27455fc62aa`. Re-read confirmed new routes and legacy Kreavo routes both remain in the script; `BRIDGE_KV` binding and compatibility date are preserved. KV namespace was empty on final inspection.
- The old Kreavo browser bridge's `/client/command`, `/client/result`, `/client/snapshot`, `/install`, and `/health` code remains present. Avoid running its bookmarklet alongside Agent until legacy queue use is serialized.
- Learned App should use ID `chatgpt_bridge`, start URL `https://www.kreavo.id`, a read-only `get_command` and one narrow result-post tool `send_result`. The dynamic ticket is read from browser URL fragment `#agent=<ticket>` and sent as request header at runtime; no static ROOM token in the tool definition.
- Agent source confirms notification trigger format `com.google.android.gm|CHATGPT-AGENT` and has `routine_create` for notification events. The routine must open the Kreavo URL including the ticket fragment, fetch one task, do read-only work only, then post its result.
- **Still pending:** the user must approve saving the Learned App, Trust only the ChatGPT Bridge app, allow Gmail notifications/Android notification access, and create the notification routine. Then ChatGPT must run and retrieve one harmless end-to-end test. No connection success may be claimed before this round trip succeeds.
- Full technical record and verification detail: `research/chatgpt_agent_bridge_2026-11-14.md`.
- No RapiUsaha, Projects.co.id, RentalOps, SignageQuote, SiapKirim, or Backtest project state was changed.

## 0AE. PROJECTS.CO.ID — PROFILE UPDATE SAVED AND RELOAD-VERIFIED — 2026-11-13

**Status: VERIFIED SAVED.** The user explicitly authorized improving the Projects.co.id profile while leaving the photo unchanged. An authenticated browser submitted the profile form, then reloaded the edit page and confirmed the server-persisted values.

- **Display Name:** changed from `tianmmngn` to `Tian`.
- **Self Introduction:** replaced the blank field with a clear Indonesian description of the three existing spreadsheet services for UMKM: data input, basic data cleaning, and a simple sales-summary template. The full copy is recorded in `research/projects_co_id_profile_update_2026-11-13.md`.
- **Language:** selected `Indonesia`.
- **Skills:** after cleanup and reload, only `Data Entry` is confirmed selected. No irrelevant skill remains selected in the final verified result. The final run could not select `Microsoft Excel` or `Google Sheets` as exact skill labels; an earlier run reported seeing those labels, so their availability evidence is inconsistent and must not be represented as definitively unavailable. Do not claim any unselected skills.
- **Preserved:** Photo/avatar (user said they must supply the image themselves), Gender = Pria, Notify Me New Projects = On, private/contact/location fields, website, and portfolio.
- **Not touched:** all three published service listings, prices, descriptions/options, bids, messages, orders, transactions, payment settings, password, email, and phone.
- **Verification evidence:** authenticated browser opened the profile edit page, clicked `Save Profile`, reloaded the edit page, and confirmed Display Name `Tian`, Language `Indonesia`, the complete Self Introduction, and the final Skills selection `Data Entry` persisted server-side.
- **Project hierarchy unchanged:** RapiUsaha remains the primary business (B2C digital products for UMKM); Projects.co.id remains a supplementary freelancer-income channel. This was a profile-only account update, not a business or service pivot.
- **Next step:** the user can add their chosen profile image separately. Do not edit service listings or other account sections without a new explicit instruction.

## 0AD. PROJECTS.CO.ID — PROFILE REWRITE PREPARED; SAVE NOT CONFIRMED — 2026-11-13 (SUPERSEDED ATTEMPT LOG)

**Historical status only: OPEN / NOT SAVED at the time. Superseded by verified save in section 0AE.** The user explicitly asked the assistant to improve their Projects.co.id profile. A read-only audit found that public profile fields for display name, self-introduction, language, and skills were blank/generic. The proposed rewrite is restricted to accurately describing the three existing spreadsheet services; it does not claim employment history, clients, completed jobs, certifications, or expertise.

- User-authorized scope: improve the user's profile only. Do not edit services, bids, messages, portfolio, payment, or unrelated account settings.
- Proposed Display Name: `Tian`.
- Proposed Self Introduction is recorded in `research/projects_co_id_profile_update_2026-11-13.md`.
- Language option found: `Indonesia`.
- Skills selector: `Data Entry` was available; no verified choices found for Microsoft Excel, Google Sheets, Data Cleaning, Spreadsheet Formatting, or Sales Reporting in the attempted selector search.
- First browser edit run reported success, but an independent read-only reload showed the profile unchanged (Display Name still `tianmmngn`, Self Introduction blank, Language unselected, no Skills). Treat the first success report as incorrect.
- A second run filled Display Name, Self Introduction, and Language and searched for Data Entry, but stopped before selecting the skill or clicking `Save Profile`; no server confirmation or reload verification occurred. Therefore **no profile change is confirmed saved**. The browser form may have unsaved values; its current client-side state is unknown.
- No unrelated personal/contact fields, avatar, website, location, or portfolio were changed.
- Do not tell the user the profile is improved/live until the form is explicitly submitted and a fresh reload confirms the server-persisted values.
- Next step: when an authorized browser automation session is available, finish only the approved profile save, then reload and verify public profile display. Do not repeat login setup just because the browser-profile registry omits `projects.co.id`; prior read-only browser access worked.
- RapiUsaha remains the primary business (B2C digital products for UMKM); Projects.co.id remains a supplemental track. This profile-only request does not authorize broader changes to the business or services.

## 0AC. PROJECTS.CO.ID — LIVE THREE-SERVICE READ-ONLY AUDIT COMPLETED — 2026-11-13

**Status: VERIFIED BY AUTHENTICATED BROWSER READ.** User confirmed they had completed the profile-save/login step. Although the profile registry still showed no `signed_in_sites` entry for projects.co.id, a browser run using the same Default profile opened the account's `/user/my_services` pages and explicitly reported the account as logged in. The registry flag was therefore not a reliable blocker in this case; do not make the user repeat setup solely because that flag is empty. Audit was read-only; no account/listing changes, messages, bids, or transactions were made.

- Account handle shown by the authenticated page: `tianmmngn`.
- All three services show status `Registered`.
- Service 01: [Jasa Input Data ke Excel / Google Sheets hingga 100 Baris](https://projects.co.id/public/browse_services/view/7dcf13/jasa-input-data-ke-excel-google-sheets-hingga-100-baris) — base Rp50,000; up to 100 rows / 5 columns; 2 days; registration 13/11/2026 10:34:41 WIB.
- Service 02: [Jasa Cleaning Data Excel: Hapus Duplikat & Cek Data Kosong](https://projects.co.id/public/browse_services/view/89cf13/jasa-cleaning-data-excel-hapus-duplikat-amp-cek-data-kosong) — base Rp75,000; up to 300 rows / 10 columns, one source file and one sheet; 3 days; registration 13/11/2026 16:01:20 WIB.
- Service 03: [Jasa Pembuatan Template Rekap Penjualan Excel Sederhana](https://projects.co.id/public/browse_services/view/8ccf13/jasa-pembuatan-template-rekap-penjualan-excel-sederhana) — Rp125,000; workbook includes guide, input sheet up to 500 transactions, 12-month summary, chart, one light revision; 4 days; registration 13/11/2026 16:31:39 WIB.
- Exact scope/exclusions and full observed details are recorded in `research/projects_co_id_live_listing_audit_2026-11-13.md`.
- At audit time all three showed zero sales/count and Rp0 amount, no rating, zero points/ranking, and no inquiry/order/review activity displayed. Dashboard aggregate metrics were not separately displayed. Treat zero as the displayed state at audit time, not as a claim about historical exposure/impressions.
- This audit does not change the user's existing decision to pause further bids/account/listing changes/expansion until 25 Dec 2026 unless the user instructs otherwise. RapiUsaha remains the primary business (B2C digital products for UMKM); Projects.co.id remains supplemental.
- No assistant-side modifications or commercial actions occurred.

## 0AB. PROJECTS.CO.ID — THREE SERVICES PUBLISHED; FURTHER ACTION PAUSED — 2026-11-13

**Status: USER_CONFIRMED PUBLISHED.** The user explicitly corrected the audit: all three prepared Sell Services (data entry, data cleaning, and sales-summary template) have already been published by the user. This supersedes previous notes that treated all three as drafts. This statement was superseded by section 0AC: the live account pages and listing details were subsequently inspected in an authenticated read-only browser audit on 2026-11-13. See `research/projects_co_id_live_listing_audit_2026-11-13.md`. Do not claim to have checked the pages. Do not unpublish, delete, or change the existing listings without the user's instruction.

- **Project hierarchy:** RapiUsaha remains the primary business: B2C digital products for UMKM. Projects.co.id remains a separate supplemental freelancer-income track. No change to either business model.
- **User instruction about timing:** the user said they would act more freely after turning 18 on 25 December 2026 and asked to audit first. The three existing listings remain published; the pause applies to new bids, account/listing changes, and expansion until the user gives a new instruction. Any incoming order or message before then must be assessed separately; do not accept or promise delivery automatically.
- **Market verdict:** Projects.co.id is suitable only as a supplementary channel to test, not a dependable main income source yet. On 2026-11-13 public pages showed approximately 130–135 open projects and 8 Data Entry projects, against 1,756 Data Entry services. Listings are not proof of purchases; actual order frequency/conversion remains unknown. Prior relevant bid snapshots were high (around 92–224 bids).
- **Three services user-confirmed published:** (1) data entry/input, (2) data cleaning, (3) Excel sales-summary template. Earlier working drafts listed proposed prices of Rp50k, Rp75k, and Rp125k respectively, but these are not confirmed as the exact live prices. Confirm from public URLs before using those amounts in new calculations or advice.
- **Sample readiness:** demo workbooks use synthetic data. Service 02 and Service 03 samples still need end-to-end Android QA. The packaging-reject demo workbook has incomplete visual QA and must not be used in a bid until corrected.
- **Policy discrepancy:** Terms wording says individuals must be "over 17", while a separate clause refers to users under 17. The user has chosen to wait until 25 Dec to act more freely. Service fee is 12%. For open Projects, FAQ Worker says 12% at all budgets while Pricing says free at Rp50k–Rp100k and 12% above Rp100k; use conservative 12% until clarified.
- **Not verified:** live listing URLs and final listing configuration; impressions/views; inquiries; orders; messages; reviews; sales; current account status.
- **No assistant-side activity claimed:** the assistant did not submit these listings. No bid has been submitted by the assistant in this track.
- **Audit:** `research/projects_co_id_readiness_audit_2026-11-13.md`.
- **Next prep:** identify and inspect the three live public pages, record actual live settings and current listing metrics if visible, QA the samples on Android, and measure completion times. Do not publish additional services or bid until the user authorizes the next step.

---

## 0Z. RAPIUSAHA — VALIDASI PRODUK PUTARAN 2 — 2026-11-13

**Status: RISET ASISTEN SELESAI; tidak ada produk atau niche baru yang dikunci pengguna.** Semua verdict pada audit ini adalah ASSISTANT_PROPOSAL, bukan USER_CONFIRMED.

- **Arah bisnis tetap:** RapiUsaha — produk digital B2C untuk UMKM (USER_CONFIRMED). Projects.co.id tetap kanal pemasukan tambahan yang terpisah.
- **Home Daycare Launch / Parent Onboarding Pack: HOLD.** Ada bukti item-level demand (listing 275 halaman 93 review, rating 4.8/5; listing pesaing lain 69 review, rating 4.8/5), tetapi kategori Etsy menampilkan 5.000+ items. Solusi serupa sudah hadir dalam Canva, Word, Google Docs dan fillable PDF. Produk fillable PDF yang ditinjau bahkan meminta pengisian via komputer karena sebagian phone/browser viewer tidak mendukung field. Perbedaan format/lebih ringkas/mobile-friendly belum menjadi gap unik yang tervalidasi. Ada risiko legal/regulasi/medical untuk daycare forms. Jangan buat produk sekarang.
- **Kalkulator margin seller marketplace Indonesia: NO-GO generik.** ASEAN Seller sudah menyediakan spreadsheet gratis Shopee/Tokopedia/Lazada dengan BEP dan simulasi campaign; MahirSeller, TakarLaba dan iBuat.Link menyediakan kalkulator browser gratis. Fee marketplace bervariasi dan dapat berubah sehingga produk statis juga menuntut maintenance.
- **Vending machine route/profit tracker: WATCHLIST, bukan build.** Ada produk Etsy dengan 451 reviews dan 84 reviews, tetapi VendingHQ mengklaim paket gratis unlimited machines dengan P&L/route/service logs; RouteStock gratis untuk 2 mesin; VendSoft punya alternatif berbayar mulai US$19/bulan. Paid demand ada, tetapi pengganti gratis memenuhi inti fungsi dan niche ini belum jelas cocok untuk basis target RapiUsaha di Indonesia.
- **Florist / flower-shop tracker: NO-GO generik.** Ada produk Etsy dengan 34 item reviews dan harga sekitar US$14.49; Evoflor memasarkan paket gratis hingga 100 orders/bulan dan 2 event/bulan dengan order/customer/recipe-costing functions.
- **Beauty salon tracker: NO-GO generik.** Produk sejenis punya review Etsy hingga sekitar 3.4k, tetapi hasil pasar padat dan OneStellara, SalonHQ, Pronto menawarkan software gratis dengan kombinasi booking, customer, inventory dan finance.
- **Distribusi/payout:** Kreavo Terms menyebut usia minimal 17, pembayaran QRIS kecuali metode lain muncul pada checkout, saldo ditahan 7 hari, minimum withdraw Rp100.000 dan proses 1–3 hari kerja; Help Center menyebut QRIS/VA/e-wallet/retail dan biasanya 1–2 hari kerja. Homepage menampilkan “0+ seller dan 0+ produk digital”. Perbedaan Terms/Help/marketing wajib diuji pada checkout aktual; jangan menganggap Kreavo sudah punya discovery audience. Etsy tetap diblokir sampai pengguna berusia 18 pada 25 Desember 2026 jika tanpa wali.
- **Putusan sementara:** belum ada kandidat yang lolos seluruh filter (item-level payment evidence, gap yang bertahan dari software gratis, phone-first, biaya produksi Rp0, self-service, channel dan payout yang bisa dipakai sekarang). Jangan membuat produk/iklan/listing hanya untuk terlihat maju.
- **Langkah berikutnya:** lanjutkan riset peluang dari kebutuhan UMKM yang relevan di Indonesia dan fokus pada workflow spesifik yang belum diselesaikan alternatif gratis; produk, niche, harga dan marketplace tetap OPEN sampai bukti cukup dan pengguna menyetujui.
- **Laporan dan bukti:** `research/RapiUsaha_product_validation_round2_2026-11-13.md` (commit `d14be783f22e09ab3ced81b8b44a8df0195649a7`). [Buka laporan](https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa/blob/main/research/RapiUsaha_product_validation_round2_2026-11-13.md).
- **Tidak dilakukan:** tidak ada akun baru, perubahan akun, listing, produk, iklan, penjualan, pembelian, atau pembayaran.

---

## 0Y. PROJECTS.CO.ID — SERVICE 02 DATA CLEANING PREPARED — 2026-11-13

**Catatan status terkini — lihat 0AB:** bagian di bawah adalah arsip persiapan Service 02. Pengguna telah mengonfirmasi ketiga layanan sudah diterbitkan. Status draft/belum terbit pada bagian historis ini tidak lagi berlaku. Rincian halaman live belum diverifikasi.

**Status historis saat catatan ini dibuat: ASSISTANT_PREPARED / DRAFT READY.** User instructed to create the most important next step after agreeing to expand income. The first priority chosen was a higher-value, clearly bounded spreadsheet cleaning service. Specific price/options remain an assistant proposal; no account listing has been published.

- **Project context:** supplementary Projects.co.id freelancer income track. The primary business direction remains RapiUsaha — B2C digital products for UMKM. Do not merge or replace the main business with a service agency.
- **Why this service first:** existing entry service is basic data entry at Rp50,000. Cleaning + quality-check summary offers a clearer higher-value step while reusing the same spreadsheet skill family. Current public competitor examples range from Rp50,000 for advertised large-batch light cleaning to Rp150,000 for around 250 rows with a basic dashboard and Rp200,000–250,000 for 300-row cleaning/data support. Prices are offers, not proven sales. Therefore use a competitive Rp75,000 starter, not the earlier Rp125,000–175,000 suggestion, and differentiate via explicit QA summary rather than unsupported volume claims.
- **Prepared Sell Service draft:** title: Jasa Cleaning Data Excel: Hapus Duplikat & Cek Data Kosong. Base price Rp75,000; finish time 3 days; maximum 300 rows × 10 columns, one source file and one sheet. Includes duplicate removal based on agreed key, trimming excess spaces, standardizing text/date/number formats, flagging empty/incomplete data, sorting/table cleanup, QA summary, XLSX/CSV output, and one minor in-scope revision.
- **Options (proposed, multiple choice):** additional up to 300 rows (maximum total 600) +Rp50,000; one additional sheet in the same file (maximum 300 rows) +Rp25,000.
- **Exclusions:** no web research/scraping, scanned image/PDF entry, guessing missing values, complex formulas, pivot/dashboard, macros, API integration, or external validation that email/phone numbers are active. Missing values are flagged for client confirmation. Duplicate removal key is agreed before work begins.
- **Tags (choose only those actually available in the marketplace selector):** Data Cleaning, Microsoft Excel, Data Processing, Data Entry, Spreadsheet.
- **Net calculation:** Rp75,000 less 12% Service fee = Rp66,000 before bank withdrawal fee/tax/working cost.
- **Created workbook:** /mnt/data/Contoh_Pembersihan_dan_Pemeriksaan_Kualitas_Data.xlsx. Contains Panduan, Data Awal, Data Bersih, and Ringkasan QA sheets. The example has 28 input rows, 25 unique IDs after removing 3 duplicates, 1 missing contact, 1 missing email, and 1 missing category. All records are synthetic, not real client data. Summary formulas were inspected; scan found no formula errors. Visual QA preview inspected.
- **Copy-ready text:** /mnt/data/Projects_co_id_Service_02_Cleaning_Data.txt.
- **Detailed copy and research:** research/projects_co_id_service_02_data_cleaning_2026-11-13.md.
- **User correction retained:** never mention or link a portfolio in Sell Service copy; the user deleted that item. No portfolio reference appears in this prepared service.
- **Not done:** no Projects.co.id login/account access, service form entry, listing submission, publication, bid, message, payment, or sale. Do not claim the service is live.
- **Next priority:** after the user reviews/approves this listing, prepare a higher-ticket one-workflow spreadsheet template service. Do not publish or claim a price was user-approved without explicit confirmation.

## 0X. PROJECTS.CO.ID — STRATEGI MEMPERBESAR PEMASUKAN — 2026-11-13

**Catatan status terkini — lihat 0AB:** strategi dan harga di bawah merupakan catatan proposal sebelum pengguna mengonfirmasi bahwa tiga service sudah diterbitkan. Jangan membaca status lama ini sebagai bukti bahwa listing belum live; detail konfigurasi live belum diverifikasi.

**Status historis saat catatan ini dibuat:** Tujuan memperbesar pemasukan adalah USER_CONFIRMED dari percakapan 2026-11-13. Rancangan strategi dan paket/harga di bawah adalah ASSISTANT_PROPOSAL; belum dipilih, diterbitkan, atau diuji.

- **Konteks aktif:** Projects.co.id adalah kanal freelancer tambahan yang terpisah dari bisnis utama RapiUsaha. RapiUsaha tetap bisnis produk digital B2C untuk UMKM; jangan mengubahnya menjadi studio jasa/konten.
- **Koreksi strategi sebelumnya:** mengandalkan satu atau dua proyek bidding Data Entry yang kebetulan cocok terlalu sempit dan tidak bisa menjadi mesin pendapatan utama. Halaman Projects.co.id yang diperiksa pada 2026-11-13 menunjukkan sekitar 134 proyek aktif di semua kategori dan 8 di Data Entry & Data Mining. Angka listing bisa berubah, tetapi snapshot cukup untuk menunjukkan bahwa bidding Data Entry saja memiliki suplai terbatas.
- **Kompetisi service:** halaman Browse Services menampilkan sekitar 19.045 service total dan 1.756 pada kategori Data Entry & Data Mining. Data Entry generik sangat padat. Service input 100 baris seharga Rp50.000 hanya cocok sebagai produk pembuka, bukan pusat profit.
- **Bukti harga penawaran (bukan penjualan):** contoh service publik mencakup Rp125.000 untuk Data Entry dan Pengolahan Data; Rp250.000 untuk paket Data Entry/Admin sekitar 300 baris; Rp250.000 untuk penawaran Google Sheets 1.000 baris; serta Rp300.000 untuk Data Entry dengan formula. Jangan mengklaim listing ini sudah laku jika indikator penjualan/rating tidak terlihat.
- **Fitur untuk menaikkan nilai order:** FAQ resmi Service menjelaskan bahwa worker dapat menambahkan paid options/additional pada harga dasar; buyer dapat memilih opsi dan mengajukan special requirements. Aturan resmi juga meminta satu jenis service yang spesifik per halaman. Artinya, gunakan service yang fokus tetapi punya add-on terukur untuk menaikkan average order value.
- **Rekomendasi katalog service (ASSISTANT_PROPOSAL, bukan listing aktif):**
  1. **Basic — Input & Penataan Data:** pertahankan draft Rp50.000 / maksimum 100 baris dan 5 kolom sebagai entry offer. Tambahan baris/sheet/format khusus dapat menjadi paid option jika scope jelas.
  2. **Standard — Pembersihan & Perapian Spreadsheet:** usulan harga awal Rp125.000–Rp175.000 untuk maksimum 300 baris; mencakup standardisasi format, penghapusan duplikat, penandaan data kosong, pengurutan, dan pemeriksaan ulang. Ini harus diuji dengan sampel dan batas sumber yang jelas.
  3. **Higher-ticket — Pembuatan Template Excel/Google Sheets Sederhana:** usulan harga awal Rp250.000–Rp350.000 untuk satu alur kerja yang disepakati (mis. stok, transaksi, atau rekap), sheet input + ringkasan sederhana, formula dasar, dan panduan singkat. Hindari klaim dashboard/otomatisasi kompleks sampai benar-benar diuji dan sanggup diserahkan konsisten.
  4. **Text Annotation Bahasa Indonesia:** jadikan service spesifik terpisah hanya bila guideline, ukuran batch, dan kriteria label dapat dijelaskan. Sampel latihan bukan pengalaman klien.
- **Strategi bid:** jangan menunggu kata kunci Data Entry saja. Cari tugas yang masih berdekatan dengan kemampuan yang dibangun—administrative support, spreadsheet formatting, data cleaning, data reconciliation, form processing, dan anotasi teks—namun hanya ambil pekerjaan dengan sumber data jelas, scope terukur, akses aman, tenggat masuk akal, dan tidak menuntut skill/prosedur yang belum dikuasai. Tolak data pribadi akun/credential, pembuatan akun massal, transfer akun, survey yang ditolak, dan pekerjaan offline yang memerlukan perjalanan.
- **Revenue math:** service fee 12%. Pesanan Rp50.000 menyisakan Rp44.000 sebelum biaya transfer; Rp150.000 menyisakan Rp132.000; Rp250.000 menyisakan Rp220.000. Secara matematis, lima order Rp250.000 menghasilkan Rp1.100.000 setelah fee service tetapi sebelum withdrawal/tax/biaya kerja. Ini target aritmetika, bukan ramalan order.
- **Sell Product untuk RapiUsaha:** tetap kanal tambahan jangka menengah, bukan solusi uang cepat. Marketplace menampilkan 729 produk secara keseluruhan pada snapshot, dan contoh template Excel Rp75.000 menunjukkan 8 sales serta template stok Rp75.000 menunjukkan 3 sales. Ini bukti ada beberapa transaksi, bukan ukuran pasar besar. Fee product 20%; file harus final, diuji, dan bisa digunakan sebelum dikirim untuk verifikasi.
- **Affiliate Projects.co.id:** komisi 10–20% dihitung dari fee platform, bukan dari nilai transaksi. Contoh hitung: proyek Rp1.000.000 dengan fee 12% menghasilkan fee platform Rp120.000; komisi affiliate sekitar Rp12.000–Rp24.000 sebelum pajak. Produk Rp75.000 dengan fee platform 20% menghasilkan fee Rp15.000; affiliate sekitar Rp1.500–Rp3.000. Tidak ada subreferral. Ini bukan prioritas pendapatan utama tanpa sumber traffic/referral yang besar dan relevan.
- **Arah yang disarankan:** bangun tangga penawaran (entry service + cleaning service + template/custom spreadsheet service), pakai add-ons untuk menaikkan nilai tiap order, sambil bidding pada kategori berdekatan yang lolos filter. Produk digital RapiUsaha diperlakukan sebagai jalur aset terpisah yang butuh validasi. Ukur harga bruto, fee, waktu kerja, revisi, interaksi, dan penghasilan bersih per jam; jangan menambah banyak listing sebelum tiap paket memiliki contoh kerja dan kapasitas deliverable yang teruji.
- **Status listing dan transaksi:** draft Sell Service input 100 baris masih ASSISTANT_PROPOSAL. Tidak ada service baru yang diterbitkan, product listing dibuat, bid dikirim, pesan owner dikirim, atau transaksi dilakukan.
- **Sumber dan riset detail:** `research/projects_co_id_expanded_income_strategy_2026-11-13.md`.

## 0W. PROJECTS.CO.ID — PROFIT OPPORTUNITY ANALYSIS — 2026-11-13

**Catatan status terkini — lihat 0AB:** analisis ini adalah arsip sebelum pengguna mengonfirmasi tiga service telah diterbitkan. Pernyataan historis bahwa listing belum diterbitkan tidak lagi berlaku. Rincian live belum diverifikasi.

**Status historis saat catatan ini dibuat: ASSISTANT_RESEARCHED; recommendations are ASSISTANT_PROPOSAL, not user-approved account actions or published listings.** This is a supplementary income channel for the freelancer track; it does not replace the active RapiUsaha business.

- **Project context:** P-DIGITAL / RapiUsaha remains the active primary business: B2C digital products for UMKM. Projects.co.id freelancing is a separate, supplementary attempt to earn through project bidding and fixed-scope services. Do not convert RapiUsaha into a service studio or change its business model.
- **Latest user-confirmed correction:** user explicitly said the portfolio was deleted and asked that Sell Service copy not mention it. Do not refer to, link, or promise a portfolio in the current Sell Service description. This instruction concerns marketplace copy; it does not authorize deleting or editing any separate website/Worker.
- **Sell Service draft status:** one suggested service only — “Jasa Input Data ke Excel / Google Sheets hingga 100 Baris”, Rp50,000, 2-day delivery, maximum 100 rows and 5 columns. Description excludes portfolio references. This remains ASSISTANT_PROPOSAL; not yet confirmed as final, published, or sold.
- **Marketplace structure:** Projects.co.id has three distinct earning paths: open Projects (bid for client work), Sell Service (fixed-scope service that buyers can order), and Sell Product (downloadable digital products). Official Service FAQ says service offers must describe one clear scope, service fee 12%, and buyer can order a Service through a private project workflow. Official Seller FAQ says digital product files are delivered by automatic download after payment and seller fee is 20%.
- **Market competition:** public category snapshots on 2026-11-13 showed about 1,756 Data Entry services among 19,045 services, so generic data entry is a crowded category. Listing counts are not sales counts and can change. Similar public offers included Rp50,000 for 100 data rows, Rp50,000 for up to 500 simple cleaning rows, and higher-volume / richer spreadsheet offers. Therefore the proposed Rp50,000 / 100-row offer is a low-friction entry offer only, not a demonstrated profit-maximizing offer. Keep scope exact; do not promise complicated cleaning, formulas, scraping, or dashboards in that base price.
- **Service strategy recommendation (ASSISTANT_PROPOSAL):** begin with one narrowly scoped service rather than many nearly identical listings. Use optional paid add-ons only for work that can be reliably delivered and clearly scoped (additional rows, extra sheet, formatting, duplicate/missing-value check). A separate service for spreadsheet data cleaning/quality checks may be considered later only if it has a real differentiator and separate scope. Do not imply prior client work or credentials.
- **Open-project bidding — immediate candidate (ASSISTANT_PROPOSAL):** “Anotator Clickbait Judul Berita Berbahasa Indonesia untuk Penelitian Skripsi” was listed with 3 annotator slots, 300 headlines per worker, Rp120,000–150,000, around 92 bids in the latest observed page snapshot, 8 days, owner with no prior project rating at the time checked. User has an independent 30-headline synthetic practice sample, clearly a practice sample rather than client experience. It appears the best current fit among the reviewed bids due to three slots and language fit, but competition and unknown owner reputation remain. No bid was submitted.
- **Open-project backup (ASSISTANT_PROPOSAL):** “Template Excel — Laporan Data Reject Pengemas” around Rp125,000–150,000, about 100 entries and monthly category summary, around 134 bids in the latest observed snapshot. A sample workbook exists using fictitious data. This is a backup, not user-approved bid.
- **Lower priority / reject:** “Input 50 data” has a title/description mismatch (50 vs 100 rows) and very high competition (latest public snapshot about 612 bids); only consider after owner clarification. Avoid personal-contact/Gmail scraping, game-account handling, account transfers, survey work, cold sales/outreach, and tasks requiring unavailable travel/devices or unsafe access to third-party accounts.
- **Sell Product — future fit with RapiUsaha (ASSISTANT_PROPOSAL):** Projects.co.id lists downloadable digital products; sample Excel template product listings around Rp75,000 showed 8 and 3 recorded sales respectively in the page snapshot, which is a modest demand signal, not proof of broad market size. Seller fee is 20%, and buyer download is automatic after payment. This could be tested later as an additional distribution channel for a finished, validated RapiUsaha digital product. Do not publish the current RapiPO prototype yet: it is not user-confirmed as the final product, Google Sheets Android compatibility remains untested, no buyer validation has occurred, and price is not locked.
- **Fee/payout calculations:** Service fee 12% means a Rp50,000 base order leaves Rp44,000 before withdrawal fee. Published bank transfer fee for banks outside the listed preferred banks is Rp7,500 and the stated minimum withdrawal in that category is Rp57,500. One Rp50,000 order therefore may not reach the cash-out minimum by itself; accumulated balance or larger/add-on orders may be needed. For open Projects, the FAQ Worker says 12% worker fee, while the Pricing page displays a different fee table for low project budgets. Use 12% conservatively for bid calculations until Projects.co.id confirms which table applies. Do not conflate Project fee and Service fee.
- **Immediate priority recommendation:** maintain one simple Sell Service as a first-review acquisition offer; pursue only one or two well-matched live bids rather than mass-bidding; track views, orders, awarded bids, actual completion time, platform fee, transfer threshold, and net hourly return. If a genuine sale/review arrives, use actual data to decide whether to raise price or expand scope.
- **Evidence and source URLs:** 
  - Service FAQ and service fees: https://projects.co.id/public/pages/faq_umum
  - Worker bidding and escrow workflow: https://projects.co.id/public/pages/faq_worker
  - Pricing / fee table: https://projects.co.id/public/pages/pricing
  - Seller FAQ / digital product process: https://projects.co.id/public/pages/faq_seller
  - Data Entry services and project category snapshots: https://projects.co.id/public/browse_services/listing?search=Data+Entry and https://projects.co.id/public/browse_projects/listing?search=Data+Entry
  - Candidate annotation project: https://projects.co.id/public/browse_projects/view/41651d/anotator-clickbait-judul-berita-berbahasa-indonesia-untuk-penelitian-skripsi
  - Backup Excel project search: https://projects.co.id/public/browse_projects/listing?filter=newest&search=Microsoft+Excel
  - Candidate digital-product listing page: https://projects.co.id/public/browse_products/listing?search=Excel
- **What has actually happened:** public pages and marketplace policies were researched; existing GitHub shortlist and bid-draft notes were reviewed. No account login, service publication, product listing, bid, message, payment, or transaction was performed.
- **Next step (not yet user-approved):** choose whether to prioritize the annotation bid or first publish the single Sell Service. Do not submit any bid/message or edit the user's Projects.co.id account without explicit instruction.
- **Detailed analysis:** `research/projects_co_id_profit_opportunity_analysis_2026-11-13.md`.


## 0V. PROJECTS.CO.ID — INTERACTIVE PORTFOLIO — 2026-11-13

**Status: LIVE, public, and mobile-browser-tested.** Standalone portfolio link is ready for the user's Projects.co.id portfolio listing.

- User clarified that the website itself should not send clients back to Projects.co.id; the public website URL is simply pasted into the user's marketplace portfolio entry so visitors can open the work samples directly.
- Live public URL: https://portofolio-data.usahalab-id.workers.dev/
- Cloudflare Worker `portofolio-data` deployed on `usahalab-id.workers.dev`; health endpoint `/health` returns `ok`.
- Removed the full marketplace/contact CTA block, disabled "menunggu tautan profil" button, Projects.co.id copy references, and profile URL JavaScript. No Projects.co.id button or external marketplace link remains on the website.
- Changed top navigation CTA to an internal anchor `Lihat contoh kerja` linking to `#karya`.
- GitHub source: `portfolio/index.html`; detail: `research/projects_co_id_interactive_portfolio_2026-11-13.md`.
- Three demos previously passed mobile browser tests; this change affects only the final CTA/navigation and profile-specific handler, not demo logic.
- The URL above is the link to paste into the Projects.co.id portfolio item's `Link` field. No public profile URL is needed. No account edit/upload, bid, message, payment or transaction was performed.

## 0U. PROJECTS.CO.ID — GENERAL PROFILE & PORTFOLIO KIT — 2026-11-12

**Status: ASSISTANT_PREPARED; general profile copy and three visual work samples created; no profile edits or uploads.**

- User clarified that they want general profile material, not an Excel-only portfolio. Positioning: **Data Entry & Pengolahan Data | Spreadsheet | Pemeriksaan Data | Anotasi Teks**.
- Created a consolidated 4-page PDF: `Portofolio_Umum_Pengolahan_Data.pdf`, plus three standalone images for separate platform portfolio entries: input/penataan data, pemeriksaan kualitas data, and anotasi teks Bahasa Indonesia.
- Created copy-ready profile title/description, suggested skill tags, and portfolio item titles/descriptions in `Teks_Profil_Projects_co_id.txt`.
- The PDF rendered to four pages and was visually inspected after layout correction. All sample data/headlines are fictitious; these are practice examples, not client work, employment claims, or formal qualifications.
- Platform facts verified from official FAQ: edit profile through My Account > My Profile > Edit Profile; add portfolio through My Account > My Portfolio > Add New; each portfolio item asks for year, description, link, and image; maximum 10 items and 10 MB per file. Source: https://projects.co.id/public/pages/faq_umum
- Exact local deliverables: `/mnt/data/Portofolio_Umum_Pengolahan_Data.pdf`, `/mnt/data/Portfolio_01_Input_dan_Penataan_Data.png`, `/mnt/data/Portfolio_02_Pemeriksaan_Kualitas_Data.png`, `/mnt/data/Portfolio_03_Anotasi_Teks_Bahasa_Indonesia.png`, `/mnt/data/Teks_Profil_Projects_co_id.txt`.
- Prior Excel-specific workbook remains one additional sample, not the overall profile identity. Earlier shortlist ranking and project strategy are unchanged.
- No Projects.co.id account access, profile edit, image upload, bid, message, payment, or transaction occurred. User must perform the profile edits manually.

## 0T. PROJECTS.CO.ID — PORTFOLIO SAMPLE EXCEL REJECT PENGEMASAN — 2026-11-12

**Status: ASSISTANT_PREPARED; local workbook created and QA-checked. No account actions or bids/messages.**

- User asked for a concrete step to increase attractiveness to prospective clients. Prepared a targeted Excel portfolio sample for the backup listing "Template Excel — Laporan Data Reject Pengemas".
- Workbook delivered in this conversation: `Portfolio_Template_Laporan_Reject_Pengemasan.xlsx`.
- It includes 100 clearly labelled synthetic practice records plus 100 blank input rows, dropdown validation, per-row reject-rate formulas, a summary dashboard, monthly/category calculations, chart, reference lists, and usage instructions.
- QA: 100 records; 3,044 reject units; 789,448 production units; overall reject rate approximately 0.39%; category counts sum to 100; formula-error scan returned 0 matches; dashboard and input layout visually inspected.
- **This is only an independent demo using fictitious data.** It is not client work or evidence of employment, and must not be represented as such.
- Detailed artifact specification and checks: `research/projects_co_id_portfolio_sample_excel_reject_2026-11-12.md`.
- This does not alter the shortlist ranking: Indonesian headline annotation remains first priority; Excel reject-report template remains a backup. No Projects.co.id login, profile edit, upload, bid, message, payment, or transaction occurred.
- Next step: use the workbook as a sample only if the user chooses to pursue an Excel listing and the platform permits attaching it. Keep price and delivery commitment conditional on confirmed scope.

## 0S. PROJECTS.CO.ID — SHORTLIST LIVE UNTUK PEMULA — 2026-11-12

**Status: ASSISTANT_RESEARCHED; shortlist complete, no bids/messages sent.** User asked assistant to find worthwhile current projects.

- Ranked first: Anotator Clickbait Judul Berita (3 slots, 300 labels each, Rp150,000/person, deadline 7 Dec 2026, around 85 bids; owner new/no rating). Existing independent 30-row practice sample is relevant.
- Backup: Template Excel for packaging reject report (about 100 rows plus monthly category summary, Rp125k–150k, newer listing owner yruhiat769 has 0/10 and 0 projects, around 110–120 bids; older duplicate owner Ikaratna21 has 10/10 and 3 projects but ~224 bids).
- Third: Input 50 data (title/description count mismatch; owner 10/10, 6 projects, ~501 bids).
- Rejected: 1,000 Gmail contact extraction because new owner, 400+ bids, high volume and sensitive contact data; game-account data entry; old LinkedIn account transfers; outreach/sales; offline store photography; tester job because unclear per-tester pay and similar prior project is in arbitration. Survey work remains strictly rejected.
- Full shortlist, evidence, links, criteria, and rules: `research/projects_co_id_beginner_job_shortlist_2026-11-12.md`.
- No account access, bid, message, payment or transaction. Explicit user approval is still required before any bid/message.


## 0R. PROJECTS.CO.ID — INPUT 50 DATA (SCOPE CLARIFICATION) — 2026-11-12

**Status: ASSISTANT_ASSESSMENT_AND_DRAFT; no bid or message sent.** User supplied the listing details and asked for an assessment and a good message.

- Project: [Input 50 data](https://projects.co.id/public/browse_projects/view/d7651d/input-50-data), owner BisnisProposal.
- Current public listing still shows Published; owner rating 10/10 with 6 projects; about 501 bids; budget Rp150,000–Rp300,000; Select Deadline 10 Dec 2026; finish days 7.
- Scope discrepancy: title says 50 data, description says 100 food data. Source availability is also unspecified.
- Assessment: reasonable fit and positive owner track record, but very high competition. Recommended to ask owner to clarify number of rows and whether source list is supplied before committing to a price or delivery time.
- Draft and exact conditional price guidance: `research/projects_co_id_input_50_bid_advice_2026-11-12.md`.
- **No account access, message, bid, or payment occurred.** User has not chosen a final bid amount or explicitly authorized a submission.


## 0Q. PROJECTS.CO.ID — LIVE OPPORTUNITY + BID DRAFT — 2026-11-12

**Status: ASSISTANT_RESEARCHED; no user bid decision or account action yet.** Upwork remains blocked for account use until the user meets its age requirement. This checkpoint records a current, legally distinct Projects.co.id path that appears to meet the site's literal age wording.

- Live candidate selected for consideration: **Anotator Clickbait Judul Berita Berbahasa Indonesia untuk Penelitian Skripsi** — https://projects.co.id/public/browse_projects/view/41651d/anotator-clickbait-judul-berita-berbahasa-indonesia-untuk-penelitian-skripsi
- Listing currently shows Published / bids still open, 3 annotator slots, 300 Indonesian headlines per annotator, written guideline, Google Sheets, plus validation discussion; honorarium stated Rp150,000/person; deadline 7 Dec 2026; estimated 8 days.
- Latest observed indexed bid count 87 (some cached page snapshots show 80–85; counts fluctuate). Owner new / 0 previous projects / no rating, so reputation evidence is absent. Do not call this guaranteed or low-risk.
- Projects.co.id Terms say individuals must be "berusia diatas 17 (tujuh belas) tahun"; their page also reserves discretion to accept/reject users. User is 17 years 10 months on 12 Nov 2026; literal wording appears to allow registration, but acceptance is not guaranteed.
- Fee estimate if paid Rp150,000: 12% worker fee = Rp18,000; BRI transfer fee Rp7,500; estimated payout Rp124,500. Bidding is free. Never start until the platform confirms escrow is funded; keep all messages and payments inside Projects.co.id.
- Another alternative assessed: "Input 50 data" (description says 100 food entries), Rp150,000–300,000, owner rated 10/10 with 6 projects, but ~500 bids and quantity mismatch. Lower priority due intense competition and scope ambiguity.
- Created a new attachment for the annotation bid: `/mnt/data/Portfolio_Anotasi_Judul_Berita_30_Contoh.xlsx`. It contains 30 synthetic practice headlines, labels, rationales, confidence, review flags, guidance, and a formula summary; 15 Clickbait / 15 Non-clickbait; formula error scan returned zero. It is explicitly a practice sample, not client experience.
- Draft bid text and detailed evidence/calculation are stored in `research/projects_co_id_live_shortlist_and_bid_draft_2026-11-12.md` (commit verified in this session).
- **No account registration, profile edit, bid, message, or transaction has been performed.** User has not explicitly chosen or approved this job/bid.
- Next step if user chooses to pursue it: user submits the prepared bid from their Projects.co.id account; assistant can then translate/respond to project-owner communication and help produce/QA the deliverable. Do not submit or represent user without their explicit approval.


## 0P. UPWORK PROFILE KIT + TWO PORTFOLIO SAMPLES CREATED — 2026-11-12

**Status: ASSISTANT_PREPARED package is complete for offline review. No Upwork account/profile was read or changed, no KTP was sent, and no public profile or proposal was published.**

- User asked the assistant to complete the profile materials independently, without KTP/account onboarding.
- Created profile title and English overview, Bahasa Indonesia translation, skill suggestions, rate recommendation, profile-field guidance, two portfolio descriptions within Upwork limits, and two proposal starters.
- Created and visually verified 2 independent PDF work samples: product catalog data entry/QA and Indonesian headline annotation. Clearly marked as independent practice samples, not client work.
- Created and formula-checked an editable XLSX containing five sheets: Start Here, Product Data Entry, Data Entry QA, Text Annotation, Annotation Guide.
- Workbook verification: 30 fictional product rows; 0 duplicate SKU; 0 missing product names; 11 reorder flags; IDR 11,474,000 sample stock value; 30 synthetic headlines (13 Clickbait, 17 Non-clickbait); zero formula errors in scan. Dropdowns added for editable categories/labels.
- Files delivered in current conversation:
  - `Upwork_Profile_Kit_and_Proposal_Templates.pdf`
  - `Portfolio_01_Product_Catalog_Data_Entry.pdf`
  - `Portfolio_02_Indonesian_Text_Annotation.pdf`
  - `Upwork_Portfolio_Work_Samples.xlsx`
- Official Upwork help was checked for profile guidance, skills, portfolio title/description constraints, PDF portfolio upload, and minimum rates. Suggested rate is assistant proposal: USD 4/hour; not a market guarantee.
- Remaining user-only items when eligible: real profile photo, education provider name if desired, actual availability selection, review of the final text, and selecting skills available in Upwork tag picker. Employment history and certifications remain blank rather than invented.
- **Eligibility gate remains:** Upwork requires a freelancer account holder to be at least 18 (or older if age of majority requires it). This offline package does not authorize account creation/access/operation before eligibility. Do not use false age or another person's identity.
- Full source draft and artifact inventory: [research/upwork_profile_portfolio_package_2026-11-12.md](research/upwork_profile_portfolio_package_2026-11-12.md).


## 0O. UPWORK CONNECTOR SUDAH MUNCUL — BATAS USIA TETAP BERLAKU — 2026-11-12

**Status: USER_CONFIRMED menyatakan telah menghubungkan Upwork; toolset Upwork kini terlihat dalam runtime ChatGPT. Akun dan data privat belum diakses. Penggunaan akun freelancer ditahan karena Upwork mensyaratkan minimal 18 tahun.**

- Pengguna menyatakan: “Oke, gw dah hubungkan.”
- Pemeriksaan tool runtime setelah pesan tersebut menemukan Upwork MCP tools untuk freelancer dan client (job search, account listing, proposal, profile, messages, offers, contracts, milestone, financials). Ini mengonfirmasi toolset terdaftar/tersedia di ChatGPT; belum membuktikan akun freelancer telah ditarik/diperiksa karena tool tidak dipanggil.
- Upwork Help Center yang diperiksa 12 Nov 2026 menyebut pengguna harus berusia 18 tahun atau usia mayoritas di negaranya (mana yang lebih tua). Legal Center juga mensyaratkan akun individual berusia 18+ dan mampu membuat kontrak mengikat. KTP dan rekening tidak menggantikan persyaratan usia.
- **Keputusan aman:** karena pengguna saat ini 17, jangan panggil list_accounts, jangan baca profil privat, jangan cari/submit proposal melalui akun, jangan mengirim pesan, dan jangan lakukan tindakan akun sampai pengguna memenuhi eligibility. Jangan bantu mengakali batas usia atau memakai identitas/akun orang lain.
- Fitur resmi Upwork di ChatGPT yang dapat dipakai setelah eligible: job search, save/hide jobs, proposal drafting/submission, invitations, profile, messages, offers, contracts, milestones, earnings/Connects. Tindakan submit proposal harus direview dan dikonfirmasi pengguna; tindakan uang/kontrak mengikat diselesaikan melalui Upwork.
- Integrasi sudah tersedia secara teknis di runtime; **belum dilakukan account read atau tindakan apa pun**. Tidak ada job, proposal, kontrak, atau penghasilan yang telah diakses/dibuat.
- Sementara itu, asisten masih dapat membantu menerjemahkan deskripsi pekerjaan yang pengguna bawa secara manual, menyiapkan profil/CV dan latihan proposal tanpa membuka akun Upwork.
- Sumber resmi:
  - https://support.upwork.com/hc/en-us/articles/48662546612883-How-to-use-Upwork-in-ChatGPT
  - https://support.upwork.com/hc/en-us/articles/211067778-Who-s-eligible-to-join-and-use-Upwork
  - https://www.upwork.com/legal

## 0N. UPWORK + CHATGPT: BANTUAN BAHASA DAN WORKFLOW — 2026-11-12

**Status: USER_CONFIRMED — pengguna menyukai Upwork karena rating/review Google Play yang tinggi, tetapi kesulitan memahami bahasa Inggris dan menginginkan alur yang lebih otomatis. Assistant proposal: gunakan integrasi resmi Upwork di ChatGPT sebagai jalur utama setelah akun memenuhi syarat.**

- Pengguna menyebut memilih Upwork karena aplikasi Play Store punya rating bagus dan banyak ulasan; alasan ini adalah preferensi/kepercayaan pengguna, bukan bukti bahwa setiap lowongan/klien aman. Tetap audit tiap job dan client.
- Pengguna tidak nyaman dengan bahasa Inggris dan ingin workflow yang lebih otomatis.
- **Fakta baru — integrasi Upwork resmi ChatGPT:** Upwork Help Center (artikel “How to use Upwork in ChatGPT”, diperiksa 12 Nov 2026) menyatakan app Upwork terintegrasi ke ChatGPT. Cara koneksi: ChatGPT Settings → Apps → browse app directory → search “Upwork” → Connect → consent.
  - Freelancer dapat mencari job melalui @Upwork, mendapat rekomendasi sesuai profil, save/hide jobs, mengajukan proposal, meninjau/menolak/meminta perubahan atas offer, membaca/merespons pesan kontrak, mengirim milestone yang selesai dan memeriksa earnings/Connects.
  - Proposal/messaging/job actions disiapkan untuk review pengguna sebelum dikirim; proposal submit memerlukan konfirmasi. Accepting offer adalah tindakan mengikat yang diselesaikan di Upwork.com; aksi terkait dana/pembayaran/milestone juga mengarah ke Upwork.com.
  - Upwork menyatakan integrasi tidak dikenakan biaya tambahan dan tersedia bagi ChatGPT users termasuk Free; tindakan yang menghabiskan Connects tetap menunjukkan biaya sebelum konfirmasi.
  - Koreksi 12 Nov 2026: setelah pengguna menghubungkan Upwork, toolset Upwork muncul di runtime. Namun karena pengguna masih berusia 17 dan Upwork mensyaratkan 18+, tidak ada account read atau operasi akun yang dijalankan sampai eligible.
  - Sumber: https://support.upwork.com/hc/en-us/articles/48662546612883-How-to-use-Upwork-in-ChatGPT
- **Age gate:** Upwork Terms/eligibility menyatakan pengguna harus berumur 18 tahun atau usia mayoritas yang lebih tinggi. KTP/rekening tidak menggantikan umur minimum. Jangan membuat akun dengan tanggal lahir palsu atau memakai identitas/akun orang lain. Pengguna saat ini 17; Upwork account/proposal tidak digunakan sebelum 18.
  - Sumber: https://support.upwork.com/hc/en-us/articles/211067778-Who-s-eligible-to-join-and-use-Upwork ; https://www.upwork.com/legal
- **Android:** Upwork mobile app mendukung Android 10+; pengguna memakai Android 15. App mendukung profile, job search, proposals, messages, notifications, contracts. Beberapa fitur mungkin tetap butuh browser.
  - Sumber: https://support.upwork.com/hc/en-us/articles/360015504093-How-to-use-the-Upwork-mobile-app ; https://support.upwork.com/hc/en-us/articles/219506307-Using-the-mobile-app-for-job-searches-and-proposals
- **Bahasa Inggris:** Google Translate Tap to Translate mendukung teks yang dapat disalin dari app lain: aktifkan melalui Google Translate → Settings → Tap to Translate; di Android 10+ gunakan opsi Paste; salin teks Inggris lalu tekan gelembung/aksi Translate. Untuk gambar/screenshot, kirim ke ChatGPT atau gunakan Google Translate Camera/All Images. ChatGPT dapat mengubah listing, pesan, kontrak dan syarat kerja menjadi ringkasan Indonesia serta menyusun balasan Inggris yang diperiksa pengguna.
  - Sumber: https://support.google.com/translate/answer/6350658?hl=en-MC
- **Connects/biaya proposal:** jumlah Connects ditampilkan pada setiap job; harga satu Connect US$0.15. Boost bersifat opsional. Menggunakan Connects tidak menjamin proposal dilihat/diwawancara/diterima. Jangan bid semua pekerjaan dan jangan boost; seleksi proyek dengan hati-hati. Sumber: https://support.upwork.com/hc/en-us/articles/211062898-About-Connects-costs-uses-balances-and-expiration
- **Batas automation:** jangan gunakan script, bot, scraper, browser extension atau service yang otomatis mengakses/scrape/submit ke Upwork tanpa izin; Upwork dapat memberikan warning, restriction atau ban. Workflow boleh diotomatisasi melalui Upwork official ChatGPT integration dan API key yang disetujui; user harus meninjau/mengonfirmasi proposal dan tindakan penting.
  - Sumber: https://support.upwork.com/hc/en-us/articles/43342677368467-Use-bots-and-other-automation-properly ; https://support.upwork.com/hc/en-us/articles/48662546612883-How-to-use-Upwork-in-ChatGPT
- **Workflow yang diusulkan (ASSISTANT_PROPOSAL):**
  1. Setelah akun memenuhi 18+, hubungkan Upwork di ChatGPT Settings → Apps.
  2. Gunakan prompt bahasa Indonesia: cari remote fixed-price tasks berupa data entry, Indonesian-language data annotation, transcript cleanup, document formatting, spreadsheet cleanup/product catalog; phone-friendly; no calls; low English requirement; client history yang baik; no unpaid test.
  3. ChatGPT/Upwork menampilkan job. Assistant menerjemahkan deskripsi, deliverable, budget, deadline, English level, risks, Connects cost, client history dan fit Android.
  4. Assistant menyusun proposal bahasa Inggris berdasarkan skill nyata pengguna; jangan mengklaim pengalaman/keahlian yang tidak ada.
  5. Pengguna review dan memberi konfirmasi sebelum proposal dikirim.
  6. Semua pesan/offer diterjemahkan ke Indonesia; assistant menyiapkan balasan Inggris; pengguna meninjau sebelum pengiriman.
  7. Assistant membantu pengerjaan/QA deliverable pada file yang diizinkan klien. Ikuti kebijakan klien tentang AI dan jangan menyembunyikan penggunaan AI bila diwajibkan klien/platform.
  8. Terima offer, verifikasi kontrak/milestone, dan tindakan pembayaran hanya melalui Upwork; jangan pindah ke Telegram/WhatsApp atau membayar biaya di luar platform.
- **Status:** Upwork belum tersambung, akun belum dibuat/terverifikasi, tidak ada lowongan atau proposal yang dikirim. User memakai Android; penggunaan Upwork menunggu eligibility 18+.

## 0M. LARANGAN SURVEI UNTUK PENCARIAN KERJA — 2026-11-12

**Status: USER_CONFIRMED — pengguna sangat tidak menyukai survei. Semua platform atau tugas yang utamanya mengisi survei/kuesioner opini harus dikeluarkan dari rekomendasi pekerjaan.**

- Pengguna menegaskan JAKPAT/Nusaresearch bukan yang dicari; setelah mencobanya/melihatnya, pengguna juga menegaskan NusaTalent SideJobs ternyata survei. Maka **NusaTalent SideJobs: REJECTED**, bukan kandidat cadangan.
- **Ladenin: REJECTED**, berdasarkan pemeriksaan langsung pengguna; halaman tugas yang muncul ternyata survei.
- **JAKPAT, Nusaresearch, SurveyKita dan platform survey/reward panel lainnya: REJECTED untuk pencarian kerja ini.** Jangan merekomendasikan survei berbayar atau kuesioner sebagai alternatif.
- Makna “kerja repetitif” yang dicari: tugas/proyek freelance yang menghasilkan output konkret bagi klien, misalnya data entry, anotasi/labeling, proofreading/formatting, transkripsi dokumen, data cleaning, product listing/catalogue entry, QA website/aplikasi yang sah, atau virtual admin terstruktur.
- Marketplace freelance/project bidding dan fixed-price services tetap boleh dicari; utamakan proyek yang jelas deliverables, budget, deadline, client history, dan escrow/payout.
- Batasan penting: HP Android saja, modal Rp0, minim komunikasi/negosiasi, rekening BRI/DANA atas nama sendiri; tanpa sales, cold-DM, referral, spam, fake reviews/engagement, atau konten rutin.
- **Marketplace utama saat ini:** Projects.co.id; IFreelance.id sebagai lead yang harus mengonfirmasi umur 17, fee, dan payout; Freelancer.com tidak cocok karena akun minor memerlukan akun dewasa dengan izin dan bidding mensyaratkan saldo minimum. Ladenin dan NusaTalent dicoret.
- **Langkah selanjutnya:** tambah pencarian marketplace freelance Indonesia/global yang sah bagi pengguna sendiri, lalu cari listing proyek langsung di tiap marketplace; jangan menampilkan survey/gig survey sebagai pekerjaan.
- Laporan yang relevan: [research/online_work_projects_co_id_concrete_jobs_2026-11-12.md](research/online_work_projects_co_id_concrete_jobs_2026-11-12.md), [research/online_work_freelance_platform_alternatives_2026-11-12.md](research/online_work_freelance_platform_alternatives_2026-11-12.md).


## 0L. ALTERNATIF MARKETPLACE FREELANCE — 2026-11-12

**Status: Projects.co.id tetap utama; IFreelance.id menjadi lead kedua yang kelayakan usia 17-nya harus dikonfirmasi tertulis. Platform lain yang diperiksa sebagian besar tidak cocok karena umur, modal, atau aktivitas situs.**

- **IFreelance.id (ASSISTANT_PROPOSAL/OPEN):** marketplace jasa lokal di bawah PT IFree Technologies Indonesia, Terms diperbarui 8 Sep 2026. Punya kategori Data Entry & Pengetikan (22 layanan), pendaftaran freelancer gratis, seller listing paket/portfolio, custom offer/chat, order escrow, dan withdrawal ke rekening terverifikasi. Namun ToS hanya menyebut legal capacity dan age requirements yang ditentukan Platform; tidak menyebut angka minimum. Seller onboarding dapat meminta NIK, tanggal lahir, KTP, selfie, rekening. Belum diketahui apakah usia 17 diterima, komisi/fee dan batas minimum payout. Beberapa profil yang diamati bergabung Oktober 2026 dan memiliki 0 order/0 review; tautan profil homepage dengan review count tinggi ada yang 404. **Jangan KYC sebelum support mengonfirmasi usia 17 + KTP/rekening sendiri, fee, minimum payout dan mekanisme payout.** Support: support@ifreelance.id. URL https://ifreelance.id/
- **Freelancer.com:** terms melarang pengguna yang tidak dapat mengikat kontrak atau berusia di bawah 16; orang 16–17, tunduk hukum lokal, hanya dapat menggunakan akun orang dewasa dengan izin. Free member mendapat 6 bids/bulan tetapi harus memiliki saldo minimum US$20 untuk bid; withdrawal express ke bank Indonesia minimal US$50 ekuivalen + US$1 fee, dan first withdrawal dapat tertunda 15 hari. **Tidak cocok untuk akun independen tanpa wali dan modal Rp0.**
- **Dieliminasi karena usia/aturan:** Sribu minimal 18 + KTP + NPWP + 4 portofolio; Fastwork minimal 18; Fiverr <18 perlu akun orang tua/wali dan desktop untuk aktivasi seller; Workana tidak tersedia untuk minors; Truelancer, Guru, Contra dan Malt minimal 18 (Malt juga butuh bisnis terdaftar).
- **Freelance.co.id:** daftar proyek yang tampil terbaru Mei 2025, banyak posting tampak uji coba, layanan no-review/no-order; tidak dianggap aktif.
- **Gawee:** 17+ dan KTP, tetapi model kerja on-site, check-in/out lokasi dan fee 20%; bukan freelance remote.
- **Laporan:** [research/online_work_freelance_platform_alternatives_2026-11-12.md](research/online_work_freelance_platform_alternatives_2026-11-12.md).


## 0K. KOREKSI LADENIN + PELUANG KONKRET PROJECTS.CO.ID — 2026-11-12

**Status: USER_CONFIRMED menyatakan pengalaman langsung bahwa misi Ladenin yang ditampilkan adalah survei. Ladenin ditolak. ASSISTANT_PROPOSAL: Projects.co.id adalah jalur kerja pertama yang saat ini paling konkret dan syarat usia tertulisnya cocok untuk pengguna.**

- **Ladenin: REJECTED.** Pengguna telah membuka/memeriksa platform dan melaporkan bahwa pekerjaan yang disajikan ternyata survei. Jangan lagi mengarahkan pengguna ke Ladenin, meskipun Terms/fiturnya menyebut kategori “Digital/Online”. Pengamatan langsung pengguna mengoreksi klasifikasi asisten.
- **Projects.co.id: kandidat utama (ASSISTANT_PROPOSAL, belum ada akun/bid/penerimaan).**
  - Terms resmi menyebut individu “berusia diatas 17 (tujuh belas) tahun”; ini berbeda dari minimum 18. Pengguna berumur 17 tahun 10 bulan pada tanggal riset, sehingga secara literal sudah berusia di atas 17. Tidak perlu mengubah tanggal lahir atau menyamarkan identitas.
  - Pendaftaran pengguna gratis; satu akun dapat menjadi worker. Pekerjaan adalah proyek dengan hasil yang dapat dikemas sebagai file.
  - Cara kerja: pilih project → kirim bid/penawaran → owner memilih → owner mendanai proyek di escrow → sistem mengirim notifikasi mulai kerja → worker unggah hasil dan klik Report Done → setelah diterima, saldo dikreditkan → tarik ke rekening bank.
  - Tidak ada biaya saat mengajukan bid. Worker fee 12%. Transfer bank selain BCA, BNI, BNI Syariah, Mandiri dikenai Rp7.500; standar minimum auto-withdrawal Rp57.500. BRI tidak dibebaskan dari biaya ini. Estimasi net honor Rp150.000 = Rp132.000 setelah fee 12%, atau Rp124.500 setelah transfer Rp7.500.
  - Pembayaran hanya aman jika owner mendanai escrow terlebih dahulu; jangan mulai kerja sebelum notifikasi sistem resmi dan jangan menerima pembayaran langsung di luar platform.
  - User dapat menggunakan komunikasi chat di platform; masih ada kebutuhan bid dan komunikasi singkat. Tidak perlu cold-DM, tetapi ini bukan job yang sepenuhnya tanpa interaksi.
- **Peluang yang terpantau aktif saat pemeriksaan:**
  1. **Anotator Clickbait Judul Berita Bahasa Indonesia untuk penelitian skripsi** — https://projects.co.id/public/browse_projects/view/41651d/anotator-clickbait-judul-berita-berbahasa-indonesia-untuk-penelitian-skripsi
     - Kebutuhan 3 anotator; tiap orang melabeli 300 judul berita ke Clickbait/Non-clickbait di Google Spreadsheet; guideline diberikan; diskusi validasi diperlukan untuk hasil yang berbeda; kerja remote.
     - Honor tertulis Rp150.000 per anotator; published budget Rp120.000–Rp150.000; deadline 7 Desember 2026; 8 hari estimasi.
     - Bid count 88 pada saat pemeriksaan 12 November; owner baru, 0 project/0 rating. Kelebihan: tiga slot, pekerjaan cocok untuk HP + bahasa Indonesia + anotasi output. Kekurangan: persaingan tinggi, owner belum bereputasi, ada kebutuhan diskusi singkat. Pembayaran melalui escrow tetap melindungi jika mengikuti mekanisme platform.
     - **Rekomendasi peluang pertama untuk dipertimbangkan**, bukan jaminan dapat.
  2. **Input 100 data makanan ke Excel/Google Sheets** — https://projects.co.id/public/browse_projects/view/d7651d/input-50-data
     - 100 baris data makanan mengikuti template, validasi nama/info dan cek duplikasi; budget Rp150.000–Rp300.000; deadline 10 Desember; 7 hari estimasi.
     - Owner punya 6 project, rating 10/10, 36 reputation points; ini bukti histori lebih baik.
     - Bid count sudah 565 pada pemeriksaan halaman detail 12 November. Peluang lolos lebih berat karena persaingan ekstrem, tetapi owner lebih meyakinkan.
     - Lebih sedikit diskusi jika template jelas, dapat dilakukan melalui Google Sheets Android; perlu konfirmasi format sumber dan izin akses.
  3. **Internal Tester Google Play** — Rp150.000–Rp200.000, 12 tester, 14 hari, 70 bids. Bukan prioritas karena perlu menguji aplikasi yang belum diketahui serta kemungkinan melibatkan akses/perizinan akun Google.
  4. **Anotator Clickbait** dipilih di atas dua opsi lainnya karena pekerjaan data/labeling, tiga slot, dan budget/honor yang disebut jelas. Namun jangan membuat klaim pengalaman formal yang tidak dimiliki.
- **Pekerjaan yang ditolak pada kategori yang sama:**
  - Entry 3.000 akun game ke software: lingkup dan kepemilikan data tidak jelas.
  - Scrap 200 leads berisi nomor WA/akun sosial: tidak cocok dengan preferensi pengguna dan menyangkut pengumpulan data kontak.
  - Salin 1.000 email/nomor/alamat dari Gmail: menyentuh data pribadi dan akses email pihak lain; jangan pilih.
  - Konversi 80 halaman PDF scan jadi 50.000 kata: target akurasi 98%, tenggat 5 hari, sangat tidak realistis melalui HP saja; tolak.
- **Langkah selanjutnya (ASSISTANT_PROPOSAL):** bantu pengguna membuat akun Projects.co.id, isi profil jujur tanpa mengada-ada pengalaman, buat 1 bid yang kuat untuk lowongan anotator (honor Rp150.000) dan/atau 1 bid data entry, setelah persyaratan perangkat/waktu dipastikan. Teks bid dapat disiapkan asisten; pengguna tetap mengendalikan pengiriman bid.
- **Catatan penting:** tidak ada akun, KYC, bid, kontrak atau pembayaran yang telah dilakukan. Ini temuan peluang aktif, bukan klaim pekerjaan sudah didapat.
- **Laporan:** [research/online_work_projects_co_id_concrete_jobs_2026-11-12.md](research/online_work_projects_co_id_concrete_jobs_2026-11-12.md).


## 0J. FILTER KTP + REKENING — PLATFORM 17+ — 2026-11-12

**Koreksi instruksi pengguna:** jangan jadikan usia 18 sebagai filter absolut. Cari platform yang syarat resminya menerima usia 17+ dan/atau menyatakan KTP sebagai syarat eligibility. Gunakan identitas dan rekening milik pengguna sendiri; jangan memalsukan data. Jangan berhenti pada saran menunggu 18 bila ada platform resmi yang mengizinkan 17+.

- Kebutuhan tetap: tugas kerja nyata berbasis output (bukan survei), Android, tanpa modal, minim komunikasi, rekening BRI/DANA; tidak mau cold-DM, sales, spam, referral, engagement palsu, dan konten rutin.
- **Ladenin: REJECTED berdasarkan pemeriksaan langsung pengguna; lihat checkpoint 0K.**
  - Terms: usia minimal 17 tahun atau sudah memiliki KTP; identitas harus asli.
  - Web app memiliki kategori “Digital / Online” dan filter Online vs Di Lokasi.
  - KYC pekerja: NIK, foto KTP, selfie dengan KTP. Wajib sebelum melamar/mengambil misi. Jangan bagikan OTP/password/rekening ke pemberi misi.
  - Cashout: minimum Rp15.000; administrasi Rp2.500; maksimal 2x24 jam kerja. Input bank/e-wallet dan nomor rekening/nama pemilik. Dana pekerjaan menggunakan escrow; halaman menyatakan pemrosesan pembayaran melalui Mayar.id. Free tier mengizinkan ambil misi.
  - **Belum terverifikasi:** daftar misi aktif tidak tampil melalui crawl publik (menampilkan “sedang memuat data misi”), jadi belum ada bukti bahwa saat ini tersedia misi online yang cocok atau volume task di Bolaang Mongondow. Cek daftar terlebih dahulu, jangan KYC sebelum melihat tugas yang benar-benar layak.
  - URL: https://www.laden.co.id/
- **NusaTalent SideJobs: REJECTED oleh pengguna karena ternyata menampilkan survei; jangan rekomendasikan lagi.**
  - Google Play secara eksplisit menyatakan usia minimal 17 + KTP; reward misi dapat dicairkan ke rekening bank/e-wallet. Listing memiliki 1 juta+ download, rating 4.4 dan sekitar 30 ribu ulasan (ini skala aplikasi, bukan bukti jumlah SideJobs).
  - Namun Play Data Safety menyatakan aplikasi dapat mengumpulkan info keuangan dan data tidak dienkripsi saat transit.
  - Catatan domain diperbaiki: situs nusatalent.com yang tercantum di listing pada 12 Nov 2026 dialihkan ke situs kasino pihak ketiga, **tetapi** website resmi terbaru pada LinkedIn perusahaan adalah nusatalent.app, yang aktif dan mengidentifikasi PT Nusa Talenta Indonesia. Ini membuat website .com sebagai link di Google Play tetap anomali; jangan memasukkan KTP atau rekening sampai developer menjelaskan domain/link resmi.
  - Model bisnis perusahaan terutama influencer marketing, app/Google Maps reviews, lead generation, social-media management, affiliate/sales, dan merchant acquisition. Banyak misi berpotensi tidak sesuai preferensi anti-promosi/engagement pengguna. Jangan rekomendasikan misi review palsu, spam, lead-gen, referral atau sales.
  - URL resmi yang dirujuk LinkedIn: https://nusatalent.app/ ; Play Store: https://play.google.com/store/apps/details?id=com.nusatalent.nusatalent
- **Kerja E-niaga (HOLD — klaim belum terverifikasi):**
  - Terms menyatakan usia minimal 17 dan pembayaran harian ke bank/e-wallet; halaman mengiklankan tugas e-commerce remote berbasis HP seperti review listing, cek harga, stok, resi, kategorisasi katalog, QA foto, uji checkout.
  - Namun tidak menampilkan nama badan hukum/operator yang jelas, alamat kantor, brand klien bernama atau bukti pembayaran independen; angka 48 ribu pekerja dan Rp32 M+ pembayaran hanya klaim situs sendiri. Tombol tugas mengarah ke formcraft.app, bukan detail tugas. Jangan kirim KTP, rekening, OTP atau deposit sampai operator diverifikasi independen.
  - URL: https://kerjaeniaga.com/
- **Gawee (tidak cocok untuk batasan online sekarang):**
  - Terms menyatakan usia minimal 17, KTP valid, KYC KTP + selfie. Pendaftaran gratis; pekerjaan adalah gig tenaga harian/jangka pendek **di lokasi** (check-in lokasi, dokumentasi kerja) dengan komunikasi pemberi kerja lewat chat.
  - Platform fee 20% dari upah; dana melalui escrow dan dilepas setelah pemberi kerja menyetujui atau tidak merespons 24 jam.
  - Bukan pilihan HP-only/remote; pertimbangkan hanya jika ada gig sangat dekat dan akses transport aman.
  - URL: https://www.gawee.id/
- **BuzzTask (tidak cocok):** terms menerima 17+, dokumen identitas, rekening/e-wallet sendiri; tetapi pekerja mendaftar sebagai buzzer tugas social-media marketing. Pengguna tidak mencari engagement/marketing tasks. URL: https://socialrush.id/syarat-ketentuan
- **Status shortlist lama superseded:** Ladenin dan NusaTalent ditolak oleh pengguna karena daftar tugas yang tampil adalah survei. Jangan rekomendasikan keduanya. Kerja E-niaga HOLD; Gawee non-remote; BuzzTask ditolak karena jenis tugas. Rute kerja yang lebih konkret: Projects.co.id (checkpoint 0K).
- Laporan: [research/online_work_17plus_ktp_bank_platforms_2026-11-12.md](research/online_work_17plus_ktp_bank_platforms_2026-11-12.md).


## 0I. JALUR KERJA AI-DATA: 17 TAHUN SEKARANG, 18 TAHUN NANTI — 2026-11-12

**Status: hasil riset aktual; tidak ada pekerjaan/task platform yang saat ini terverifikasi cocok untuk seluruh batasan usia 17 + Android-only + minim komunikasi. Jalur terbaik setelah usia 18 sudah ditemukan.**

- Profil yang dipakai: pengguna berusia 17 tahun, memiliki e-KTP, Android-only, modal Rp0, memprioritaskan pekerjaan repetitif yang menghasilkan output (data labeling, anotasi, transkripsi, validasi/data entry), tidak mau survei, sales, referral, engagement palsu, cold-DM, atau membuat konten rutin.
- **Tanggal akses usia 18:** catatan pengguna menyebut ulang tahun 25 Desember 2026. Pada tanggal riset 12 November 2026, tersisa 43 hari. Jangan menggunakan tanggal lahir palsu atau mengajukan aplikasi yang mensyaratkan 18+ sebelum tanggal tersebut.
- **Pilihan utama setelah 18 tahun — Tasker (PT Literasi Digital Aksara):**
  - Website resmi menyebut AI data work dari rumah: pelabelan gambar/video, anotasi teks, transkripsi dan pelabelan audio.
  - Syarat: 18+, penutur asli Bahasa Indonesia, bahasa daerah menjadi nilai tambah, dan menerima laptop atau HP dengan internet stabil.
  - Pendaftaran gratis, tidak meminta biaya/deposit, ada tes dan pelatihan sebelum proyek; bayaran hanya untuk hasil yang lolos quality check.
  - Form aplikasi juga meminta kota/kabupaten, bahasa, jenis tugas, perangkat, CV PDF/DOCX; pengalaman bersifat opsional. Mereka secara eksplisit meminta calon kontributor **tidak mengirim foto KTP atau data rekening di tahap aplikasi awal**.
  - Belum dipublikasikan: tarif tiap proyek, volume proyek per worker, jadwal pencairan dan rel pembayaran. Wajib tanyakan/konfirmasi setelah lolos tahap awal; jangan menjanjikan nominal.
  - URL: https://tasker.co.id/
- **Pilihan kedua setelah 18 tahun — Jwuma / Corpshore AI:**
  - Official Indonesia page menyatakan pekerjaan remote aktif di Indonesia: anotasi teks/gambar, evaluasi model dan quality assurance. Sample task juga mencakup response ranking dan data entry.
  - Bahasa yang mereka sebut memiliki permintaan di Indonesia: Bahasa Indonesia, Jawa, Sunda, Inggris. Jenis task yang terbuka bergantung bahasa dan assessment.
  - Terms dated 22 Oct 2026: umur minimal 18; harus legally able to work di negara domisili; satu akun/orang. Tarif ditampilkan sebelum reservasi; tidak ada biaya daftar/kualifikasi/kerja/payout; pekerjaan yang diterima dibayar pada batch rutin.
  - Halaman Indonesia menyebut payout mingguan via Wise, yang dapat meneruskan ke rekening bank atau DANA/GoPay/OVO/ShopeePay di Indonesia. Wise account sendiri tidak wajib.
  - **Belum terverifikasi:** kecocokan penuh antarmuka/seluruh jenis task dengan HP saja; uji kompatibilitas dari Android harus dilakukan setelah memenuhi umur.
  - URL: https://platform.corpshore.ai/id/indonesia ; Terms: https://platform.corpshore.ai/legal/terms
- **Kandidat 17+ yang ditahan — NusaTalent SideJobs:**
  - Google Play menyatakan SideJobs menerima usia 17+ dengan KTP, menyediakan misi dan reward yang dapat dicairkan ke bank/e-wallet; terdapat beberapa ulasan pengguna yang mengaku pernah mendapat uang dari misi.
  - Namun deklarasi Google Play menyebut aplikasi dapat mengumpulkan informasi keuangan dan data tidak dienkripsi saat transit. Lebih serius, tautan situs resmi developer yang tercantum di Google Play (nusatalent.com) pada 12 Nov 2026 justru redirect ke domain judi kasino Polandia, bukan situs NusaTalent.
  - **Keputusan keamanan: HOLD.** Jangan unggah KTP, rekening, atau data sensitif ke aplikasi sampai pengembang mengonfirmasi domain resmi dan penanganan data melalui kanal developer yang terverifikasi. Jangan anggap misi yang tidak diketahui sebagai pekerjaan cocok. Listing Play Store: https://play.google.com/store/apps/details?id=com.nusatalent.nusatalent
- **KitaLulus: kanal pencarian, bukan bukti pekerjaan yang cocok.** Platformnya secara resmi menerima pelamar usia 17+ dengan KTP, tetapi lowongan remote/admin yang diperiksa menampilkan inkonsistensi: Ulina Studio terdaftar beralamat Medan sementara lowongan ditandai Jakarta Pusat; label part-time tetapi jam kerja Senin–Jumat 09.00–17.00; rincian gaji baru dijelaskan saat proses. Listing Pemeriksa Data Digital AKP Fashion Store punya pola jam yang sama dan minim detail. Jangan menganggap listing tersebut valid sebelum perusahaan dan recruiter diverifikasi langsung di platform.
- **Dieliminasi:** Jwuma dan Tasker bukan pilihan sebelum 18; Visym Collector privacy policy menyebut tidak mengumpulkan data anak di bawah 18 dan terms mengatur data video/GPS serta lisensi dataset; Braintrust payout support list tidak mencantumkan Indonesia, sehingga jangan prioritaskan untuk rekening lokal. Flavetask/Jwuma/Tasker tidak boleh diakses dengan umur palsu.
- **Urutan tindakan konkret:**
  1. Sekarang: jangan buang waktu pada panel survei dan jangan unggah KTP ke NusaTalent sementara domain/keamanan belum jelas. Tidak ada microtask 17+ yang lolos semua bukti yang dikumpulkan.
  2. Sebelum 25 Des: siapkan CV satu halaman dan latihan anotasi dasar Bahasa Indonesia; jangan bayar kursus/sertifikat atau mengirim data identitas ke pihak tak terverifikasi.
  3. Mulai 25 Des 2026: ajukan Tasker dulu karena HP dinyatakan diterima secara eksplisit; kemudian ajukan Jwuma dan uji task/task UI dari Android. Cek tarif per task, minimum payout, serta apakah DANA/BRI tersedia sebelum menginvestasikan waktu besar.
  4. Catat hasil qualification, project availability, accepted/rejected output, waktu kerja dan uang bersih. Stop bila tidak ada task atau rate tidak layak.
- **Kesimpulan:** peluang yang paling mendekati preferensi pengguna adalah pekerjaan AI-data, bukan aplikasi survei. Jalur realistis yang diverifikasi kuat dimulai saat pengguna berusia 18: Tasker (HP jelas diterima) dan Jwuma (payout Indonesia transparan serta no-fee). Belum ada jaminan akan diterima atau mendapat volume kerja tertentu.
- Laporan rinci: [research/online_work_ai_data_path_17_to_18_2026-11-12.md](research/online_work_ai_data_path_17_to_18_2026-11-12.md).


## 0H. KOREKSI ARAH KERJA ONLINE — TANPA SURVEI — 2026-11-12

**Status: USER_CONFIRMED menolak jalur survei berbayar. Kandidat kerja repetitif harus menghasilkan output kerja; belum ada platform atau lowongan yang dipilih/dijalankan.**

- Koreksi dari pengguna: “Survei? Ngapain anjing survei.” Ini menolak rekomendasi survei sebagai jawaban atas permintaan kerja online. Asisten sebelumnya salah menyamakan repetisi dengan survei berbayar.
- **JAKPAT dan Nusaresearch: REJECTED untuk pencarian kerja ini.** Jangan rekomendasikan atau arahkan pengguna mendaftar ke panel survei.
- Definisi kerja yang dicari: tugas yang menghasilkan output kerja, misalnya pelabelan data/gambar, pemeriksaan dan validasi data, input katalog/produk, dokumentasi, uji aplikasi yang sah, atau tugas administrasi terstruktur; bukan kuesioner opini, promosi, referral, manipulasi review, atau menghubungi calon pelanggan.
- Profil tetap: HP Android saja, modal Rp0, minim komunikasi/negosiasi, usia 17, e-KTP, rekening BRI dan DANA, tanpa keterlibatan wali. Jangan mengasumsikan bisa memakai laptop/PC atau kemampuan bahasa Inggris.
- **Kandidat platform paling dekat (ASSISTANT_PROPOSAL): NusaTalent SideJobs.** Listing resmi Google Play menyebut pendaftar minimal 17 tahun + KTP; misi memberi reward/komisi yang dapat dicairkan ke bank atau e-wallet. Namun jenis misi, rate, volume task, minimum payout, dan bukti payout aktual belum dipublikasikan/diperiksa. Wajib cek daftar misi dulu; hanya pertimbangkan pekerjaan data/admin/app-testing yang sah dan repetitif. Tolak promosi, sales, referral, spam, review palsu, pembelian, top-up, dan tugas yang meminta mengontak orang. Catatan keamanan: Google Play menyatakan data keuangan dapat dikumpulkan dan data tidak dienkripsi saat transit menurut deklarasi developer.
- **Kanal lowongan cadangan (ASSISTANT_PROPOSAL): KitaLulus.** Pusat bantuan resminya menyebut usia minimum 17 tahun dengan KTP, serta filter freelance dan remote. Namun daftar remote-freelance yang diperiksa pada 12 Nov dipenuhi “Misi Seru” yang merupakan campaign marketing/promosi; itu tidak cocok dengan preferensi pengguna. Beberapa lowongan administrasi/AI remote lainnya meminta laptop/komputer atau jadwal full-time dan/atau identitas perusahaan belum cukup diverifikasi. Belum ditemukan lowongan spesifik yang lolos seluruh batasan HP-only dan minim komunikasi.
- **Ladenin: tidak diprioritaskan.** Syaratnya 17 tahun atau memiliki KTP dan sistemnya memuat misi online, tetapi sampel tugas yang tampil didominasi follow/engagement media sosial, rating/review, referral, atau pekerjaan dengan chat; beberapa hanya bernilai Rp1.000–Rp2.000 dan banyak kuota sudah 0. Hindari review palsu, manipulasi engagement, referral, serta misi yang meminta data/akun atau pembelian.
- **Platform tidak eligible saat ini:** Tasker (18+), Jwuma (18+), Flavetask (18+), AdaTugas (18+). Jangan sarankan pengguna mengakali umur.
- **Tidak terverifikasi / jangan serahkan data:** Kerja E-niaga mengklaim tugas e-commerce bernilai puluhan ribu dan pencairan harian, tetapi tidak ditemukan bukti independen tentang badan usaha, volume tugas, atau pembayaran nyata. Jangan unggah KTP/rekening atau bekerja di sana sebelum identitas operator dan payout diverifikasi.
- **Waitlist, bukan pekerjaan aktif:** DataKita mempromosikan tugas beta yang dirancang untuk smartphone dan pembayaran e-wallet Indonesia, tetapi saat diperiksa hanya membuka waitlist; tidak menjanjikan akses, task, atau pendapatan dan syarat usia belum jelas.
- **Kesimpulan saat ini:** belum ada platform yang terbukti menyediakan kerja repetitif berbayar, HP-only, minim komunikasi, dan memenuhi usia 17 sekaligus. NusaTalent SideJobs adalah kandidat pertama untuk diperiksa isi misinya, bukan jaminan pekerjaan. Jangan kembali ke survei sebagai pengganti.
- **Sumber riset koreksi:** [research/online_work_task_based_shortlist_no_surveys_2026-11-12.md](research/online_work_task_based_shortlist_no_surveys_2026-11-12.md).

## 0G. ARSIP SHORTLIST LAMA — SURVEI (SUPERSEDED) — 2026-11-12

**Status: rekomendasi JAKPAT/Nusaresearch pada catatan lama dicabut setelah pengguna menolak survei. Jangan gunakan sebagai langkah aktif.** Isi riset historis masih tersimpan di [research/online_work_shortlist_android_repetitive_2026-11-12.md](research/online_work_shortlist_android_repetitive_2026-11-12.md); kandidat survei berstatus REJECTED, bukan pilihan pengguna.

## 0F. CHECKPOINT KANDIDAT PRODUK HOME DAYCARE — 2026-11-12

**Status: HOME DAYCARE STARTER/ADMIN KIT MENJADI KANDIDAT VALIDASI TERATAS (ASSISTANT_PROPOSAL); BELUM PRODUK FINAL USER_CONFIRMED.**

- **Bukti utama:** satu listing Etsy Home Daycare Starter Kit berstatus Bestseller, 20+ views/24h, harga promo US$9.99, memiliki **93 ulasan listing itu sendiri**, rating 4.8/5, dan 98% buyers recommend. Ini bukti pembelian item yang lebih kuat daripada kandidat terbaru lain yang dibandingkan.
- **Koreksi metrik:** 82.3k sales dan 5.5k ratings adalah angka seluruh toko, bukan listing. 93 review dan 4.8/5 adalah angka item-level. Bestseller menunjukkan volume penjualan tinggi dalam enam bulan terakhir menurut Etsy; bukan jumlah transaksi pasti.
- **Apa yang dijual pesaing:** 275 halaman Canva meliputi enrollment documents, parent handbook/policy, emergency plan, child intake, daily reports, attendance, menus, schedule, flyers, signs dan activity materials. Harga promo $9.99.
- **Sinyal kebutuhan:** review item menyebut mudah di-download/edit, “it's what I have been searching for”, dan menghargai banyaknya materi yang termasuk.
- **Calon pembeda (belum terbukti):** smaller, curated, phone-friendly, no-Canva, fillable/printable Parent Onboarding & Admin Pack. Listing pesaing menyatakan Canva phone app dapat glitchy dan merekomendasikan komputer; ini pernyataan penjual tentang produk sendiri, bukan bukti masalah universal.
- **Hard competitor:** NestliCare menyatakan seluruh software daycare gratis selamanya, termasuk attendance, daily reports, meal tracking, billing, parent messaging, enrollment, documents/forms, staff management dan parent app. Jadi jangan membuat software/attendance/payment tracker yang menduplikasi fungsi tersebut.
- **Batas produk yang direkomendasikan:** fokus pada printable/fillable onboarding dan komunikasi keluarga serta dokumen admin non-legal. Hindari kontrak mengikat, kebijakan lisensi/regulasi, emergency plans, medical/medication authorization, dan klaim legal compliance karena harus disesuaikan yurisdiksi serta memerlukan review profesional.
- **Pembanding lain:** mobile detailing pre-inspection ($4.35, 12 item reviews 5.0) bersaing dengan aplikasi gratis DetailProof/Shine Route; embroidery pricing ($2.65, 37 item reviews) punya beberapa kalkulator gratis; candle costing (35 item reviews) berhadapan dengan BatchScale 23 free tools dan Stockria free up to 250 items. Laporan deep-dive: [research/RapiUsaha_daycare_candidate_deep_dive_2026-11-12.md](research/RapiUsaha_daycare_candidate_deep_dive_2026-11-12.md).
- **Distribusi hard gate:** Etsy paling menarik untuk discovery file digital, tetapi pemilik akun harus 18+. Pengguna tidak menghendaki wali; usia pengguna belum memenuhi syarat hingga 25 Desember 2026. Kreavo hanya pilihan sementara yang perlu diuji untuk age 17+, checkout negara pembeli, rekening/KYC/payout; bukan diasumsikan ramai atau mampu menerima transaksi global.
- **Keputusan produk:** RapiPO home bakery generic tetap NO-GO saat ini. Daycare kit menjadi kandidat riset berikutnya, bukan final. Segmen, produk, isi paket, harga dan marketplace tetap OPEN hingga ada persetujuan pengguna dan hard gate pembayaran lolos.
- **Langkah berikutnya:** audit review negatif/fitur kekurangan home daycare listings lain, validasi apakah pembeli membutuhkan pack yang lebih kecil/no-Canva, hitung margin, dan pastikan checkout/payout dapat bekerja bagi pengguna. Jangan membuat produk full atau video launch sebelum gap dan jalur penjualan terbukti.


## 0E. CHECKPOINT AUDIT CELAH HOME BAKERY DAN PEMBANDING — 2026-11-12

**Status: PUTARAN AUDIT SELESAI; BELUM ADA PRODUK FINAL YANG LAYAK DIKUNCI.**

- **Proyek aktif tetap:** RapiUsaha — produk digital B2C untuk UMKM/owner-operators; harga menarik dengan kualitas sebagai pembeda; marketplace yang ramai; video TikTok/Instagram dibuat asisten. Ini semua tetap USER_CONFIRMED.
- **Kandidat lama:** Home bakery masih memiliki paid-item evidence paling jelas dari segmen yang diperiksa sebelumnya, tetapi kategori order/costing/production planner sudah padat.
- **Audit gap terbaru:** masalah perubahan pesanan custom nyata (flavor, size/tier, design, dietary requirements, delivery, rush fees; impact ke biaya, waktu, dan keputusan pembelian bahan). Namun, inti solusinya sudah ditawarkan oleh Orders App (capture/update order details) dan GetTheOK (customer change approval, price impact, version history; free 3 approvals/month, paid plans $9/$19/$29/month). Etsy search juga menampilkan Bakery Order Change & Pickup Kit seharga $2.49.
- **Putusan:** jangan membuat RapiPO generik, order-revision spreadsheet, atau approval tracker bakery generik sekarang. Pain point valid, tetapi diferensiasi dan willingness-to-pay untuk file baru belum terbukti serta pengganti software gratis sudah ada.
- **RapiPO:** NO-GO untuk ekspansi dan peluncuran dalam bentuk generik. Prototype tetap arsip pembelajaran; jangan menyebut produk siap jual/validated.
- **Segmen pembanding:** dog grooming/mobile pet grooming dan short-term-rental cleaning juga punya produk digital yang bersaing. Sinyal item-level reviews yang diperiksa terbatas; shop sales jangan disamakan dengan sales listing. Belum ada bukti cukup untuk pindah segmen.
- **Laporan audit:** [research/RapiUsaha_gap_validation_custom_order_changes_2026-11-12.md](research/RapiUsaha_gap_validation_custom_order_changes_2026-11-12.md) (SHA terakhir diverifikasi 539f7e53b8716007907088f5e8d6efe657b79b1c).
- **Langkah berikutnya (ASSISTANT_PROPOSAL):** perluas riset ke kandidat UMKM lain, memakai bukti ulasan/pembelian pada listing spesifik dan audit solusi gratis/software; baru buat prototype setelah ditemukan gap yang tetap nyata setelah kompetitor diperiksa. Jangan mengunci produk/segmen/harga/platform tanpa persetujuan pengguna.
- **Batas:** jangan membangun produk hanya agar ada kemajuan; bukti masalah saja tidak cukup jika pengganti yang gratis sudah menyelesaikannya. Jangan membuat video iklan launch untuk produk yang belum dipilih dan divalidasi; video prototipe yang ada hanya demo internal.


## 0D. CHECKPOINT PERBANDINGAN PRODUK DAN AUDIT KOMPETITOR — 2026-11-12

**Status: RISET PEMBANDING SELESAI UNTUK PUTARAN INI; TIDAK ADA PRODUK FINAL YANG DIKUNCI.**

- **Proyek aktif:** RapiUsaha, produk digital B2C untuk UMKM; model, kualitas sebagai pembeda, harga menarik, marketplace ramai, promosi TikTok/Instagram dengan video dibuat asisten tetap USER_CONFIRMED.
- **Temuan utama:** home bakery masih kandidat dengan bukti permintaan listing spesifik paling kuat dibanding cleaning, nail tech, dan photography yang diperiksa. Bukti termasuk listing Etsy berstatus Bestseller dan listing lain dengan 61 ulasan item 4.8/5.
- **Koreksi strategi produk:** RapiPO all-in-one generik tidak layak diluncurkan dalam bentuk sekarang. Pesaing sudah menawarkan 11–14 tab, order/DP, recipe costing, inventory, CRM, expenses, dashboards, production planning, dan bahkan order form Google Forms yang auto-save ke Sheets.
- **Diferensiasi “mobile-first/mudah digunakan” belum terbukti.** Beberapa pesaing sudah mendukung Google Sheets mobile/phone; klaim itu tidak boleh dianggap pembeda sebelum uji komparatif.
- **Celah yang masih perlu diuji:** satu job spesifik yang belum ditangani baik (misalnya riwayat revisi order/version control atau weekly preorder capacity). Ini hipotesis riset, belum keputusan produk.
- **Cleaning business:** harga dan nilai problem pricing menarik, tetapi bukti yang diperiksa lebih banyak pada level shop sales, sementara listing kalkulator spesifik sering belum punya review. Belum menggantikan home bakery.
- **Nail tech:** pasar ramai tetapi harga banyak tertekan dan apps booking/payment adalah alternatif yang lebih kuat.
- **Photography:** harga bundle dapat lebih tinggi tetapi validasi item dan kebutuhan support belum cukup.
- **Sourdough microbakery:** subsegmen lebih tajam, tetapi sudah ada pesaing untuk bread club, preorder, fermentation schedule, recipe costing dan production planning.
- **Laporan lengkap:** [research/RapiUsaha_product_opportunity_comparison_2026-11-12.md](research/RapiUsaha_product_opportunity_comparison_2026-11-12.md).
- **Langkah berikutnya yang disarankan (ASSISTANT_PROPOSAL):** riset gap kompetitor yang spesifik dengan sumber ulasan/komunitas; buktikan apakah problem berulang dan apakah pembeli mau membayar. Jangan menambah fitur RapiPO atau mengunci home bakery sebelum itu.
- **Tidak ada perubahan keputusan bisnis pengguna:** segmen, produk, harga, dan marketplace tetap OPEN. RapiPO tetap proposal asisten dan prototipe, bukan produk disetujui/tervalidasi.


## 0C. KERANGKA FILOSOFI BISNIS RAPIUSAHA — 2026-11-12

**Status: kerangka evaluasi yang diminta pengguna untuk dilanjutkan; detail rubrik adalah USULAN ASISTEN, bukan perubahan model bisnis atau persetujuan produk.**

- **Gagasan utama:** RapiUsaha bukan sekadar pembuat file digital; bisnis ini harus menghasilkan solusi yang layak dibayar. Produk digital adalah kemasan/distribusi solusi.
- **Prinsip:** mulai dari masalah nyata; jual hasil bukan fitur; kualitas harus terbukti dalam penggunaan; fokus pada satu pembeli/pekerjaan penting; MVP kecil dan berguna; harga terjangkau tetapi sehat secara margin; rancang produk dan distribusi bersama; bangun kepercayaan; validasi sebelum skala; perhitungkan unit economics dan beban support; bangun reputasi/katalog dari kebutuhan yang terbukti.
- **Hierarki bukti:** transaksi/repeat purchase paling kuat; preorder/deposit/komitmen membayar kuat; keluhan berulang + review/penjualan listing pesaing menengah; views/likes/followers/jumlah hasil pencarian lemah dan hanya membantu menentukan eksperimen.
- **Rubrik pemilihan kandidat:** intensitas/frekuensi masalah; bukti pembayaran; ruang diferensiasi; demonstrasi video; kelayakan phone-first; dukungan mandiri; margin bersih setelah fee; kelayakan marketplace/usia/KYC/payout. Hard gate bila jalur pembayaran/pengiriman tidak sah atau produk tidak berfungsi pada perangkat sasaran.
- **Penerapan pada RapiPO:** tetap kandidat eksperimen/prototipe, belum produk final USER_CONFIRMED. Mobile-first untuk home bakery merupakan hipotesis diferensiasi yang harus diuji, bukan fakta terbukti.
- **Dokumen lengkap:** [RapiUsaha_DASAR_FILOSOFI_DAN_FILTER_PRODUK.md](RapiUsaha_DASAR_FILOSOFI_DAN_FILTER_PRODUK.md).
- **Langkah berikutnya:** bandingkan beberapa segmen/produk menggunakan rubrik ini sebelum memperdalam RapiPO; jangan terus membangun RapiPO hanya karena prototipenya sudah dibuat.


## 0B. CHECKPOINT RISET MARKETPLACE RAPIUSAHA — 2026-11-12

**Status: RISET KANAL SELESAI; REKOMENDASI ASISTEN, BELUM DIKUNCI PENGGUNA.**

- **Kebutuhan USER_CONFIRMED:** marketplace yang punya pembeli ramai; produk digital harga terjangkau dengan kualitas sebagai pembeda; TikTok dan Instagram menggunakan video visual; pengguna tidak mau mengedit video dan meminta asisten menghasilkan video promosi siap unggah.
- **Rekomendasi kanal jangka panjang (ASSISTANT_PROPOSAL): Etsy.** Laporan resmi Etsy Q3 2026 menyebut sekitar 88 juta active buyers (TTM), GMS Etsy Marketplace US$2,6 miliar (+8,8% YoY). Etsy mendukung instant digital downloads dan Indonesia tersedia di Etsy Payments.
- **Kendala usia:** pemilik akun Etsy minimal 18 tahun. Penjual usia 13–17 harus memakai akun serta informasi keuangan orang tua/wali dengan izin dan pengawasan langsung. Karena pengguna tidak menginginkan keterlibatan wali, Etsy belum bisa dibuka secara mandiri sampai pengguna memenuhi syarat usia.
- **Shopee:** program resmi Penjualan Produk Digital yang ditemukan mendefinisikan barang sebagai e-voucher dari Partner yang disebut di lampiran, bukan jalur terbuka untuk file template biasa. Jangan menyamarkan download sebagai barang fisik atau mengirim file lewat metode di luar sistem.
- **Tokopedia:** skala traffic besar, tetapi kebijakan/fitur penjualan file digital generik untuk seller biasa belum berhasil diverifikasi. Jangan asumsikan spreadsheet/PDF bisa di-list.
- **Kreavo:** Terms mengizinkan pengguna minimal 17 tahun dan produk digital original, tetapi halaman publik yang diperiksa menunjukkan 0+ seller/produk dan 0 kreator/penjualan; belum ada bukti marketplace discovery ramai. Hanya opsi checkout/file delivery sementara yang diarahkan oleh traffic sendiri, setelah verifikasi payout.
- **Tokoge:** mendukung file digital dan auto-delivery, Terms menyebut fee 3,5%, tetapi usia minimum dan basis pembeli aktif belum jelas. Belum layak jadi kanal utama.
- **Kandidat produk untuk riset lanjutan (ASSISTANT_PROPOSAL):** sistem spreadsheet home bakery / cottage bakery. Hasil pencarian Etsy menampilkan 1.000+ item; satu listing digital bertanda Bestseller, lebih dari 20 views/24 jam, dan harga promo US$8,75; pesaing terdekat menawarkan sistem 14-tab. Shop penjual listing tersebut mempunyai 117,4 ribu penjualan keseluruhan toko—bukan angka penjualan listing itu. Ada pula banyak pesaing dengan harga promo sekitar US$3,50–US$15,94.
- **Biaya Etsy Indonesia:** listing US$0,20; transaction fee 6,5%; payment processing 4,5% + Rp7.000/order; deposit minimum Rp28.000; bila deposit di bawah threshold Rp1.400.000 ada deposit fee Rp28.000. Setup fee satu kali dan currency conversion fee dapat berlaku sesuai kondisi. Harga produk tidak boleh terlalu murah karena biaya tetap.
- **Sumber riset detail:** [research/RapiUsaha_marketplace_channel_2026-11-12.md](research/RapiUsaha_marketplace_channel_2026-11-12.md).
- **Status produk:** belum ada produk final, listing, penjualan, atau video promosi selesai. Home bakery masih kandidat, bukan keputusan final pengguna.
- **Langkah berikutnya:** bandingkan celah produk home bakery dengan segmen kandidat lain dan review pesaing; tentukan fitur pembeda yang benar-benar penting; bangun MVP phone-first; uji formula/flow; siapkan listing dan video promosi. Etsy baru digunakan ketika persyaratan usia/payout terpenuhi. Kreavo hanya opsi sementara bila akun 17+ dan payout BRI benar-benar eligible.
- **Batas keras:** jangan klaim angka traffic sebagai jumlah pembeli unik; jangan samakan jumlah hasil pencarian Etsy dengan penjualan; jangan buat akun dengan usia palsu atau data wali; jangan anggap platform kecil ramai tanpa bukti.


**Terakhir diperbarui:** 2026-11-12  
**Fungsi:** indeks dan checkpoint lintas proyek. Dokumen ini mencegah proyek, keputusan, dan status dari percakapan berbeda tercampur.

> Aturan utama: dokumen ini membantu menemukan sumber yang benar; ia tidak menggantikan bukti langsung dari repo, deployment, file, atau keputusan eksplisit terbaru pengguna. Status yang belum dicek ditandai apa adanya.

### 0B.1. CHECKPOINT PROTOTIPE RapiPO — 2026-11-12

**Status: PROTOTIPE AWAL SELESAI DIBUAT; BELUM FINAL / BELUM DIUJI PEMBELI.**

- **Produk kerja (ASSISTANT_PROPOSAL):** RapiPO — Home Bakery Order & Profit Tracker, untuk home bakery/cottage bakery kecil. Produk/segmen tetap rekomendasi asisten, belum persetujuan final eksplisit pengguna.
- **File prototipe XLSX dibuat di percakapan:** `RapiPO_Home_Bakery_Order_Profit_Tracker_Prototype.xlsx`.
- **Bagian workbook:** Start Here; Menu & Cost; Orders (10 kolom inti, dropdown produk/status); Dashboard.
- **Perhitungan formula yang diuji dengan data demo:** total order sales US$123.00; estimated gross profit US$60.20; unpaid balance US$68.00; active orders 3. Menu calculator contoh: cookies cost/unit US$1.45, suggested price US$2.42, selling price US$3.00.
- **Pemeriksaan kualitas:** tidak ditemukan error formula #REF!, #DIV/0!, #VALUE!, #NAME?, atau #N/A; render dashboard/orders diperiksa. Kompatibilitas Google Sheets Android belum diuji langsung. Data demo harus dihapus sebelum penggunaan nyata.
- **Video teaser dibuat di percakapan:** `RapiPO_Prototype_Promo_Vertical.mp4`, MP4 vertikal 720×1280, 30 fps, durasi 18 detik, musik instrumental orisinal. Ditandai sebagai prototype/demo, bukan iklan produk yang sudah diluncurkan.
- **Batasan video/prototipe:** angka dashboard dan contoh order adalah data demo, tidak boleh diklaim sebagai hasil pelanggan. Tidak ada penjualan/validasi yang terjadi.
- **Belum tersimpan sebagai binary di repo:** XLSX dan MP4 tersedia sebagai file lampiran percakapan; checkpoint dan riset disimpan di repo. Jangan klaim binary sudah di-commit ke GitHub.
- **Yang belum selesai:** order notes/change tracking; tes formula/dropdown di Google Sheets Android; versi Bahasa Indonesia; panduan pembeli; pembuktian willingness-to-pay; verifikasi payout Kreavo; rencana pembukaan Etsy ketika syarat usia terpenuhi; listing final.
- **Langkah berikutnya:** uji workbook di Google Sheets Android dengan mengganti satu produk, menambah order, mengubah quantity dan DP; setelah temuan uji, revisi workbook, tambahkan notes perubahan pesanan, buat Bahasa Indonesia dan PDF guide, lalu verifikasi fee/payout/channel sebelum listing. Jangan menjual file prototype ini sebagai final sebelum tes.
- **Sumber riset:** `research/RapiUsaha_marketplace_channel_2026-11-12.md` (terakhir diverifikasi; SHA b7365815ff131c3d80a528112e950017552b780e).
- **Rekomendasi kanal:** Etsy untuk discovery jangka panjang setelah umur 18 dan payout/KYC eligible; Kreavo hanya checkout interim bila jalur 17+ dan rekening diverifikasi. Shopee/TikTok Shop/Tokopedia bukan jalur default file download.
- **Harga:** belum dikunci. Benchmark Etsy pesaing diskon saat diperiksa sekitar US$3.50–US$15.94; jangan tetapkan harga sebelum hitung biaya dan nilai pembeda.

## 0A. CHECKPOINT PROFIL P-DIGITAL / RapiUsaha — 2026-11-12

**Status persetujuan: USER_CONFIRMED.** Pengguna menyetujui rangkuman profil bisnis secara eksplisit pada percakapan 2026-11-12. Catatan ini merekam preferensi dan batasan pengguna; bukan persetujuan atas produk tertentu.

### Strategi produk dan pemasaran (USER_CONFIRMED; 2026-11-12)

- **Harga:** produk harus cukup terjangkau untuk menarik pembeli awal; angka/harga final belum diputuskan.
- **Pembeda utama:** kualitas produk dan pengalaman penggunaan, bukan hanya perang harga.
- **Saluran penjualan:** produk ditempatkan di marketplace yang cocok untuk file/produk digital; marketplace final belum dipilih.
- **Promosi:** TikTok dan Instagram digunakan untuk promosi dengan video visual yang menarik, bukan hanya posting teks.
- **Produksi video:** pengguna tidak ingin mengedit video sendiri; asisten diminta bertanggung jawab membuat materi video promosi sampai siap diunggah. Ini adalah kebutuhan produksi, bukan bukti bahwa video sudah dibuat.
- **OPEN:** produk spesifik, niche pembeli, struktur paket, harga peluncuran/harga normal, marketplace final, dan konsep video kampanye.
- **Batas:** jangan mengunci produk atau platform sebelum riset bukti permintaan, kompetisi, fee, syarat usia/KYC/payout, dan uji alur checkout.

### Profil yang dikonfirmasi pengguna
- **Model bisnis:** membuat produk digital sendiri dan menjualnya berulang kali.
- **Target pelanggan:** belum ditentukan; pilih berdasarkan peluang pasar terbaik.
- **Kategori/masalah produk:** belum ditentukan; riset dan pilih yang paling layak secara komersial.
- **Format produk dan pasar geografis/bahasa:** bebas; pilih yang paling menguntungkan dan realistis.
- **Saluran akuisisi yang diterima:** marketplace/toko digital; website/halaman penjualan dengan checkout otomatis; Google/SEO; Instagram/TikTok hanya sebagai promosi produk utama.
- **Yang ingin dihindari:** telepon/video call pelanggan; cold-DM atau mengejar prospek satu per satu; banyak chat pelanggan setiap hari; teknologi rumit/tidak andal; pekerjaan manual berulang untuk setiap pelanggan.
- **Dukungan pascapembelian:** minimal; produk harus self-service.
- **Modal awal:** Rp0.
- **Waktu kerja:** lebih dari 6 jam per hari.
- **Target pemasukan pertama:** secepat mungkin, idealnya kurang dari 1 bulan.
- **Gaya operasional:** bersedia bekerja keras pada tahap awal agar operasional dapat diotomatisasi kemudian.
- **Target pendapatan 30 hari:** Rp100.000–Rp300.000.
- **Target jangka panjang:** mengejar pendapatan bersih bulanan setinggi mungkin.
- **Prioritas trade-off:** SEIMBANG — tetap mengejar pemasukan pertama dalam 30 hari tanpa mengorbankan potensi jangka panjang yang besar.
- **Pembayaran/payout:** pengguna ingin mengelola sendiri dan tidak menghendaki keterlibatan orang tua/wali.
- **Akun pembayaran yang tersedia (dikonfirmasi pengguna, 2026-11-12):** pengguna menyatakan sudah memiliki rekening BRI dan e-wallet DANA. Kepemilikan atas nama sendiri, status verifikasi/KYC, fitur penerimaan pembayaran, dan kelayakan menerima payout bisnis belum diperiksa; jangan mengasumsikannya.
- **Batas legal/operasional:** pengguna berusia 17 tahun pada saat pencatatan. Persyaratan usia, KYC, kepemilikan akun, rekening, dan payout wajib diperiksa berdasarkan ketentuan resmi setiap platform. Jangan menyarankan pemalsuan usia atau penggunaan identitas orang lain. Jika jalur mandiri tidak memenuhi syarat, jelaskan hambatan dan cari alternatif yang sah; jangan menganggap pengguna setuju melibatkan wali.

### Status keputusan dan hal yang belum diputuskan
- **USER_CONFIRMED:** model produk digital buatan sendiri, target pelanggan/jenis produk/format/pasar masih terbuka untuk riset, kanal akuisisi yang diterima, batas interaksi dan dukungan, modal/waktu, target pendapatan, prioritas seimbang, serta preferensi pengelolaan payout mandiri.
- **OPEN:** produk spesifik, masalah pelanggan, segmen prioritas, harga, platform penjualan, metode checkout/payout yang memenuhi syarat usia, dan strategi distribusi final.
- **ASSISTANT_PROPOSAL:** Paket Operasional Jualan WhatsApp dan komponen-komponennya tetap hipotesis awal saja; belum disetujui sebagai produk final.
- **REJECTED/ARCHIVED:** bisnis jasa/studio konten dan bisnis yang menjual konten. Konten hanya boleh menjadi promosi produk utama.
- **Bukan bukti validasi:** belum ada produk final, pelanggan berbayar, atau penjualan yang terverifikasi.

### Checkpoint pekerjaan dan langkah berikutnya
- **Tanggal:** 2026-11-12.
- **Status:** profil kebutuhan dan batasan bisnis selesai dirangkum serta disetujui pengguna; riset pemilihan produk belum selesai.
- **Tujuan saat ini:** memilih peluang produk digital B2C yang punya bukti permintaan dan kesediaan membayar, dapat dibuat dengan modal nol, minim dukungan manual, serta punya jalur pembayaran/payout yang legal untuk kondisi usia pengguna.
- **Yang benar-benar berubah:** profil pengguna dicatat sebagai keputusan USER_CONFIRMED di checkpoint ini.
- **Bukti persetujuan:** pesan pengguna, “Setuju. Simpan profil bisnis ini ke GitHub, lalu verifikasi hasilnya.” setelah menyetujui rangkuman profil.
- **Belum diverifikasi:** kelayakan usia dan payout platform mana pun; permintaan pasar, willingness-to-pay, kompetisi, harga, serta produk yang paling layak.
- **Langkah berikutnya:** riset komparatif peluang produk digital dengan sumber primer/terkini; evaluasi permintaan, kompetisi, biaya pembuatan, kecepatan menuju penjualan pertama, dukungan pelanggan, dan syarat usia/payout. Berikan satu rekomendasi utama beserta bukti dan alternatif sebelum membangun produk. Jangan mengunci produk atau segmen tanpa persetujuan eksplisit pengguna.
- **Larangan/perangkap:** jangan mengubah RapiUsaha menjadi jasa konten, affiliate software, rental, atau SaaS; jangan menganggap konsep WhatsApp sebagai pilihan final; jangan menyarankan akun pembayaran dengan usia palsu atau rekening pihak lain tanpa dasar izin platform.

## 0. ARAH BISNIS AKTIF — STATUS TERKOREKSI 2026-11-12

**Status sekarang: P-DIGITAL / RapiUsaha adalah arah kerja yang sedang dilanjutkan.**

- **Dasar pemulihan:** tepat sebelum akun Instagram dibuat, arah yang dibahas adalah bisnis **produk digital B2C untuk UMKM**, bukan jasa produksi konten untuk klien. Pengguna meminta nama akun, memilih nama brand **RapiUsaha**, lalu menyatakan akun Instagram sudah dibuat.
- **Penawaran awal yang pernah diusulkan:** paket digital operasional jualan WhatsApp untuk UMKM/penjual rumahan: pencatatan order dan pembayaran, kalkulator harga/modal/margin, template katalog, template pesan pelanggan, serta panduan singkat.
- **Batas kepastian:** RapiUsaha dan arah produk digital adalah arah kerja yang sedang dilanjutkan berdasarkan konteks langsung. Rincian produk tersebut masih HIPOTESIS/MVP AWAL; belum ada bukti riset willingness-to-pay, produk selesai, penjualan, atau keputusan bahwa setiap fitur wajib masuk.
- **Bukan bisnis aktif:** P-CONTENT (jasa/studio produksi konten untuk UMKM) TIDAK DISETUJUI pengguna dan tidak boleh diperlakukan sebagai keputusan. P-DISTRO/P-RENTAL dan SignageQuote adalah riwayat/proyek berbeda, bukan arah otomatis RapiUsaha.
- **Larangan:** jangan mengubah RapiUsaha menjadi jasa social-media/content studio, affiliate software, rental, atau SaaS tanpa instruksi eksplisit pengguna.
- **Langkah berikutnya:** lanjutkan dari akun RapiUsaha yang sudah dibuat; rapikan identitas profil lalu bangun MVP digital product yang paling kecil untuk diuji. Jangan bikin akun baru.

Bagian ini adalah checkpoint pemulihan terbaru. Jika ada paragraf lama di bawah yang menyatakan P-CONTENT atau P-DISTRO sebagai bisnis utama, anggap status itu sudah digantikan oleh bagian ini dan koreksi pada entri proyeknya.

### P-DIGITAL — RapiUsaha (PROYEK AKTIF)
- **Preferensi bisnis eksplisit pengguna (2026-11-12):** pengguna TIDAK SUKA bisnis/produk/jasa yang berhubungan dengan pembuatan konten. Konten hanya boleh digunakan sebagai materi PROMOSI untuk menjual produk/bisnis utama. Jangan pernah menawarkan studio konten, jasa social-media management, copywriting/content production sebagai bisnis, atau menjual paket konten kecuali pengguna sendiri secara eksplisit meminta.

- **Status:** ARAH KERJA AKTIF per 2026-11-12; brand akun Instagram **RapiUsaha** sudah dibuat pengguna. Handle persis dan kelengkapan profil belum diverifikasi.
- **Model:** produk digital B2C untuk membantu UMKM/penjual kecil. Produk dibuat sekali, lalu dijual dan dikirim secara digital; bukan jasa mengelola akun atau membuat konten media sosial untuk klien.
- **Konsep produk awal:** **Paket Operasional Jualan WhatsApp**. Komponen kandidat: lembar pencatatan pesanan/pembayaran, kalkulator harga dan margin, template katalog/daftar produk, template pesan untuk pelanggan, dan panduan penggunaan singkat.
- **Status produk:** konsep MVP dari usulan asisten; belum selesai dibuat, belum diuji di HP pengguna, belum dipasarkan, belum ada penjualan. Jangan menyebutnya produk tervalidasi atau menganggap semua komponen final.
- **Target awal untuk validasi:** penjual rumahan/UMKM yang menerima pesanan lewat WhatsApp. Pilih satu masalah yang paling menyakitkan dahulu; jangan mengembangkan paket besar sebelum diuji.
- **Prinsip pengerjaan:** phone-first, modal serendah mungkin, gunakan alat gratis, file mudah dibuka di Android, minim langkah pelanggan, dan tidak mengandalkan langganan/iklan berbayar.
- **Pemasaran:** akun Instagram RapiUsaha sebagai kanal awal; buat konten edukasi/demonstrasi untuk menjual produk digital sendiri. Scheduling otomatis boleh dipakai setelah akun terhubung; jangan klaim otomatisasi sudah aktif tanpa bukti.
- **Batas:** jangan pivot menjadi jasa studio konten, affiliate software, rental, atau SaaS tanpa persetujuan eksplisit pengguna.
- **Langkah berikutnya:** tentukan satu use-case MVP dan buat contoh produk yang benar-benar berfungsi di Android; setelah itu uji respons dan harga sebelum menambah fitur.

## 1. Proyek yang terpisah

### P-CONTENT — Studio batch konten siap-publikasi untuk UMKM (TIDAK AKTIF / TIDAK DISETUJUI)
- **Status:** DITARIK dari status proyek utama pada 2026-11-12. Catatan/rencana ini dibuat oleh asisten, tetapi tidak ada bukti pengguna menyetujui model jasa studio konten. Jangan jalankan atau perlakukan sebagai bisnis pengguna.
- **Tujuan:** mendapatkan pembayaran pertama secepat mungkin dengan modal rendah, alur kerja phone-first, minim meeting/cold-DM, dan produksi konten yang bisa dibatch dengan AI/template. Pemasaran disiapkan melalui kalender konten dan scheduling otomatis; jangan mengklaim sudah berjalan sebelum akun terhubung.
- **Penawaran awal:** paket produksi batch konten siap unggah untuk UMKM; target uji pertama penjual makanan rumahan/snack/frozen food/katering kecil yang sudah berjualan lewat WhatsApp dan punya foto produk sendiri.
- **Produk pilot:** Rp99.000 sebagai harga uji untuk 4 feed/carousel + 4 variasi story + caption/CTA + kalender 7 hari + satu revisi sederhana. Paket bulanan uji Rp249.000 untuk 8 feed/carousel + 8 story + 4 skrip Reels + caption/CTA + kalender 30 hari. Harga ini hipotesis eksperimen, bukan harga pasar terverifikasi. Pelanggan mengunggah sendiri; tidak termasuk admin akun, balas DM, iklan, foto/video shooting, atau jaminan penjualan.
- **Dasar pasar:** ringkasan BPS Statistik E-Commerce 2024 menyatakan 94,76% usaha e-commerce menjual lewat pesan instan, 29,66% lewat media sosial, dan 17,23% lewat marketplace. Ini menunjukkan relevansi audiens WhatsApp/social, bukan bukti konversi jasa konten. Sumber: https://www.bps.go.id/id/publication/2025/11/28/647323224ecc656c2933571b/statistik-e-commerce-2024.html ; https://databoks.katadata.co.id/en/technology-telecommunications/statistics/692fe011736e4/instant-messaging-main-channel-for-e-commerce-businesses-in-indonesia
- **Benchmark publik:** MulaiKarya mempublikasikan paket Rp250.000/bulan untuk 8 feed + 4 story beserta caption/hashtag/kalender/report; Lakara mempublikasikan Rp1.500.000/bulan untuk 12 aset konten dengan copywriting dan scheduling. Ini contoh kompetitor, bukan rata-rata pasar. Sumber: https://mulaikarya.com/ ; https://lakara.id/pricing
- **Mesin pemasaran:** konten demo/edukasi di satu brand sosial → CTA ke halaman order → brief/form dan pembayaran → AI draft + template Canva → QC → kirim folder konten. Metricool Free mendukung 1 brand dan hingga 20 post/bulan. Status sesi 2026-11-12: brand ID 8048597 terdeteksi tetapi belum ada jejaring sosial tersambung; belum ada post terjadwal. Hubungkan di https://app.metricool.com/brands/connections?blogId=8048597. Sumber: https://metricool.com/pricing/ ; https://help.metricool.com/main-differences-between-free-and-paid-plans-bl0v9
- **Checkout/payout potensial:** Lynk.id, tetapi halaman Terms memiliki perbedaan bahasa soal usia minimum; keduanya menyebut izin orang tua/wali bagi pengguna di bawah usia dewasa. Pengguna berusia 17 tahun harus meminta konfirmasi tertulis dan izin wali sebelum membuat akun/menarik dana. Ketentuan menyebut minimum pencairan Rp100.000 dan maksimal 3 hari kerja; pricing menyebut fee transaksi 5% akun gratis dan biaya withdrawal Rp5.000. Pastikan di dashboard. Sumber: https://www.lynk.id/terms ; https://www.lynk.id/pricing/
- **Validasi:** belum ada pelanggan atau pemasukan. Buat portfolio DEMO enam konten dan landing page satu layar; uji publikasi 14 hari; ukur reach, profile visits, clicks, brief, order berbayar, dan waktu produksi. GO awal bila ada setidaknya satu order berbayar dari luar keluarga/teman dekat dan deliverable selesai sesuai janji. Bila tidak, revisi segmen/offer berdasarkan data.
- **Riset lengkap:** https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa/blob/main/research/P-CONTENT-pivot-2026-11-12.md
- **Campaign historis:** `marketing/P-CONTENT-launch-campaign-001.md` adalah arsip untuk usulan jasa konten yang TIDAK DISETUJUI; jangan dipublikasikan atau dilanjutkan.
- **Status arsip:** semua detail penawaran, harga, dan campaign P-CONTENT di bawah adalah artefak usulan asisten yang TIDAK DISETUJUI; jangan gunakan sebagai basis tindakan bisnis kecuali pengguna meminta secara eksplisit.
 
### P-DISTRO — Distribusi software B2B lintas industri (ARSIP / TIDAK AKTIF)

- **Status:** BUKAN arah kerja RapiUsaha saat ini. Riset Jubelio/Mekari/Olsera dan campaign penjual multichannel disimpan sebagai arsip; jangan lanjutkan sebagai bisnis aktif kecuali pengguna secara eksplisit memintanya.
- **Tujuan jangka pendek:** menghasilkan pemasukan/komisi pertama secepat mungkin, dengan modal rendah dan proses yang dapat diotomatisasi. Pilih program berdasarkan (1) kecepatan mendapat pelanggan pertama, (2) nilai komisi dan ambang payout, (3) tim vendor membantu demo/closing, (4) kelayakan peserta usia 17 tahun dan metode pencairan, (5) mudahnya dipasarkan dengan konten digital.
- **Tujuan jangka panjang:** mempunyai beberapa sumber komisi/partner lintas industri. Rental, e-commerce, accounting, HR, POS, CRM, dan software vertikal lainnya adalah kandidat pasar; jangan mengunci bisnis pada satu industri karena percobaan pertama.
- **Catatan sejarah:** instruksi lintas-industri ini berlaku pada tahap sebelumnya, tetapi kemudian arah percakapan bergeser ke pembentukan brand RapiUsaha dan produk digital B2C. Jangan mengaktifkan kembali P-DISTRO berdasarkan catatan lama ini.
- **Kandidat lintas industri yang sudah diteliti:**
  1. **Penjual online/e-commerce omnichannel — Jubelio Ambassador (prioritas uji pertama).** Program promosi sederhana: ambassador membagikan kode, tim Jubelio mengurus proses setelah lead diserahkan. Komisi publik Rp200.000/klien setelah bisnis menggunakan Jubelio; penarikan minimal 2 pengguna berkode; pembayaran awal bulan berikutnya; akun harus diverifikasi, akun sosial/blog publik, rekening pencairan harus sama dengan nama ambassador; pendaftaran gratis; verifikasi hingga 3 hari kerja. Program menyatakan siapa pun dapat bergabung selama memiliki media sosial publik, tetapi batas usia 17 tahun tidak dinyatakan secara eksplisit dan belum dikonfirmasi tertulis. Jangan menganggap pendaftar 17 tahun dapat bergabung; jangan pakai identitas/rekening orang lain tanpa persetujuan resmi. Draft pertanyaan ke support@jubelio.com dibuat di Gmail, draft ID `r6592441606336964955`, BELUM DIKIRIM. Sumber: https://jubelio.com/jubelio-ambassador/ ; https://jubelio.com/affiliate-jubelio/
  2. **Akuntansi/pembukuan bisnis — Mekari Jurnal Referral Agent (prioritas uji kedua).** Program resmi menyebut komisi 15% dari nilai kontrak referral yang berhasil close-deal berlangganan Jurnal, program gratis, pendaftar individu/freelancer/content creators cocok, kode/link referral dan dashboard tersedia, Mekari memproses penjualan. Harga resmi menunjukkan mulai Rp399.000/bulan pada pricing overview, sedangkan halaman produk pembukuan menyebut sekitar Rp449.000/bulan; jangan menetapkan komisi nominal sebelum vendor memastikan harga dan dasar kontraknya. Program komunitas Mekari punya aturan KTP/rekening H+14 dan payout paling lambat H+14 setelah Won, tetapi belum dipastikan bahwa semua aturan itu persis berlaku pada Referral Agent. Kelayakan usia 17 tahun, rekening wali, KTP, serta waktu payout perlu konfirmasi tertulis. Sumber: https://mekari.com/partnership/referral/ ; https://mekari.com/pricing/ ; https://mekari.com/produk/jurnal/pembukuan/ ; https://community.mekari.com/gamifikasi/referral-community/
  3. **POS/retail/F&B — Olsera Affiliate Hunter (kandidat baru, hasil riset 2026-11-12).** Halaman partner resmi terbaru menyebut komisi Hunter 10% setelah trial 14 hari berakhir dan toko memperpanjang; nominal publik: Basic Rp15.000/bulan atau Rp128.000/tahun; Premium Rp24.000/bulan atau Rp198.000/tahun; Pro Rp32.000/bulan atau Rp268.000/tahun. Partner dapat naik ke Warrior dengan komisi yang dipublikasikan 20%: Basic Rp31.000/bulan atau Rp257.000/tahun; Premium Rp49.000/bulan atau Rp397.000/tahun; Pro Rp65.000/bulan atau Rp537.000/tahun. Halaman bantuan resmi menunjukkan partner dapat membuat link khusus, melihat saldo komisi, dan menambahkan rekening bank untuk pencairan. Minimum payout, jadwal pencairan, syarat KTP/rekening, serta kelayakan peserta 17 tahun BELUM DIPUBLIKASIKAN/DIKONFIRMASI. Syarat umum akun pelanggan Olsera menyatakan pendaftar individu minimal 18 tahun, tetapi itu tidak otomatis membuktikan syarat akun affiliate/partner sama; wajib tanya tim partner. Portal: https://partner.olsera.com/en/sign-up ; halaman program: https://www.olsera.com/id/partner/intro ; panduan saldo/link: https://crm.olsera.co.id/knowledge-base/article/cara-gabung-partner ; syarat akun pelanggan (ruang lingkup berbeda): https://www.olsera.com/id/terms-conditions
- **Sinyal pasar e-commerce:** dokumen Kemendag yang mengutip Statistik E-Commerce BPS 2024 menyatakan 42,02% usaha melakukan penjualan online pada 2024 dan 57,98% tidak. INDEF Mei 2026, mengutip BPS/APJII, menyebut sekitar 4,4 juta usaha pengguna e-commerce pada 2024. Ini membuktikan pasar cukup luas sebagai arah riset, bukan jumlah calon pelanggan software atau jaminan konversi. Sumber: https://bkperdag.kemendag.go.id/unduhan-file/a11ba920-123c-4a4f-856b-cdbb1f0f64c4 ; https://indef.or.id/wp-content/uploads/2026/05/Monthly-Brief_CODES-INDEF_Mei-2026.pdf
- **Harga/fitur Jubelio:** harga resmi yang dipublikasikan Rp150 per order yang diproses; unlimited SKU, channel/marketplace/webstore/POS, pengguna/lokasi, serta fitur analytics, akuntansi, WMS, Jubelio POS/Chat/Shipment. Syarat promo/kontrak harus dikonfirmasi sebelum menyatakan biaya final. Sumber: https://jubelio.com/harga/
- **Strategi pemasaran otomatis:** jangan bergantung pada cold-DM. Gunakan satu content engine lintas produk: satu riset masalah diubah menjadi short-video script, carousel, artikel ringkas, dan post teks; jadwalkan batch ke akun sosial; setiap konten memakai link UTM menuju landing page/kalkulator khusus vertikal; track reach → click → referral/lead → won → komisi; gunakan vendor yang mengerjakan demo/closing setelah lead diserahkan.
- **Integrasi marketing:** koneksi Metricool dapat dipanggil dari sesi ini dan brand ID `8048597` terdeteksi, tetapi percobaan membaca brand settings gagal dengan pesan bahwa belum ada satu pun jejaring sosial yang terhubung. Jadi Metricool tersedia/terhubung sebagai layanan, namun akun sosial BELUM tersambung dan belum ada post yang dapat dijadwalkan. Link koneksi yang dikembalikan Metricool: https://app.metricool.com/brands/connections?blogId=8048597. Jangan mengklaim scheduling/publikasi aktif sampai minimal satu akun sosial tersambung dan post terjadwal benar-benar terlihat.
- **Konten pemasaran pertama:** file `marketing/campaign-001-multichannel-seller.md` dibuat dan diverifikasi pada 2026-11-12. Berisi rationale segmen, target audience, batch 7 konten, CTA, dan alur marketing otomatis. Status: DRAFT, BELUM DIPUBLIKASIKAN, karena referral link dan social integrations belum aktif. Commit: `e4a2e2436ea76db777a21192c4818fcfab0f368f`. Link: https://github.com/christianomomongan82-lang/terserah-mau-taruh-apa/blob/main/marketing/campaign-001-multichannel-seller.md
- **Batas otomatisasi:** scheduling, repurposing dan analytics dapat diotomatisasi melalui tool terhubung; tidak boleh spam auto-DM, memalsukan testimoni, membeli followers, atau menjamin hasil. Persetujuan konten, respons pada prospek yang meminta penjelasan, verifikasi eligibility, dan pencairan komisi tetap perlu kontrol.
- **Automation riset partner yang sudah aktif:** tetap gunakan automation mingguan `Pantau program partner software B2B` ID `6af47a587e788191ab32ee88d34d47fe` (Asia/Makassar, Rabu 09.00). Jangan membuat duplikat.
- **Status langkah berikutnya:** TIDAK AKTIF. Pertahankan riset sebagai arsip; arah kerja RapiUsaha saat ini tercatat pada P-DIGITAL.
- **Larangan fokus:** jangan terus mengunci diri pada rental, jangan memindahkan target ke rental event/sound system sebagai pivot tanpa izin, jangan membangun website besar sebelum program partner valid, dan jangan menggunakan nama pribadi pengguna untuk brand/domain/situs.
 
### P-RENTAL — Distribusi software B2B untuk bisnis rental
- **Status:** ARSIP/PROYEK LAMA; bukan bisnis aktif RapiUsaha. Aset dan riset dipertahankan hanya sebagai referensi jika pengguna secara eksplisit ingin membukanya kembali.
- **Identitas proyek:** bisnis distribusi software B2B berbasis komisi/partner, bukan perusahaan jasa rental dan bukan sekadar website RentalOps.
- **Arah jangka panjang:** membangun perusahaan distribusi software B2B dengan beberapa sumber komisi/vendor. Aset inti: saluran akuisisi pelanggan, konten informatif, alat bantu interaktif, dan sistem referral.
- **Segmen yang sedang dievaluasi:** pemilik bisnis rental. Jangan anggap seluruh pemilik rental sebagai target yang cocok untuk setiap produk; segmentasi harus berdasarkan kebutuhan operasional dan percakapan pelanggan.
- **Kandidat produk/vendor:
  - **CSHub:** pilihan sementara saat pembahasan terakhir karena potensi komisi recurring. Harga acuan Rp300.000/bulan; komisi yang dipublikasikan 20% (ilustrasi Rp60.000/pelanggan aktif/bulan). Halaman afiliasi punya informasi bertentangan tentang tingkat/durasi komisi. Angka ini belum terverifikasi dan bukan pendapatan terjamin.
  - **Wicom / SakuKapling:** kandidat pembanding; software developer kavling, harga acuan Rp1.200.000/tahun dan komisi terpublikasi 30% (ilustrasi Rp360.000/penjualan). Syarat usia juga belum terverifikasi.
  - **MyRental dan kecocokan produk:** kesimpulan lama bahwa CSHub hanya relevan untuk pengelolaan chat perlu diperbarui. Dokumentasi resmi cshub kini secara eksplisit menjelaskan produk sewa harian per tipe kendaraan, jumlah unit, ketersediaan berdasarkan rentang tanggal, pencegahan bentrok sebelum booking disimpan, hitung tarif harian, checkout/tagihan, inbox multi-admin, AI untuk chat di luar jam kerja, dan tiket kerusakan/denda. Ini berarti CSHub juga punya fungsi operasional rental, bukan sekadar CRM chat. Belum ada uji produk langsung; klaim ini bersumber dari halaman solusi dan dokumentasi vendor. Kandidat segmen awal yang paling masuk akal untuk diuji: rental mobil/motor multi-unit dengan chat masuk rutin, potensi bentrok booking, admin lebih dari satu, atau permintaan malam hari. Ini hipotesis kecocokan produk, bukan bukti bahwa segmen tersebut paling besar atau paling menguntungkan. Jangan menetapkan cutoff jumlah armada tanpa riset pelanggan.
- **Aset akuisisi yang sudah dibuat menurut percakapan sumber (2026-11-11):** Kalkulator Potensi Booking Rental, file HTML mandiri untuk menghitung potensi keuntungan yang hilang akibat calon pelanggan tidak terlayani atau booking tidak ditindaklanjuti. File aktual/lokasinya belum dipetakan ke repo atau Worker.
- **Kelayakan affiliate CSHub (BLOKIR GO):** email pertama TERKIRIM pada 2026-11-11 14:30 UTC / 22:30 WITA ke `care@cloudchat.id`, tetapi alamat/domain itu milik layanan CloudChat/PT Cloud Chat Indonesia, bukan CSHub; email itu salah alamat. Email kedua yang benar-benar terkirim ke operator CSHub `manufacturdigitalhub@gmail.com` (Manufactur Digital Hub) pada 2026-11-12; Gmail Message ID `1a1cb91653fc1426`, thread `1a1cb8019cf21fbb`. Isinya meminta konfirmasi usia 17/persetujuan wali, rekening pencairan, rate setiap tier, durasi komisi, atribusi/refund, aturan payout, biaya dan target. Sampai pemeriksaan terakhir belum ada balasan yang terlihat. Kontak `privacy@cshub.id` hanya untuk hal privasi/penghapusan akun; jangan pakai sebagai kontak affiliate. Jangan menyatakan eligibility/komisi terkonfirmasi sebelum ada jawaban tertulis.
- **Ketentuan affiliate yang dipublikasikan CSHub (diverifikasi 2026-11-12):** halaman affiliate mensimulasikan 20% recurring untuk Starter Rp300.000/bulan dan menyebut persentase naik mengikuti tier, tetapi angka rate setiap tier tidak dipublikasikan. Durasi komisi kontradiktif di halaman yang sama: satu bagian menyebut sampai bulan ke-24, FAQ menyebut tanpa batas selama pelanggan aktif. Pencairan juga kontradiktif: FAQ promosi menyebut tanpa minimum dan 1×24 jam kerja, sedangkan dokumentasi operasional menyebut minimum Rp20.000 dan diproses 1–3 hari kerja. Perlu jawaban resmi tertulis; gunakan ketentuan dokumentasi operasional sebagai detail yang lebih konkret, tetapi tetap jangan anggap final.
- **Usia/identitas:** Syarat & Ketentuan yang dibuka (terakhir diperbarui 18 Oktober 2026) tidak menampilkan batas usia spesifik. Panduan pendaftaran menjelaskan nama, nomor WA, email, dan persetujuan T&C/privasi, tetapi tidak menyatakan umur minimum. Ketiadaan klausul di halaman yang dibaca bukan bukti bahwa peserta 17 tahun diperbolehkan; tetap perlu konfirmasi langsung.
- **Keputusan bisnis terakhir yang terkunci:** BELUM GO untuk membangun website distribusi besar-besaran sebelum syarat komisi dan kelayakan peserta jelas. CSHub adalah pilihan sementara, bukan keputusan final. Langkah bernilai berikutnya: selesaikan kelayakan partner dan pilih segmen rental paling menguntungkan berdasarkan bukti.
- **Pemantauan partner software — TERVERIFIKASI AKTIF:** automation "Pantau program partner software B2B" ditemukan di sistem Automations pada 2026-11-12. ID `6af47a587e788191ab32ee88d34d47fe`, `is_enabled=true`, timezone `Asia/Makassar`, jadwal Rabu mingguan pukul 09.00, `next_run_time=2026-11-18T09:00:00+08:00`. Prompt tersimpan mencakup batas usia 17 tahun, pemeriksaan ketentuan resmi, CSHub dan Wicom/SakuKapling sebagai benchmark, maksimal tiga kandidat, serta laporan mingguan meskipun tidak ada kandidat baru. Ini automation yang benar-benar ditemukan, bukan sekadar klaim percakapan.
- **Nama RentalOps / website:** https://rentalops-site.usahalab-id.workers.dev. Ini aset landing page untuk jalur akuisisi, bukan definisi keseluruhan bisnis distribusi B2B.
- **Alasan penggantian alamat (dipulihkan dari percakapan share 2026-11-11):** pengguna meminta alamat website yang sebelumnya memuat nama akun pribadi diganti agar tidak menampilkan nama pribadi. Kesalahan yang terjadi: asisten sempat memberikan alamat SignageQuote ketika yang diminta pengguna adalah RentalOps. Jangan ulangi; cocokkan nama proyek, URL, dan isi halaman sebelum membagikan link.
- **Verifikasi live 2026-11-12:** `https://rentalops-site.usahalab-id.workers.dev` berhasil dibuka dan judul/isi yang tampil adalah **RentalOps — Booking Rental Lebih Rapi**. `https://signagequote.usahalab-id.workers.dev` berhasil dibuka dan judul/isi yang tampil adalah **SignageQuote — Estimator Bengkel Signage**. Kedua URL saat ini menuju proyek yang benar dan tidak tertukar.
- **Batas verifikasi live:** pembukaan halaman dan kecocokan judul/isi sudah dicek; ini belum membuktikan CTA, tautan referral, komisi, ataupun proses konversi berfungsi.
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker rentalops-site ada pada akun Cloudflare yang terhubung; created_on=2026-11-11T14:38:12Z, modified_on=2026-11-11T15:21:16Z, last_deployed_from=api. Ini membuktikan resource Worker ada, bukan bahwa halaman/CTA/referral saat ini benar atau berfungsi.
- **Pemetaan sumber kode:** daftar repo GitHub dari akun yang terhubung tidak menampilkan repo bernama RentalOps. Repo sumber/deployment pipeline belum dipetakan; jangan menebak repo dari hasil pencarian publik.
- **Batas keras:** jangan mengubah proyek ini menjadi SignageQuote atau SiapKirim. Jangan menyatakan produk/komisi/referral sudah aktif tanpa verifikasi. Jangan menggunakan nama pribadi pengguna pada nama brand, domain, website, atau salam.
- **Sumber pemulihan wajib:** percakapan share tanggal 2026-11-11 yang diberikan pengguna: https://chatgpt.com/share/6af49862-6ef4-83ec-b3ae-eb207ee75a9a?ogimg=plain. Isi pentingnya diringkas di bagian ini agar arah bisnis tetap pulih lintas chat.
- **Status verifikasi 2026-11-12:** arah dan keputusan bisnis di atas dipulihkan dari percakapan share; kelayakan CSHub, email terkirim/tidak, file kalkulator, status automation, isi website live, CTA, dan referral tetap perlu diverifikasi dari sumber masing-masing sebelum tindakan.
### Checkpoint P-RENTAL — 2026-11-12 (HISTORIS / SUDAH DIGANTIKAN)
- **Status:** checkpoint ini merekam fokus rental pada tahap sebelumnya. Sudah digantikan oleh arah kerja P-DIGITAL / RapiUsaha pada 2026-11-12; jangan perlakukan sebagai fokus sekarang.
- **Prioritas hasil:** first paying customer dan komisi pertama. Bukan ukuran pasar nasional, brand baru, atau fitur website tambahan. Hindari pekerjaan yang tidak memperpendek jalur ke pelanggan yang membayar.
- **Segmen aktif tetap:** operator rental mobil/motor dengan beberapa unit, arus inquiry WhatsApp yang nyata, dan risiko kehilangan lead atau bentrok booking karena admin/jadwal tersebar. Kriteria adalah hipotesis penyaringan; jangan mengunci batas minimal armada atau volume chat tanpa bukti.
- **Koreksi kesalahan asisten:** riset rental sound system/lighting/AV event yang sempat muncul pada sesi 2026-11-12 adalah pivot yang tidak diminta dan BUKAN keputusan pengguna. Jangan jadikan sebagai segmen aktif atau mengganti target rental mobil/motor. Catatan penelitian event-rental boleh dipertahankan hanya sebagai eksplorasi sampingan, bukan pekerjaan berikutnya.
- **Aset yang sudah ada dan bisa segera dipakai:** RentalOps Worker `https://rentalops-site.usahalab-id.workers.dev` sudah live. HTML Worker berhasil dibaca read-only pada 2026-11-12. Judul: `RentalOps — Booking Rental Lebih Rapi`; H1: `Jangan sampai unit yang sama dijanjikan ke dua penyewa.`; kalkulator sudah tertanam. Input kalkulator: permintaan booking per bulan, persentase chat terlambat/tidak tertangani, peluang konversi jika terlayani, laba kontribusi per booking, dan biaya sistem. Rumus potensi kebocoran = permintaan × tingkat chat terlambat × peluang konversi × laba kontribusi per booking. Halaman secara benar menyatakan software tidak otomatis memulihkan semua lead yang hilang.
- **Status CTA:** kedua CTA produk saat ini mengarah ke halaman publik biasa `https://cshub.id/id/solutions/rental-sewa`, belum merupakan tautan referral. Tidak ada perubahan/deployment website pada audit terakhir. Jangan mengklaim atau mengirim CTA referral sebelum ada link resmi affiliate yang valid.
- **Ekonomi referral CSHub:** halaman affiliate mencontohkan 20% recurring dari paket Starter Rp300.000/bulan = Rp60.000 per pelanggan aktif per bulan. Maka 5 pelanggan Starter aktif ≈ Rp300.000/bulan; 17 ≈ Rp1.020.000/bulan; 50 ≈ Rp3.000.000/bulan pada rate 20% yang sama. Ini komisi berulang, bukan uang sekali bayar; belum tentu cukup cepat untuk kebutuhan tunai segera. Jangan menjanjikan hasil.
- **Syarat payout yang masih kontradiktif:** landing affiliate menyatakan tanpa minimum dan 1×24 jam kerja, sedangkan panduan affiliate yang lebih rinci menunjukkan minimum Rp20.000 dan waktu 1–3 hari kerja. Anggap panduan operasional Rp20.000 dan 1–3 hari kerja sebagai asumsi konservatif, tetapi perlu konfirmasi tertulis.
- **Eligibility usia:** pengguna berusia 17 tahun; belum ada konfirmasi resmi apakah boleh bergabung dengan persetujuan wali, dan apakah rekening wali dapat dipakai. Jangan membuat akun dengan usia palsu atau menjanjikan pencairan sebelum jelas.
- **Email CSHub:** email resmi TERKIRIM ke operator CSHub `manufacturdigitalhub@gmail.com` pada 2026-11-12. Gmail message ID `1a1cb91653fc1426`, thread `1a1cb8019cf21fbb`. Pertanyaan meliputi usia/wali, nama rekening, tier/rate, durasi recurring, attribution/refund, payout, biaya dan target. Pada pemeriksaan terakhir belum ada balasan. Email awal ke `care@cloudchat.id` salah alamat; jangan anggap itu kontak CSHub.
- **Vendor lain:** MyRental dan Rentra adalah pembanding yang pernah diteliti, bukan fokus saat ini. Draft pertanyaan partnership Rentra Gmail ID `r266373474619121964` belum dikirim. Jangan menghabiskan waktu memperluas daftar vendor sebelum uji akuisisi pertama atau bukti bahwa CSHub tidak bisa dipakai.
- **Langkah cepat yang aktif:** (1) pakai kalkulator RentalOps yang sudah live sebagai konten/alat gratis untuk menarik rental mobil/motor; (2) sebarkan satu konten edukatif dengan tautan kalkulator ke komunitas rental yang relevan, bukan pindah segmen; (3) ukur respons dan cari satu operator dengan armada/chat cukup aktif untuk uji CSHub; (4) setelah link affiliate dan usia/payout terkonfirmasi, sambungkan CTA. Prioritaskan kanal publik/inbound agar tidak bergantung pada cold-DM; siapkan pesan kontak singkat hanya bila diperlukan.
- **Hasil yang benar-benar diselesaikan dalam sesi koreksi:** fokus pasar dikembalikan ke rental mobil/motor; live website dan kalkulator diperiksa; kesalahan pivot event-rental dicatat sebagai bukan keputusan; email CSHub benar-benar terkirim; tidak ada email Rentra dikirim dan tidak ada deployment website dilakukan.


### P-SIGNAGE — SignageQuote
- **Status:** PROYEK TERPISAH; bukan proyek rental.
- **Tujuan:** kalkulator/estimator penawaran untuk bengkel signage/reklame.
- **Website yang tercatat:** https://signagequote.usahalab-id.workers.dev
- **Bukti Cloudflare diperiksa 2026-11-12:** Worker terpisah bernama `signagequote` ada; `created_on` 2026-11-07T01:39:04Z, `modified_on` 2026-11-11T16:00:30Z, `last_deployed_from=api`.
- **Verifikasi live 2026-11-12:** `https://signagequote.usahalab-id.workers.dev` berhasil dibuka; judul/isi yang tampil adalah **SignageQuote — Estimator Bengkel Signage**. URL ini bukan alamat RentalOps.
- **Artefak terkait yang pernah ada:** SignageQuote Offline Kit.
- **Batas keras:** jangan gunakan kode, URL, keputusan, atau status SignageQuote untuk menyimpulkan keadaan RentalOps.

### P-SIAPKIRIM — SiapKirim
- **Status:** PROYEK TERPISAH; bukan proyek rental dan bukan SignageQuote.
- **Tujuan:** layanan digital penyusunan dokumen lamaran kerja/CV untuk pelamar pemula.
- **Dokumen strategi yang tercatat:** SiapKirim_Strategi_Nasional_v0.2.md (Library, 2026-11-09).
- **Batas keras:** kemunculan kata “bisnis” atau hasil pencarian Library tidak menjadikan SiapKirim proyek aktif. Hanya lanjut jika pengguna menyebutnya secara eksplisit atau konteks percakapan membuktikannya.

### P-BACKTEST — Riset trading/backtest
- **Status:** PROYEK TERPISAH.
- **Repo utama yang tercatat:** https://github.com/christianomomongan82-lang/scanner
- **Fokus:** riset kuantitatif dan validasi strategi; preregistrasi dan aturan eksperimen harus dipertahankan.
- **Batas keras:** jangan mencampur data, metrik, keputusan, atau cabang repo backtest dengan bisnis rental.

### P-ASPRUSSIA — Rancangan negara fiksi AS-Prusia
- **Status:** PROYEK TERPISAH.
- **Sumber utama:** README.md pada repo ini.
- **Batas keras:** README.md adalah arsip proyek rancangan negara fiksi, bukan indeks bisnis dan bukan status RentalOps.

## 1A. Pemetaan repositori dan deployment (diperiksa 2026-11-12)

- Daftar repositori yang terlihat melalui koneksi GitHub akun pemilik saat pemeriksaan: `dinasti-medieval`, `dinasti-medieval-build`, `dinasti-medieval-work`, `tetod`, `scanner`, dan `terserah-mau-taruh-apa`.
- Tidak ada repo RentalOps atau SignageQuote pada daftar tersebut. Ini **tidak** membuktikan source code tidak ada di tempat lain; artinya repo sumber belum berhasil dipetakan.
- Daftar Worker Cloudflare menunjukkan dua script berbeda: `rentalops-site` dan `signagequote`. Karena keduanya dideploy melalui API menurut metadata daftar Worker, jangan otomatis menganggap keduanya tersambung ke repo GitHub tertentu.
- Sebelum perubahan kode berikutnya, sumber deploy yang benar harus ditetapkan: repo sumber jika ada, atau Worker Cloudflare yang sedang berjalan jika memang dikelola langsung. Catat hubungan source → build/deploy → Worker untuk masing-masing proyek.


## 2. Urutan sumber kebenaran

Jika sumber bertentangan, gunakan urutan ini:
1. Instruksi eksplisit terbaru pengguna dalam percakapan yang sedang berlangsung.
2. Keputusan/checkpoint terbaru yang tercatat pada sumber utama proyek yang tepat.
3. Bukti langsung dari keadaan aktual: repo, branch, commit, file, deployment, halaman, dashboard, atau hasil tool.
4. Catatan percakapan lama yang secara eksplisit merujuk proyek yang sama.
5. Memory/ringkasan percakapan.
6. Hasil pencarian Library atau pencarian semantik.

**Library search, ingatan, dan kemiripan kata tidak pernah cukup untuk mengubah proyek aktif.** Hasilnya hanya petunjuk untuk menemukan sumber; bukan bukti bahwa file tersebut relevan dengan percakapan sekarang.

## 3. Protokol sebelum melanjutkan pekerjaan

Sebelum tindakan atau rekomendasi yang mengubah arah proyek:
1. Identifikasi ID proyek dari pesan pengguna saat ini dan konteks percakapan langsung.
2. Cocokkan dengan satu entri proyek di dokumen ini.
3. Buka sumber utama proyek yang tepat; verifikasi nama, tujuan, keputusan terkunci, repo/branch, URL, dan artefak terakhir.
4. Pisahkan fakta menjadi **terverifikasi saat ini**, **catatan terakhir tetapi belum dicek**, **asumsi**, dan **belum diketahui**.
5. Periksa apakah langkah yang hendak dilakukan bertentangan dengan keputusan terkunci atau berasal dari proyek lain.
6. Jika ada konflik nyata, hentikan tindakan yang berisiko dan jelaskan konflik spesifiknya. Jangan menebak pengganti.

Untuk permintaan “lanjutkan”, “bisnis tadi”, “proyek itu”, atau rujukan serupa, konteks percakapan langsung menjadi penentu. Jangan memilih proyek berdasarkan dokumen yang paling mudah ditemukan.

## 4. Format checkpoint wajib

Setelah keputusan penting, perubahan teknis, eksperimen, atau milestone, perbarui entri proyek yang tepat dengan:
- **Tanggal:**
- **Status:** aktif / menunggu / selesai / dibatalkan / terblokir.
- **Tujuan saat ini:**
- **Keputusan terkunci:**
- **Yang benar-benar berubah/selesai:**
- **Bukti verifikasi:** URL, repo, branch, commit, file, hasil uji, atau screenshot yang relevan.
- **Yang belum diketahui/belum diverifikasi:**
- **Langkah berikutnya yang disepakati:**
- **Larangan/perangkap:** hal yang tidak boleh diulang atau dicampur.

Jangan memperbarui status menjadi selesai hanya karena tindakan dicoba. Catat hasil aktual. Jika belum ada bukti, tulis BELUM DIVERIFIKASI.

## 5. Protokol saat konteks terputus atau chat baru

1. Baca `CHATGPT_WORKING_RULES.md`.
2. Baca dokumen ini dan identifikasi ID proyek dari konteks terbaru.
3. Buka sumber utama proyek tersebut; jangan memulai dengan pencarian Library global.
4. Ambil checkpoint terakhir dan cek bukti aktual yang dapat diperiksa.
5. Nyatakan hanya status yang benar-benar didukung; lanjutkan dari titik itu.
6. Setelah bekerja, tulis checkpoint baru sebelum mengakhiri sesi.

Jika percakapan sumber tidak tersedia atau link/file tidak dapat dibuka, jangan berpura-pura telah membaca. Gunakan checkpoint yang ada dengan label ketidakpastian, lalu minta hanya satu detail yang benar-benar tidak dapat dipulihkan.

## 6. Pemeriksaan kontradiksi sebelum mengirim jawaban

Sebelum jawaban akhir tentang proyek, periksa:
- Apakah nama/ID proyek konsisten dari awal sampai akhir?
- Apakah produk, URL, repo, branch, dan file memang milik proyek itu?
- Apakah rekomendasi bertentangan dengan keputusan yang sudah dikunci?
- Apakah status yang disebut benar-benar diverifikasi, atau hanya catatan lama?
- Apakah ada klaim keberhasilan yang tidak didukung hasil nyata?
- Apakah proyek lain masuk karena hasil pencarian, ingatan, atau kemiripan istilah?

Jika salah satu gagal, koreksi dulu sebelum menjawab.

## 7. Batas kemampuan yang harus dinyatakan jujur

Ringkasan/memori lintas percakapan dapat tidak lengkap atau keliru, dan tidak menjamin seluruh chat lama otomatis terbaca. Dokumen eksternal ini adalah mekanisme pemulihan, bukan jaminan teknis bahwa model tidak akan pernah salah. Karena itu, sumber utama harus diperiksa ulang sebelum keputusan atau perubahan penting; jangan mengklaim kontinuitas yang belum diverifikasi.

---


## 0AF. CHATGPT ↔ AGENT ANDROID BRIDGE — TWO-WAY ROUTE IDENTIFIED, NOT YET CONNECTED — 2026-11-14

- **Status:** ARCHITECTURE VERIFIED; END-TO-END NOT YET TESTED. Do not claim ChatGPT and Agent are connected until one full round trip succeeds.
- **Goal:** Let ChatGPT dispatch short, read-only research/browsing tasks to the Android Agent and read the result back in ChatGPT, without PC or another app install.
- **Existing resource inspected:** Cloudflare Worker `kreavo-bridge` + KV namespace `kreavo-bridge`. The Worker already provides GET `/client/command` and POST `/client/result` routes for a shared command/result queue. Existing namespace was empty; a temporary write/read/delete probe succeeded and the namespace was verified empty afterward. No Worker code/settings or other existing Worker was changed.
- **Proposed channel:** ChatGPT writes command JSON to existing KV key `command`; sends a wake-up email with subject marker `CHATGPT-AGENT`; Agent uses a notification-triggered routine listening for Gmail; Agent reads the Worker command from its own browser on the allowed Kreavo origin and posts a result to the Worker; ChatGPT reads the KV `result` key. The Worker stores result for 300 seconds.
- **Important collision:** existing Worker uses fixed keys `command` and `result`, shared with the Kreavo bookmarklet. Do not use both bridge sessions simultaneously. Check active Kreavo snapshot/session before enqueueing; do not modify or repurpose business assets beyond this limited shared transport.
- **One-time setup not yet done:** create a narrow Agent learned web app with read command + post result tools, user approves save, user turns Trust ON only for this app, allow Gmail in Agent's read-notification apps, and create notification routine for Gmail package `com.google.android.gm` + `CHATGPT-AGENT`. Agent background task scope is read-only only.
- **Unavoidable user action:** Agent is local to Android and has no remote tool in this ChatGPT session; initial setup/notification permission/trust cannot be changed remotely. Do not ask user to install more apps, configure another relay, or do research manually.
- **Result caveat:** five-minute KV result TTL means retrieval must be prompt; durable long-running result delivery is not yet solved. Use only short tasks at first.
- **Canonical technical record:** `research/chatgpt_agent_bridge_2026-11-14.md`.
- **Scope boundary:** this technical bridge is not a business pivot. RapiUsaha remains the primary B2C digital-products business for UMKM. No changes to RapiUsaha, Projects.co.id, RentalOps, SignageQuote, SiapKirim, or P-Backtest.


## 0BA. NTFY PLAY-STORE BUILD CAN STILL HIT WEBSOCKET; USE FCM MODE — 2026-11-18

**Status: DOCUMENTED CORRECTION / user has not yet changed the phone setting or verified delivery.** The user confirmed ntfy was installed from Google Play, but the app reports “WebSocket not supported” and reconnects.

- Correct the earlier oversimplification: Google Play flavor supports Firebase Cloud Messaging (FCM) for the main host `ntfy.sh`, but this does not mean the app never attempts WebSocket. The official phone docs describe “Instant delivery” as a persistent foreground subscription service; disabling the Subscription Service falls back to FCM, which may be delayed (sometimes minutes or hours).
- Minimal user-side fix: on the ntfy Android app, long-press its persistent “instant delivery” foreground notification, open its settings, and turn off **Subscription Service**. Official docs: https://docs.ntfy.sh/subscribe/phone/ (Instant delivery section). Then send a test notification and confirm it arrives. Do not ask the user to reinstall or use a paid app.
- This is an intentional tradeoff: FCM avoids the persistent WebSocket connection but delivery is best-effort and may be delayed. If FCM does not work, investigate network/Google Play Services before changing the bridge architecture.
- The topic subscription and Agent notification routine are still not confirmed operational. Do not claim end-to-end bridge success until an Agent claim and valid result are observed.


## 0BD. MACRODROID FREE WEBHOOK AVAILABILITY CONTRADICTED BY USER'S INSTALLED APP — 2026-11-18

- **Status:** BLOCKED / NOT VERIFIED FREE ON THIS DEVICE. User installed MacroDroid and reports that the Webhook option visible in the app is marked Pro. The assistant's previous instruction to select it was premature and contradicted the user's no-payment constraint. User must not purchase Pro or proceed with a paid feature.
- Prior 0BB claim that the basic Webhook (URL) trigger is free is now disputed by direct evidence from the user's installed app. Public listing/wiki/research did not establish what exact menu item the user sees. Distinguish basic **Webhook (URL)** from **Webhook (with response)**, but do not assume the user's screen is the former without seeing it.
- **Correction:** stop MacroDroid setup. Ask for one screenshot of the trigger picker showing the exact Webhook entry and Pro marker, then verify whether a separate basic Webhook (URL) option exists. If all available webhook triggers require Pro, abandon MacroDroid immediately and investigate a genuinely free path without asking the user to pay or troubleshoot ntfy again.
- Architecture decision A remains USER_CONFIRMED in principle (ChatGPT is the brain; Agent is executor), but MacroDroid Free as the wake adapter is now OPEN/BLOCKED pending on-device verification. No MacroDroid Pro purchase authorized. No webhook URL, Worker secret, macro, or Agent task was created by this attempt.


## 0BE. MACRODROID BASIC WEBHOOK SCREEN CONFIRMED; GLOBAL TICKET VARIABLE NEXT — 2026-11-18

**Status: USER_CONFIRMED screen; partial local setup only.**

- User installed MacroDroid and selected the basic **Webhook (URL)** trigger (not the Pro-only “Webhook (with response)”).
- User transcribed its settings: generated URL; empty IP whitelist; body-to-string and query-parameter-to-dictionary outputs both “Do not save output”; Variable Whitelist checkbox. Enabling the checkbox showed no further controls.
- Explanation verified against MacroDroid documentation: webhook query parameters update pre-existing variables; the webhook does not create variables. The whitelist controls which existing variables are eligible. Next: save trigger, create global String variable `bridge_ticket` via MacroDroid Variables manager, then return to trigger and whitelist only that variable. Do not assume exact UI until user reports it.
- The webhook URL was pasted into the private chat and is treated as exposed bearer material. Before production use, user must generate a new device ID (invalidates old URLs); only the new URL may be stored as a Cloudflare Worker secret, never in GitHub. Agent token stays on-device and must never be shared.
- No Send Intent action configured; no Worker wake route/secret; no task dispatched. Existing pending read-only smoke task remains untouched. User-approved Option A remains unchanged.
- Research details and source links: `research/chatgpt_agent_action_execution_2026-11-17.md`, section 0BE.
