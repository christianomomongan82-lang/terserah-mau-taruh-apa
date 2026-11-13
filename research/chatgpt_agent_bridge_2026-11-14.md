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
