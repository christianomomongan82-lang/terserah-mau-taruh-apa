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
| Agent-native Learned App / notification routine | OPEN / EXISTING PATH TO VERIFY | Recover and verify the already documented integration; do not rebuild from scratch or duplicate tasks. |
| Current live Agent pickup | OPEN / NOT VERIFIED | A historical round-trip is not proof of current pickup. |
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

- Historical read-only Agent↔Cloudflare↔ChatGPT round-trip succeeded once; a separate failure-path test also completed. These prove historical behavior only.
- Current live Agent pickup and Instagram access are not verified in this checkpoint.
- No direct connected Android device-control channel was available in the last recorded check. Recheck current tools before asserting the same blocker.
- Continue through the existing Agent-native Learned App / notification-routine path. Do not introduce another wake app or adapter without the user's explicit approval.

## Conflict resolution

This ledger supersedes conflicting assistant-generated proposals and historical notes. A new assistant-authored document or commit cannot change a USER_CONFIRMED decision. Only an explicit later user instruction can change the user's decision. When the user changes a decision, update this ledger first, then reconcile affected project records.
