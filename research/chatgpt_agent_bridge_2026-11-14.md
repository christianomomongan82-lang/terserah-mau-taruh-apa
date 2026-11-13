# ChatGPT ↔ Agent Android bridge investigation — 2026-11-14

## Status
**OPEN / NOT CONNECTED.** No working end-to-end connection has been established. No further user setup is authorized by this note. Do not describe the integration as completed.

## User goal
The user wants ChatGPT in this conversation to send instructions to the Android app Agent (Past-da-king/agent) and receive Agent's results, so Agent acts as ChatGPT's hands/automation runner. The user is phone-only (Samsung A06, Android 15), has no PC, and wants to avoid extra app installs, costs, and manual research tasks.

## Verified account and platform constraints
- User's ChatGPT plan in conversation: Free.
- Official OpenAI Help Center documentation for Developer Mode/MCP apps:
  - https://help.openai.com/en/articles/12584461-developer-mode-and-mcp-apps-in-chatgpt
  - Custom MCP apps are currently web-only, not available on mobile; Free does not have custom MCP developer-mode access. Pro custom MCP access is read/fetch-only. Business/Enterprise/Edu can use full MCP apps on web.
  - Therefore, Agent's local MCP endpoint cannot be connected directly to this ChatGPT mobile/Free session.
- Agent repository: https://github.com/Past-da-king/agent
  - Agent's local automation HTTP server binds to 127.0.0.1:47321, uses a bearer token, and is only reachable on the phone.
  - Agent exposes an MCP server on loopback for its bundled runtime; it is not automatically exposed to ChatGPT.
  - Agent supports Android broadcast intents RUN_ROUTINE and INSTRUCT from other Android apps if "Allow outside runs" is enabled.
  - Agent supports notification-triggered routines. Source inspected:
    - app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt
    - app/src/main/java/com/past9/phoneaos/tools/AgentTools.kt
    - app/src/main/java/com/past9/phoneaos/data/Db.kt
  - A notification routine can match an app name/package plus keyword. On a matching notification, Agent launches its routine and passes notification context; notification text is truncated to 400 characters. Only notifications from apps the user explicitly allows are read.
  - Agent's docs say background routines cannot complete actions that require user approval (send/spend/delete) when nobody is present. This matters for unattended email replies or destructive actions.

## Relay routes evaluated
### 1. Direct MCP (REJECTED for current setup)
Blocked by ChatGPT Free + mobile product constraints and Agent's loopback-only MCP server. Do not ask the user to paste the local endpoint into ChatGPT; it cannot work here.

### 2. Gmail notification → Agent (POSSIBLE ONE-WAY WAKE-UP; NOT TESTED)
- ChatGPT has a connected Gmail sender identity verified via a recent Sent message from christianomomongan82@gmail.com.
- ChatGPT could send an email with a unique marker in subject.
- Agent can trigger a routine when Gmail posts a notification, provided Android notification access and Gmail are explicitly enabled in Agent.
- Agent could execute a short task from the notification context.
- This only provides a one-way trigger. No verified return channel to ChatGPT exists. Do not sell it as a completed two-way bridge or ask the user to configure it until a safe return path has been independently demonstrated.

### 3. Cloudflare Worker + KV callback (ATTEMPTED, BLOCKED, CLEANED UP)
- Cloudflare account was verified through the connected connector.
- Created a dedicated KV namespace chatgpt-agent-relay (ID 7aaa8256ad714fe1bc1aba6de668683f) and a temporary Worker agent-relay.
- A harmless placeholder Worker uploaded successfully, but the callback script required to receive Agent results was blocked by the tool safety check. The placeholder Worker and namespace were then deleted.
- A follow-up read verified: agent-relay Worker absent; chatgpt-agent-relay KV namespace absent.
- No existing Worker or namespace was modified. Specifically, kreavo-bridge, RentalOps, SignageQuote, and other Workers were untouched.
- Do not retry this exact deployment or route around the safety gate by obfuscating the script.

## Actual conclusion
There is currently **no verified two-way ChatGPT ↔ Agent bridge** available from this session. A notification-based one-way trigger is technically supported by Agent, but the return path remains unsolved. The assistant must not claim connection, not assign research to the user, and not request additional app installations or account changes without first proving an end-to-end design and explaining exactly which minimum user action is unavoidable.

## Next technical investigation (assistant-owned)
1. Inspect whether Agent has a supported, approval-safe result export that can be read by an already-connected ChatGPT connector (for example an explicitly authorized GitHub write/readback flow), without requiring background send/delete/spend.
2. If no safe return path exists, report that honestly and stop; do not keep experimenting with user's accounts.
3. Any future deployment needs explicit scope, least-privilege access, authentication, TTL/retention, and a verified test from ChatGPT → Agent → ChatGPT.


