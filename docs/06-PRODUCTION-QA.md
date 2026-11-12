# 06 — PRODUCTION OPERATIONS: 20-PERSON STUDIO STANDARD
**Important:** this is the process *quality bar* of a 20-person team, **not** a claim that 20 people are assigned. AI/Codex can accelerate coding but cannot replace player research, art review, QA or live game judgement.

## 1. Example 20-role staffing model
| Department | Roles | Count |
|---|---|---:|
| Leadership | Executive Producer, Game Director, Technical Director | 3 |
| Design | Systems/Economy Designer, Level/UX Designer, Live-Ops Designer | 3 |
| Engineering | Lead Roblox/Luau Engineer, Gameplay Engineer ×2, Backend/Persistence Engineer, UI Engineer, Tools/Build Engineer | 6 |
| Art | Art Director, Character Artist, Environment Artist, Animator/VFX Artist | 4 |
| Experience | UI/UX Artist, Audio Designer | 2 |
| Quality & Data | QA Lead, Data/Analytics Specialist | 2 |
| **Total** | | **20** |
People may combine roles if solo-led, but *reviews* must remain separate checklists.

## 2. Phases (illustrative schedule, not a completion promise)
- Phase 0, week 1: concept, reference research, technical spike, greybox, asset-licensing shortlist.
- Phase 1, weeks 2–3: Rojo sync, plots, profiles, first machine.
- Phase 2, weeks 4–6: creatures, mutations, tutorials, collection, key visual identity.
- Phase 3, weeks 7–8: polished mobile UI, first compliant cosmetic products, telemetry.
- Phase 4, weeks 9–10: 8-client stress, mobile performance, private alpha and balancing.
- Phase 5, weeks 11–12+: conditional soft launch after metrics gates, then opt-in raids and live events. Actual work duration varies.

## 3. Sprint discipline
- Every feature has owner, problem statement, success metric, functional contract, asset dependencies, failure modes and tests.
- Design review → implementation → peer review → integration test → QA sign-off → staged release.
- Keep `main` releasable; feature branch PRs; no direct unreviewed changes to live experience.
- Daily standup checklist: completed, blockers, next step, risk. Weekly playtest and economy review.
- Milestones stop if crash/save/receipt failures detected.

## 4. Definition of Done
- All required interactions work on keyboard/mouse, touch and gamepad where supported.
- Client inputs are validated on server and malicious input tests pass.
- Code formatted, typed, modular; meaningful automated tests for logic.
- No developer console errors in clean fresh-session or reconnect.
- All visual assets have licensing evidence and meet mobile art constraints.
- New-player can complete intended tutorial without explanation.
- Telemetry events function, rollback procedure documented.
- Crash/reconnect/receipt idempotency tested and documented.

## 5. QA matrix
| Area | Scenarios | Evidence |
|---|---|---|
| Core gameplay | first-time tutorial, skipped button, full inventory, all machines | video + steps |
| Plot contention | 8 clients join; 9th waits; owner leaves | Output + state assertions |
| Persistence | normal quit, rapid rejoin, server stop, downtime | before/after state |
| Security | forged remotes, invalid IDs, negative amounts, distance spoof | rejects, no data corruption |
| Economy | 1000 synthetic sessions, caps, purchase duplicate, upgrade race | summary distribution |
| Paid purchases | Pass ownership across joins, receipt retry / replay | entitlement and journal |
| UI | phone portrait/landscape, small Android, desktop, accessibility | screenshots |
| Network | latency, packet drops, repeated button taps, reconnect | stable state |
| Rendering | full 8 plots, 48 creatures, FX settings | profiler capture |
| Moderation | inappropriate user names in public labels, communication and rights | policy review |

## 6. Launch go/no-go (proposed hard gates)
**Must-pass:** no known item dupes, no irreversible loss from saves, valid premium grants, zero critical tutorial blockers, no unauthorized assets, basic support route, optimized client fallback. Plus publish permissions, discoverability metadata and monetization compliance.
**Metrics observation (not fabricated targets):** day-one retention, 10-minute activation, tutorial completion, crash rate, session duration, purchase conversion, reported frustration. Establish baselines in private alpha before thresholds.

## 7. Release pipeline
DEV sandbox universe → QA universe with anonymized fake profiles → CLOSED ALPHA allowlist → SOFT LAUNCH → LIVE.
- Use configuration flags to disable new purchases/raids instantly.
- For code regression: roll back specific release, respecting profile schema compatibility.
- Preserve backup of place content and build artifacts; test recovery before public launch.
- Use deliberate rollout: 5% feature exposure → 25% → 100% only if telemetry supports it (implement via stable user-ID cohorts, NOT a claim of Roblox built-in rollout).

## 8. Risk register
| Risk | Impact | Mitigation |
|---|---|---|
| Copycat gameplay | low retention | unique recipes, non-destructive raids and creature expression |
| Paid asset licensing | takedown/legal risk | asset registry + audit originals and redistribution |
| Save corruption | player loss | versioned profiles, locking, backups, reconciliation |
| Exploits/duplication | economy collapse | server authority + idempotency + anti-spam |
| Performance collapse on mobile | churn | LOD, budgets, 8-client tests, quality setting |
| Aggressive monetization | complaints/trust decline | cosmetic first, transparent and reviewable offers |
| Scope creep | no launch | vertical slice before raids/trades |
| AI-generated faulty code | live incidents | Studio integration tests and independent code review |
