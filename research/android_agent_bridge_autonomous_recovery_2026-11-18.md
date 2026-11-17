# Android Agent Bridge — autonomous recovery audit (2026-11-18)

## User instruction

The user said they are fed up with the repeated manual MacroDroid setup and directed ChatGPT to handle the project itself and use any available method. Stop sending step-by-step MacroDroid UI instructions or asking for screenshots. Minimize any unavoidable user-side action to one clear step only after exhausting remote options.

This instruction authorizes broad technical investigation and a practical route change if necessary. It does not explicitly name a new architecture as a settled preference. Preserve the last explicit architecture decision as historical context; do not claim the user specifically chose Agent-owned polling.

## Project state verified in this audit

- Canonical project: ChatGPT ↔ Android Agent Bridge.
- Last explicitly locked architecture before this instruction: ChatGPT owns dispatch, result retrieval, failure diagnosis and retry decisions; Android Agent is the executor; MacroDroid Free was intended only as a wake adapter.
- MacroDroid setup was not completed. The user hit an invalid variable value and then stated that they cannot send more photos and do not want further manual setup instructions. Treat the MacroDroid UI path as paused/abandoned unless a remote setup route becomes available.
- Remote Desktop Commander device discovery returned **no connected devices**. This ChatGPT session therefore has no direct control channel to the Android phone.
- Cloudflare account has Worker `kreavo-bridge` and KV namespace `kreavo-bridge` (namespace ID remains in Cloudflare, not copied here). Worker bindings include `BRIDGE_KV` and `AI`; the Worker secrets list was empty at audit time.
- KV currently contains a pending broker command `broker-readonly-smoke-20261118-01` and a separate pending bootstrap command `bootstrap-poller-20261118-01`. Do not duplicate, overwrite, claim, or replay either task until their state is rechecked. Do not include bearer ticket values in GitHub.
- Worker script has the broker endpoints `/agent/command` and `/agent/result`, but no configured external phone-wake route/secret was found. The broker still needs a phone-side process to poll/claim or a push adapter to wake the Agent.
- Worker currently uses a hard-coded room capability in its client route and allows only Kreavo web origins. Do not expose the value or expand access casually; review this security boundary before any production change.

## Upstream Agent source findings

Repository: https://github.com/Past-da-king/agent  
Commit inspected: `8594dfadc3f728d0123990179dfc4361d55dd69c`

1. The Agent supports external routine execution by an Android broadcast:
   - package `com.past9.phoneaos`
   - action `com.past9.phoneaos.RUN_ROUTINE`
   - extras `token`, `routine` (name or id), optional `note`.
   The receiver is exported in the manifest. This requires a sender running on the phone or a supported linked computer; a Cloudflare Worker cannot emit a local Android broadcast by itself.
2. The Agent HTTP automation server listens only on `127.0.0.1:47321` and only while the Agent app is running. It cannot be called directly from Cloudflare or another internet host. This is an explicit upstream limitation, not a configuration guess.
3. Agent routines support:
   - `interval` triggers, minimum 15 minutes;
   - `email` triggers, internally checked on a 15-minute cadence;
   - `notification` triggers on an allowed app's incoming notification.
   These are possible phone-side wake/poll mechanisms, but none is confirmed configured for the current bridge.
4. A fixed 15-minute Agent routine that only checks the Cloudflare broker could be a low-cost fallback wake adapter while ChatGPT still owns the task payload, result validation, diagnosis and retry decision. It would be delayed/best-effort and must not be represented as real-time. This is a candidate, not yet configured or tested.
5. The upstream README and `docs/automation-macrodroid.md` confirm that the app's local HTTP server is loopback-only; the intent route is the supported way for another on-device app to start a routine.

## Current conclusion

There is no available remote Android device connection in this ChatGPT session. The direct cloud-to-localhost route is technically impossible under the Agent's documented network model. The remaining viable routes require one of:
- an already-configured phone-side wake source (MacroDroid webhook, notification trigger, or scheduled Agent routine);
- a supported linked computer/device bridge;
- or a one-time on-device enablement that cannot be performed remotely from the current session.

Do not create a fake success, do not use TinyFish or another cloud browser as proof of Agent-on-phone execution, and do not send duplicate smoke tasks.

## Next work owned by ChatGPT

1. Continue remote-only investigation first: inspect the Worker/broker implementation and existing Agent routines/tasks if an authorized device route becomes available.
2. Prefer the smallest no-cost phone-side adapter that preserves ChatGPT as the orchestrator. A 15-minute fixed poller is a fallback if no push route can be configured; state its latency and model-ownership tradeoff plainly.
3. Keep the Agent token on the phone. Never ask the user to paste it into ChatGPT or GitHub.
4. Never use paid MacroDroid Pro, API billing, subscriptions or other paid services without explicit approval.
5. Before any production task, inspect pending/active/result KV state and confirm the exact task ID. A webhook HTTP 2xx is not proof; proof requires Agent claim plus a valid matching result.
6. After meaningful changes, update this checkpoint and `PROJECT_CONTROL.md`, then fetch them again to verify.

## Sources

- Agent README: https://github.com/Past-da-king/agent/blob/8594dfadc3f728d0123990179dfc4361d55dd69c/README.md
- Agent local automation API: https://github.com/Past-da-king/agent/blob/8594dfadc3f728d0123990179dfc4361d55dd69c/docs/automation-macrodroid.md
- Agent intent receiver: https://github.com/Past-da-king/agent/blob/8594dfadc3f728d0123990179dfc4361d55dd69c/app/src/main/java/com/past9/phoneaos/automation/IntentTriggers.kt
- Agent routine trigger validation: https://github.com/Past-da-king/agent/blob/8594dfadc3f728d0123990179dfc4361d55dd69c/app/src/main/java/com/past9/phoneaos/tools/AgentTools.kt
- Agent notification listener: https://github.com/Past-da-king/agent/blob/8594dfadc3f728d0123990179dfc4361d55dd69c/app/src/main/java/com/past9/phoneaos/system/NotificationReader.kt