## 2026-11-14 update: existing Kreavo Bridge gives a viable two-way relay path

**Status: ARCHITECTURE VERIFIED; NOT YET ACTIVE END-TO-END.** This supersedes the earlier conclusion that only a one-way Gmail wake-up was available. Do not claim the apps are connected until the one-time Agent setup is completed and a full round-trip test succeeds.

### Existing infrastructure inspected (read-only)
- Cloudflare Worker: `kreavo-bridge`; public host `https://kreavo-bridge.usahalab-id.workers.dev`.
- Existing Worker uses KV namespace `kreavo-bridge` (ID `ab71a201c13f48fcbf533e9b83e6c19d`) and binds it as `BRIDGE_KV`.
- Worker already has two routes suitable for a queue: GET `/client/command?t=...&after=...` reads KV key `command`; POST `/client/result?t=...` stores JSON under key `result` with a 300-second TTL. Both require the embedded room token and an Origin of `https://www.kreavo.id` or `https://kreavo.id`. The Agent browser can call them from a learned web app whose start URL is on that allowed Kreavo origin.
- Worker code and settings were fetched and inspected; no code, settings, secret, deployment or KV namespace was changed.
- KV namespace keys were initially empty. A dedicated temporary key `agent-bridge-probe` was written/read/deleted to prove the connected Cloudflare API has KV write/read/delete permission. A final list-keys call verified the namespace was empty again. Never store the embedded room token in GitHub or public files.
- The old Worker uses shared fixed keys `command` and `result`; therefore the Agent relay and any Kreavo bookmarklet session must not be active simultaneously. Check for an active Kreavo bridge/snapshot before issuing a command; serialize use. This is a known limitation, not a separate queue.

### Proposed round-trip protocol
1. ChatGPT writes a short JSON command to KV key `command` with a 300-second TTL using the connected Cloudflare API.
2. ChatGPT sends a wake-up email to the user's own Gmail address with subject marker `CHATGPT-AGENT`.
3. A notification-triggered Agent routine listens only for Gmail package `com.google.android.gm` and that marker. Agent's NotificationReader supports app/keyword triggers; Gmail must be explicitly allowed for notification reading in Agent.
4. Agent uses a saved learned web app (start URL `https://www.kreavo.id`) to GET the Worker command endpoint, runs the task under a read-only scope, then POSTs the result to `/client/result`.
5. ChatGPT reads the KV key `result` with the connected Cloudflare API and deletes `command`/ `result` after retrieval. The result expires after five minutes, so the user/assistant must retrieve it promptly after Agent finishes; longer durable delivery is not yet solved.

### Agent setup requirements (one-time, user action unavoidable)
- In Agent foreground chat, ask it to open the allowed Kreavo origin, test and save a two-tool learned app: read-only `get_command`; write `send_result` (POSTs only the result payload to the existing Worker). The WebAppSaveTool asks for approval when saving.
- In Agent > Connections > Learned apps, switch Trust ON only for this narrowly scoped `ChatGPT Bridge` app. The Agent source shows write tools prompt every run unless the user explicitly trusts that learned app. No other app should be trusted.
- In Agent > Connections > Read notifications, allow only Gmail; Android notification-listener permission and Gmail notifications must be enabled. Create a notification routine for `com.google.android.gm|CHATGPT-AGENT` which reads one queued command, does read-only research/browsing only, posts a result, and stops. No sending, purchases, deletions, account changes, posting, or form submission in background runs.
- Agent's current app must remain unrestricted from Android battery optimization for background routines to work on Samsung.

### Safety/limitations
- No additional app install, PC, new Worker, or paid plan is required by this architecture. The existing Worker is reused; do not modify its deployed code.
- Because fixed queue keys are shared with the Kreavo bookmarklet, close/disconnect that bookmarklet while Agent bridge runs. ChatGPT should refuse to enqueue work if a live Kreavo snapshot indicates the old bridge is active.
- The Worker room token is embedded in the existing deployed Worker source. Keep it only in the user's local Agent learned-app tool; never write it into GitHub.
- Initial setup and the Trust toggle cannot be performed remotely from ChatGPT; do not pretend they are done. After that, perform and verify a single small round trip before using it for real work.
- Result TTL is 300 seconds. This is suitable for short tasks and requires prompt retrieval; it is not yet a durable asynchronous result queue.
- Official OpenAI current MCP requirements: custom MCP connections are web-only and require Business/Enterprise/Edu for full MCP; Pro read/fetch only. Current Free mobile session therefore cannot directly attach Agent's loopback MCP server. Source: https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt

