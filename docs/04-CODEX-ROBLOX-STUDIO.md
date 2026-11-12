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


## 9. CODEX ↔ ROBLOX STUDIO MCP: SETUP GUIDE (WINDOWS)
**MCP is optional; Rojo remains the code sync source of truth.** There are multiple community Roblox Studio MCP bridges with different installation commands. Never copy a made-up server name, command or repository URL. Choose a maintained bridge, inspect its source, verify its permissions and follow that bridge's actual README. A GitHub connector or MCP connection to GitHub is NOT a connection to Roblox Studio.

### A. Prerequisites
1. Install Roblox Studio and create a private test place.
2. Install Codex desktop app/CLI or editor extension from official OpenAI distribution; sign in.
3. Install Node.js LTS only if your chosen MCP bridge needs Node; some bridges use Python or a binary. Check its README.
4. Install Git, Rojo and the Rojo Studio plugin (see section 1–3 above).
5. Identify an actively maintained **Roblox Studio MCP server + matching Studio plugin**. Check last update, exact installation command, plugin provenance, tool capabilities and permissions. Never run arbitrary install scripts or provide Roblox login cookies.
6. Keep Studio open on a private test place during connection tests.

### B. Register the MCP server with Codex
**Option 1: Codex CLI (recommended when bridge documents an executable command).**
```powershell
codex --version
codex mcp --help
codex mcp add --help
```
Use the real server name and launch command documented by the chosen MCP bridge. Example *syntax only* (NOT an installable server):
```powershell
codex mcp add roblox-studio -- <ACTUAL_BRIDGE_EXECUTABLE> <ACTUAL_ARGS>
codex mcp list
```
Replace angle-bracket placeholders from the bridge's README. If a bridge instead provides an HTTP/SSE endpoint, use the transport flags supported by **your installed Codex version**; inspect `codex mcp add --help` rather than guessing.

**Option 2: Codex configuration** — inspect the current Codex MCP documentation and your local `~/.codex/config.toml` format. Do not paste placeholder command names as real commands. Keep credentials out of Git and screenshots.

### C. Pair Studio plugin with MCP server
1. Install the bridge's **matching** Studio plugin from the developer's official instructions; do not mix plugins from different projects.
2. Start the MCP server if the bridge requires a standalone process.
3. Open Studio test place and enable/connect the plugin to localhost as documented.
4. Studio may prompt for HTTP requests / plugin permissions depending on bridge; enable only the minimum required for the verified bridge, and never expose a local endpoint publicly.
5. In Codex ask: "List connected MCP servers and Roblox Studio tools. Do not change the game." If no tools appear, stop and debug.
6. Ask Codex: "Read the current Studio place name and list Workspace children. Do not edit." Compare with Explorer.
7. In a **throwaway test place**, ask Codex to create a single anchored Part named `MCP_Connection_Test`; verify in Explorer. Delete it, then test reading again.
8. Do not allow unreviewed destructive commands, mass deletes, publishing, credential access or marketplace purchases through MCP.

### D. If no bridge is installed
You can still use Codex effectively: Codex edits `src/**/*.luau` → `rojo serve` → Rojo plugin syncs scripts to Studio → manual Play testing → Codex reviews logs pasted from Output. MCP is a productivity enhancement, not a blocker.

### E. Common failure checks
- Codex shows zero tools: wrong server launch command, process crashed, config not loaded, restart Codex.
- Codex tools present but Studio unavailable: plugin missing, incompatible version, Studio closed, port mismatch or local HTTP permission.
- Changes in Studio not reflected in Git: MCP modifies live place objects, not necessarily Rojo source files. Record/export intentional changes and avoid editing Rojo-owned scripts via MCP.
- Security: never grant an unverified bridge internet-exposed endpoints, Roblox auth cookies, or access to production place before review.

### F. One-time verification prompt
```text
We are building MUTANT FACTORY from this repository. Read all docs.
First, enumerate the MCP servers and Roblox Studio tools actually connected.
Do not pretend tools exist. Do not modify production data.
If Studio MCP is available, read-only inspect the open test place and list
Workspace children. If unavailable, tell me exactly which prerequisite is
missing. Keep all Luau source code in Git/Rojo; use Studio MCP only for
verified Studio operations. Do not install untrusted plugins.
```

## 10. VERIFIED OFFICIAL ROBLOX STUDIO MCP — CODEX APP (NOV 2026)
This section **supersedes section 9's generic community bridge selection** for current Roblox Studio. Prefer Roblox's built-in MCP server, no third-party Studio plugin required.

**Official source:** https://github.com/Roblox/roblox-studio-agent-plugin and https://create.roblox.com/docs/studio/mcp . The official agent plugin includes Codex setup guidance; Roblox Studio already ships its own MCP server.

### Preferred Quick Connect (Windows)
1. Update Roblox Studio, sign in, and open a **private test place**.
2. Studio → Assistant → ... → Settings → MCP Servers (older versions: Manage MCP Servers).
3. Enable **Studio as MCP server**.
4. Under Quick Connect, enable **ChatGPT (codex)** if offered.
5. Restart Codex App or start a new Codex thread so MCP tools load.
6. Ask Codex to list open Studio windows and inspect Workspace **read-only**. Verify it reports the correct place.

### Fallback: manual registration (only if Quick Connect unavailable)
PowerShell check: `Test-Path "$env:LOCALAPPDATA\Roblox\mcp.bat"` . If false, update/reinstall Studio first. Check `%USERPROFILE%\.codex\config.toml` for an existing `[mcp_servers.Roblox_Studio]` entry and **do not duplicate** it.

Add to `%USERPROFILE%\.codex\config.toml`:
```toml
[mcp_servers.Roblox_Studio]
command = "cmd.exe"
args = ["/c", "cd /d %LOCALAPPDATA%\\Roblox && .\\mcp.bat"]
```
Restart Codex and test read-only inspection. CLI alternative if installed: `codex mcp add Roblox_Studio '--' cmd.exe /c 'cd /d %LOCALAPPDATA%\Roblox && .\mcp.bat'` . The quoted `'--'` is important in PowerShell.

**No duplicate MCP servers:** if an older community bridge is already registered, do not run both. Prefer the official `Roblox_Studio` entry. Back up the test place before allowing Codex to edit Studio. Use Git/Rojo as source of truth for scripts; direct Studio MCP edits may not write back to Git.
