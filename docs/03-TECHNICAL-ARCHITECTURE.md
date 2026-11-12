# 03 — TECHNICAL ARCHITECTURE
**Language:** typed Luau | **Runtime:** Roblox server+client | **Source:** GitHub + Rojo partial management | **Map/assets:** Roblox Studio, not Rojo-generated meshes.

## 1. Proposed code repository
```text
rblxgm/
├── README.md
├── docs/
├── default.project.json             # add in implementation milestone
├── rokit.toml                       # pin actual installed tools
├── selene.toml / stylua.toml        # lint/format config
├── src/
│   ├── shared/
│   │   ├── Config/                    # item catalog, formulas, remotes
│   │   ├── Types.luau
│   │   └── Util/
│   ├── server/
│   │   ├── Bootstrap.server.luau
│   │   ├── Services/
│   │   │   ├── ProfileService.luau   # our own module, NOT third-party package name
│   │   │   ├── PlotService.luau
│   │   │   ├── CreatureService.luau
│   │   │   ├── EconomyService.luau
│   │   │   ├── MachineService.luau
│   │   │   ├── MutationService.luau
│   │   │   ├── TutorialService.luau
│   │   │   ├── PurchaseService.luau
│   │   │   └── AnalyticsService.luau
│   │   └── Controllers/              # remotes and validation
│   └── client/
│       ├── Bootstrap.client.luau
│       ├── Controllers/
│       ├── UI/
│       └── FX/
├── tests/
└── assets/
    └── ASSET-REGISTRY.csv           # metadata only, NEVER pirated commercial files
```
Proposed Rojo mapping: `src/shared`→ReplicatedStorage/Shared, `src/server`→ServerScriptService, `src/client`→StarterPlayer/StarterPlayerScripts. Remotes are explicitly created by a server bootstrap or Rojo-managed shared definitions. Art is maintained separately in Studio; protect manually authored instances with `$ignoreUnknownInstances` where appropriate.

## 2. Server authority and trust boundary
Never trust client input for:
- cash amounts, creature rarity, seeds, speed, completion, purchase proofs, object IDs, remote sender, plot ownership.
- All requests carry **intent only**, for example `UpgradeMachine(machineGuid)`, not `GiveCoins(5000)`.
- Validate data types, length/range, state preconditions, cooldown, ownership, server-side distance if relevant, and per-user remote call rate.
- Use explicit allowlist enums, reject unknown IDs, limit payload size. Keep all paid content entitlements server verified.
- Use attributes or server-owned tables for immutable configuration; never use client physics ownership as an authority for currency.

## 3. Services and APIs (contracts)
| Service | Responsibility | Example API |
|---|---|---|
| ProfileService | session locks, load, autosave, migration | Load(userId), WithProfile(userId, operation), Release() |
| PlotService | assign & release plot, factory layout | AssignPlot(player), Place(machineId, gridCell) |
| CreatureService | creature ownership, capacity, assignments | Hatch(ticketId), Assign(creatureGuid, stationId) |
| EconomyService | coins and upgrade pricing | SpendCoins(userId, amount, reason), GrantCoins(...) |
| MachineService | production & upgrades | Upgrade(machineGuid), ClaimProduction() |
| MutationService | recipe validation, outcomes | Craft(recipeId, creatureGuids[]) |
| TutorialService | story steps & tutorial rewards | CompleteStep(stepId) |
| PurchaseService | Pass ownership & receipt processing | GrantReceipt(receipt), ReconcileEntitlements() |
| AnalyticsService | sampled funnel and game events | Emit(eventName, anonymousProps) |

`WithProfile` should be atomic in a single server profile context; queue concurrent mutations per player rather than data racing.

## 4. Profile schema example
```lua
Profile = {
 schemaVersion = 1,
 currency = { coins = 0, catalysts = 0, challengeTickets = 0 },
 factory = { layoutVersion = 1, machines = {}, decorations = {}, unlockedSlots = 2 },
 creatures = {}, -- GUID keyed to { speciesId, level, xp, genes, mutationTags, assignmentId }
 collection = {}, -- speciesId → { discovered = true, variants = {...} }
 tutorial = { step = 0, completed = false, claimedRewardIds = {} },
 questProgress = {},
 purchases = { grantedPurchaseIds = {}, entitlementIds = {} },
 stats = { playSeconds = 0, lastSeenUnix = 0, createdAtUnix = 0 },
}
```
- Schema version with migrations, strict defaults and backwards compatibility.
- Profile cache plus session locking and autosave, Release on PlayerRemoving and BindToClose.
- DataStore limits: retry with exponential backoff and jitter; stay within request budgets, batch/debounce writes. Do not save every tick.
- Never enable production data writes in ad-hoc testing; separate Studio/test datastore names and use a test universe if possible.
- Idempotent transaction IDs for rewards/purchases. Session lock expiry and recovery needed after unclean shutdown.
- Store timestamps as server Unix times; clamp implausible values.

## 5. Remotes
**Client→server intents:** RequestPlaceMachine, RequestUpgradeMachine, RequestClaimProduction, RequestHatch, RequestAssignCreature, RequestCraftMutation, RequestEquipCosmetic, RequestTutorialAction.
**Server→client:** InitialSnapshot, DeltaState, ActionResult, VisualEvent, Notice.
All request handlers have cooldown limits, payload validation, and unified error codes (INVALID_INPUT, NOT_OWNED, COOLDOWN, CONFLICT, CAP_REACHED, SERVER_BUSY). Only send player-specific state to that player; avoid revealing private inventories.

## 6. Runtime and performance budgets (proposed, measure on devices)
- Aim for steady 30+ FPS on low-mid mobile devices with reduced FX; 60 FPS on capable PC, without guaranteeing it.
- Maximum visible active creatures per plot: 6 prototype; animation frequency LOD by distance.
- Avoid per-frame server loops for economy. Aggregate production per profile every few seconds or calculate on demand.
- Avoid excessive moving unanchored parts, expensive shadows, many ParticleEmitters, and large transparent overlays.
- Pool FX; stream appropriate world sections; throttle replication and remote deltas; use model bounding boxes for culling.
- Measure via Roblox MicroProfiler, Stats, client device emulator, realistic devices and multi-client studio sessions.

## 7. Security and failure scenarios
- Disconnect midway hatch: state persists as job with start/end timestamp and explicit claim semantics.
- Repeat hatch requests: idempotent operation key and state guard prevent duplicate.
- Parallel server joins: session lock + reconciliation.
- Purchase receipt: one global ProcessReceipt handler, no conflicting assignments; transaction journal retains identity.
- DataStore outage: present non-purchasing safe mode or block irreversible operations; DO NOT silently discard changes.
- Raid contracts post-MVP: server-tracked state machine and anti-repeat targeting.
- Trades post-MVP: delayed until secure escrow verified under crash and retry simulation.
- Be skeptical of Toolbox free-model scripts; audit every Script/ModuleScript/Remote before using assets. Prefer scriptless meshes.

## 8. Test plan
Unit tests (pure Luau): economy formulas, inventory capacity, recipe eligibility, deterministic RNG, price caps, schema migrations. Integration tests: plot ownership, remotes, autosave/reconnect, purchase receipt replay, concurrent claims. E2E: 8 clients, first-time mobile onboarding, shutdown mid-transaction, rapid reconnection.

## 9. Change controls
- Every system needs interface, server-side invariants, test coverage, changelog.
- All code through PRs; avoid editing same feature in both Studio and filesystem. Commit versions pinned and review diffs.
- A deployment only follows green tests + technical QA + economy balancing + content compliance.