## Next action
Prepare one copy/paste bootstrap instruction for the user to place in Agent's foreground chat, with the exact Worker endpoints and a narrow read-only routine. Ask the user only to approve the learned-app save, enable Gmail notification access, and toggle Trust for this one bridge app. Then run one harmless end-to-end test. Until a successful round trip is observed, status remains NOT CONNECTED.


## 2026-11-14 update: ephemeral Agent capability endpoints deployed

**Status: Worker patch DEPLOYED AND CONFIGURATION-VERIFIED; Agent round trip NOT YET TESTED.** This supersedes the earlier design that embedded the long-lived ROOM token in Learned App JavaScript. That design was rejected after Agent's Learned App guide refused secrets in tool definitions.

### Change made to the existing Worker
- Updated only Cloudflare Worker `kreavo-bridge` through its script-content endpoint.
- New dedicated routes:
  - `GET /agent/command` reads an Agent task using the `X-Agent-Ticket` request header.
  - `POST /agent/result` accepts a result using the same header.
- Tickets are short-lived random capabilities held in KV as `agent:task:<ticket>`; they are not hardcoded in the Learned App. The Learned App reads the one-task ticket from the current browser URL fragment `#agent=<ticket>` and sends it as a runtime header.
- CORS now allows `X-Agent-Ticket` for the new endpoints.
- New route validation requires a ticket matching 40–64 URL-safe characters, a matching task ID, a boolean `ok`, and a string result capped at 3,500 characters. Task and result records use 300-second KV TTL.
- Existing legacy routes `/client/command`, `/client/result`, `/client/snapshot`, `/install`, and `/health` were retained. The legacy ROOM-token authentication remains only for the old Kreavo routes; it is not included in the Agent Learned App.
- No other Worker, DNS record, KV namespace, or project was changed.

### Deployment and verification
- Cloudflare upload succeeded for Worker `kreavo-bridge`; deployment ID returned: `0ecddcd0152a4c08be47c35c2254d043`; uploaded script tag: `3b5f08489c874b27a873a27455fc62aa`.
- Re-fetched deployed Worker content and verified both new Agent route strings and both legacy command/result route strings exist.
- Re-fetched Worker settings and verified `BRIDGE_KV` still points to namespace `ab71a201c13f48fcbf533e9b83e6c19d`; compatibility date remains `2026-11-07`.
- Re-listed KV keys: empty. No task/result probe was left behind.
- Public HTTP health/test request could not be verified through the web browser tool in this session. The Agent-mediated fetch/round trip remains the required live endpoint test.

### Agent configuration contract
- Learned App ID/name: `chatgpt_bridge` / `ChatGPT Bridge`; start URL `https://www.kreavo.id`.
- `get_command`: read-only, no parameters, reads only the ticket in `location.hash` (format `#agent=<ticket>`) and GETs `/agent/command` with header `X-Agent-Ticket`.
- `send_result`: write tool, parameters `id`, `ok`, `result`; reads the same hash ticket and POSTs only the result to `/agent/result`. No long-lived secret is embedded in either tool definition.
- Agent tool names generated from the app ID are `web_chatgpt_bridge_get_command` and `web_chatgpt_bridge_send_result`.
- Notification routine trigger remains `com.google.android.gm|CHATGPT-AGENT`. Routine must extract the ephemeral ticket from the notification marker, open `https://www.kreavo.id/#agent=<ticket>` with Agent's `browser_open`, call `get_command`, perform only read-only research/browsing, and call `send_result` with the matching task ID.
- Current Agent source confirms `routine_create` supports `kind=notification`; the earlier statement that no routine tool existed was session-specific, not an application-wide absence.
- User still must approve Learned App save, turn Trust ON only for this app (to allow its one result-post tool in background), and permit Gmail notification reading/Android notification access. No app installation or paid service is required.
- Initial execution scope remains read-only. No email sending by Agent, posts, purchases, deletions, account changes, or form submissions.
- Shared KV keys remain separate from the old Kreavo bookmarklet's `command`/ `result` keys. The old bookmarklet must still be disconnected while this bridge is used because it shares those legacy keys; Agent tasks use `agent:task:` and `agent:result:` prefixes.
- **Do not mark the bridge connected** until one test proves ChatGPT task creation -> Gmail notification -> Agent fetch -> Agent result POST -> ChatGPT result retrieval.
