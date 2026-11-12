# 04 — CODEX + ROBLOX STUDIO OPERATING GUIDE (WINDOWS)
Follow milestones sequentially. Codex generates and reviews code; Roblox Studio is the only authoritative runtime/test platform. **A GitHub repo is not automatically linked to Studio**.

## 1. Install and verify
1. Install **Roblox Studio** from https://create.roblox.com/docs/studio and sign in to the intended owning account/group.
2. Install **Git for Windows** from https://git-scm.com/download/win ; restart terminal; run `git --version`.
3. Install **VS Code** from https://code.visualstudio.com/ ; install a maintained Luau extension.
4. Install **Rokit** toolchain manager and **Rojo** from their official documentation. Official Roblox Rojo integration guide: https://create.roblox.com/docs/projects/rojo . Verify `rojo --version`. Pin tested versions in `rokit.toml`; avoid arbitrary stale commands from tutorials.
5. In Studio, install the **Rojo plugin** (Studio Creator Store or `rojo plugin install` per installed Rojo documentation). Never install anonymous "auto script" plugins.
6. Connect Codex to **local checkout** using its supported editor/CLI workflow. Codex should edit files in Git; it cannot control a Studio session merely because a GitHub repository URL exists. Optional Roblox Studio MCP tools must be connected, authorized and tested separately.
7. Clone with terminal:
```powershell
git clone https://github.com/rizqunbw/rblxgm.git
cd rblxgm
code .
```
8. Create a private development experience in Studio. Configure publishing, permissions and developer/QA access. Never store credentials, Roblox cookies, secrets, .env, local tokens or commercial assets in a public repository.

## 2. Choose synchronization architecture
**Preferred:** PARTIAL Rojo sync for source code; keep 3D map/models, animation assets and UI layout in Studio, with manifest/IDs and backups. Roblox docs recommend this for most teams using Git.
- Codex authors `.luau` files on disk, tests, config and docs.
- Rojo serves those files into Studio. Keep manually built Workspace assets out of paths Rojo exclusively manages.
- Studio owns the place file and published assets. Export periodic place backup to a private secure location.
- Studio's native Script Sync is an alternative but do **not** enable two sync systems on the same scripts.

## 3. First sync
1. Ask Codex to create `default.project.json`, `src/shared`, `src/server`, `src/client`, tool configs with valid Rojo mappings, and one HelloWorld server/client demonstration. Keep generated code small and compilable.
2. In terminal, run `rojo serve`.
3. In Studio, open the intended test place; open Rojo plugin and connect to local server.
4. Check Explorer paths: ServerScriptService, ReplicatedStorage, StarterPlayerScripts.
5. Press Play: confirm server Output and client Output each show ONE expected boot message.
6. Edit a string in VS Code, save, and verify it updates in Studio; stop Play when updating live models as needed.
7. Commit only after verifying Rojo did not delete artists' instances.

## 4. Codex execution contract
Copy this into every Codex milestone prompt:
```text
Read README.md and all docs/*.md first. Implement ONLY the named milestone.
Language: typed Luau; target Roblox Studio. Git files are source of truth for scripts.
Use server-authoritative state. Never let client decide currency, rarity,
ownership, receipt grants, timers or upgrades. No paid randomness. Keep APIs
modular and config-driven. Provide code, tests, exact Studio manual actions,
Explorer expected hierarchy, test checklist, rollback strategy, and files changed.
Do not invent Roblox asset IDs, product IDs, API signatures or claim tests
were executed without actual evidence. Avoid modifying unrelated files.
```

## 5. Milestone prompts (copy ONE at a time)
### M0 — Repository scaffold
```text
Milestone M0: create a valid Rojo project scaffold mapped to ReplicatedStorage/
Shared, ServerScriptService, StarterPlayerScripts. Implement Bootstrap.server.luau
and Bootstrap.client.luau with typed Luau and a clean boot log. Add formatting,
linting and minimal CI instructions. Do not create gameplay yet. Explain exactly
how to verify locally in Roblox Studio.
```
**Done when:** Rojo connects, both boot logs visible, no errors, no accidentally deleted assets.

### M1 — Plot ownership + camera
```text
Milestone M1: implement 8-plot server reservation using plot markers in Workspace,
server-only ownership and release on leave. Add safe spawn relocation, basic
client UI showing owner, and a camera tour. Do not add data persistence yet.
Create assertions for double assignment and join/leave.
```
**Done when:** 8 studio clients each occupy unique plot, ninth waits gracefully or enters a spectator queue; no plot hijack.

