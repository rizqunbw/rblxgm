# MUTANT FACTORY: MUTATE • DEFEND • RAID
> Production Bible v1.0 — Roblox multiplayer collection/tycoon game. Status: **design approved for prototyping, no game code implemented yet**.

## Vision
An original, mobile-first 3D cartoon factory simulator where players discover collectible creatures, build production lines, discover mutations, defend their factory, and perform short opt-in raids. 8 players per server target. A first-time player should build their first machine in 30 seconds, hatch their first mutant within 90 seconds, and understand the next goal within 3 minutes.

**Product pillars:** immediate fun, visible progression, expressive creatures, fair social tension, fast mobile performance, ethical monetization.

**Player fantasy:** "I built this absurd monster factory, discovered a one-of-a-kind mutant, and survived another player's ridiculous heist."

**Differentiators vs steal-and-collect clones:** recipe-driven mutation experiments (not just random conveyor acquisition); customizable defensive factory paths; non-destructive insurance and opt-in raid contracts; co-op weekend world boss; mutant genealogy/collection index; deterministic discovery milestones.

**Audience:** Roblox mobile-first general audience; comply with Roblox age-based communication, monetization, and regional rules. No artificial urgency, misleading odds, or harassment rewards.

## How to use this repository
- Read [Game Design Document](docs/01-GAME-DESIGN.md), [Economy and Monetization](docs/02-ECONOMY-MONETIZATION.md), [Technical Architecture](docs/03-TECHNICAL-ARCHITECTURE.md).
- Follow [Codex + Roblox Studio Setup](docs/04-CODEX-ROBLOX-STUDIO.md) *in order*; use its prompts one milestone at a time.
- Download and evaluate assets using [3D Art Production Bible](docs/05-ASSET-PIPELINE.md).
- Execute [Roadmap, Team and QA](docs/06-PRODUCTION-QA.md), and [Analytics and Live Ops](docs/07-ANALYTICS-LIVEOPS.md).
- Tasks are **specifications**, not claims of completed features. Do not publish until security, receipts, data saves, performance, and platform compliance are verified.

## First playable vertical slice (non-negotiable)
One factory map, 8 plots, 6 original mutants, 3 production machines, coins and upgrades, incubation and one deterministic mutation recipe, functional save/load, guided onboarding, mobile HUD, and one cosmetic shop item. **Raids, trading, social economy and paid randomized products are OUT of this slice.**

## Player lifecycle
Join → reserve plot → tutorial → build starter incubator → receive free starter egg → hatch → place mutant at assigned work station → produce coins → upgrade conveyor/lab → craft a mutation → fill collection index → customize plot → raid opt-in (later) → co-op event → return for new experiments.

## Studio systems and ownership
- **Server-owned:** currency, item ownership, inventory, rewards, mutation results, timers, raids, purchases.
- **Client-owned:** HUD, camera, local visual effects, animation smoothing, view-only optimistic previews.
- **Content-driven:** creature definitions, mutations, recipes, machine stats, cosmetic catalog, event schedules.

## Release gates
A release requires deterministic tests of purchases/reconnect, multi-client playtests, mobile performance verification on target devices, no exploitable remotes, and completed asset-license registry.

## Documentation index
| File | Purpose |
|---|---|
| docs/01-GAME-DESIGN.md | Narrative, mechanics, game loops, state diagrams, onboarding, social play |
| docs/02-ECONOMY-MONETIZATION.md | Sources/sinks, pricing hypotheses, premium catalog and fraud prevention |
| docs/03-TECHNICAL-ARCHITECTURE.md | Server/client layout, data schemas, remotes, saving, security, testing |
| docs/04-CODEX-ROBLOX-STUDIO.md | Exact Windows setup, Rojo workflow, milestone prompts, acceptance criteria |
| docs/05-ASSET-PIPELINE.md | Premium-looking asset sources, licenses, Blender→Studio, budgets, art direction |
| docs/06-PRODUCTION-QA.md | 20-person team structure, sprint plan, QA, performance, readiness |
| docs/07-ANALYTICS-LIVEOPS.md | Funnel KPIs, events, telemetry, A/B, launches and player support |

## Reference documentation
- Roblox Studio: https://create.roblox.com/docs/studio
- Rojo workflow: https://create.roblox.com/docs/projects/rojo
- Creator Store: https://create.roblox.com/docs/production/creator-store
- Developer Products: https://create.roblox.com/docs/production/monetization/developer-products
- Paid Random Items: https://create.roblox.com/docs/production/monetization/paid-random-items
- Roblox asset imports: https://create.roblox.com/docs/assets


## Mandatory free asset policy and Studio MCP
All third-party game assets must be **free and verified for commercial use**. See [Free-only asset policy](docs/05-ASSET-PIPELINE.md#10-free-only-asset-policy-mandatory-overrides-optional-paid-sources-above). Optional paid sources mentioned earlier in the asset guide are superseded by this rule. Do not commit restricted source assets to this public repository.

For Codex → Roblox Studio connectivity, follow [MCP setup on Windows](docs/04-CODEX-ROBLOX-STUDIO.md#9-codex--roblox-studio-mcp-setup-guide-windows). MCP is optional; Rojo remains the preferred Luau source synchronization mechanism.