### M2 — Profiles + persistence
```text
Milestone M2: implement typed versioned profile model, session locking,
robust retries/backoff, autosave and shutdown saving, safe failure mode,
separate test datastore configuration, player reconnect tests. No purchases.
```
**Done when:** upgrades/currency recover after normal reconnect and tested interruption; two sessions cannot simultaneously mutate one profile.

### M3 — Machines + coins
```text
Milestone M3: add placement grid, server-owned starter machine, production
calculation, collection, upgrades, costs and caps. Use config-driven values.
Add malicious RemoteEvent input tests and client feedback.
```
**Done when:** no client can mint coins or place on another plot; balanced first 3 minutes.

### M4 — Creatures + deterministic hatch
```text
Milestone M4: implement 6 original creature species definitions, free tutorial
egg with guaranteed Blobbit, owned GUIDs, capacity, assignment, collection
index and placeholder meshes. Hatch is server-authoritative and idempotent.
```
**Done when:** hatch persists, no duplication under double-click/reconnect, all 6 collectible through gameplay.

### M5 — Mutation + quests
```text
Milestone M5: implement one guaranteed starter recipe and simple earned-resource
recipes, tutorial quest chain, visual reveal events and reduced-effects setting.
Use no paid random outcomes.
```
**Done when:** tutorial fully playable on mobile in 3-5 minutes; server verifies recipe input/output.

### M6 — UX + art integration
```text
Milestone M6: integrate custom original 3D models with placeholders fallback,
responsive mobile HUD, accessibility options, animation/sound hooks, studio
asset registry and performance counters. No third-party model scripts.
```
**Done when:** readable on phone, no critical overlaps with CoreGui, FPS test performed.

### M7 — Monetization
```text
Milestone M7: implement one fixed cosmetic Pass and one true repeatable cosmetic
Developer Product using configurable actual Roblox IDs. Build verified
server entitlements, one ProcessReceipt processor, durable idempotent grants,
receipt replay tests, transparent shop UI and no random Robux rewards.
```
**Done when:** reconnect and receipt-replay tests pass; receipts never awarded twice, and purchases are not silently lost.

### M8 — Content QA and private alpha
```text
Milestone M8: implement analytics event abstraction, tutorial funnel,
error logging (privacy-safe), automated smoke tests, test matrices,
performance LOD settings and feature flags. Produce release gate report.
```
**Done when:** multi-client testing, first time user test, performance and monetization gates signed off.

### M9 — Post-alpha social
```text
Milestone M9 (do not start before alpha gate): design and add opt-in raid
contracts with replica/token rewards, simple defense mini-game, anti-abuse
limits, opt-out and moderation safeguards. No permanent victim inventory loss.
```

## 6. Exact manual tasks Codex cannot assume are automated
- Create and publish test universe in Studio; set correct ownership/permissions.
- Import/edit original Blender meshes, rigs, animations; verify content moderation.
- Configure Game Pass and Developer Product in Creator Dashboard, paste actual IDs into config.
- Playtest with multi-client Studio and real mobile device; inspect Output, Stats, MicroProfiler.
- Publish through Roblox Studio/authorized workflow; test in a separate experience before public release.

## 7. Debug decision tree
- **Rojo disconnected**: confirm `rojo serve`, plugin enabled, correct localhost connection and firewall permissions.
- **Script path wrong**: validate `default.project.json` and extension names (`.server.luau`, `.client.luau`, `.luau`).
- **Code overwritten**: stop duplicate script editors/sync methods and restore Git commit.
- **Place objects missing**: inspect Rojo's ownership and `$ignoreUnknownInstances` rules; recover Studio backup.
- **Datastore unavailable in test**: verify test universe settings; never enable prod datastore in untrusted local tests.
- **Purchase never granted**: check product ID, ProcessReceipt singular assignment, transaction journal and retry behavior.
- **Studio MCP unavailable**: use Rojo + manual Studio tasks; never claim direct Studio automation exists.

## 8. Git discipline
Create feature branches (`feat/plot-service`), commit per milestone, run local tests, PR with evidence, merge only after QA. Use tags for releases and retain place-file backups. Rollback includes code commit AND compatible persisted schema; never revert schema blindly.
