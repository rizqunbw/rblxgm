# 05 — PREMIUM 3D ASSET + ART DIRECTION BIBLE
**Goal:** premium-looking, recognizable stylized 3D with mobile-grade performance. Premium appearance comes from consistent visual direction, animation, lighting, and silhouettes — not simply buying high-poly models.

## 1. Visual identity
- **Theme:** neon-biotech toy factory; cozy-industrial, accessible and bright.
- **Shape language:** rounded bevels, chunky machinery, clear outlines, asymmetric cartoon creature shapes.
- **Palette (draft):** dark navy #172039, electric teal #46E5C4, magenta #ED66D9, amber #FFBD59, lavender #9D91FF. Build accessibility alternate palette.
- **Materials:** mostly matte/painted metal; selective emissive accents; avoid mirrors/expensive translucency.
- **Scale:** avatars Roblox-size; creature 2–5 studs; machinery 4–12 studs. Test camera readability before finalizing.
- **Silhouette review:** recognize each creature from 24x24 thumbnail and 15-stud camera distance.
- **Animation style:** squash-and-stretch, expressive anticipation/follow-through; never import motion blindly without retarget testing.
- **Target:** 1 cohesive asset pack direction, even when sourced from multiple libraries. Recolor/retexture and adjust proportions under licenses.

## 2. Preferred sources and license workflow
| Source | Link | Best use | Commercial terms / caution |
|---|---|---|---|
| Roblox Creator Store | https://create.roblox.com/store | scriptless props, materials, sound | check creator ownership/asset permissions, inspect scripts; don't treat every toolbox model as safe |
| Kenney | https://kenney.nl/assets | factory blocks, UI, icons, terrain/props | individual game assets CC0; official support confirms commercial use |
| Quaternius | https://quaternius.com | animated stylized monsters, environment, machinery reference | QAL v1.0 permits commercial incorporation/edits; no standalone redistribution; read specific pack license |
| Poly Pizza | https://poly.pizza | backup low-poly props | per-model license varies (CC0/CC-BY etc.); record exact license + attribution requirements |
| Sketchfab | https://sketchfab.com | handpicked hero props | licensing varies by model and seller; verify derivatives/redistribution/commercial and Roblox use |
| CGTrader | https://www.cgtrader.com | optional purchased hero assets | read specific product license; not every license allows all distribution modes |
| Blender | https://www.blender.org | modify, optimize, rig and export | free authoring tool; all source rights still apply |
**Recommended free/premium-looking MVP:** Kenney foundation props + Quaternius high-quality stylized base assets + 6 **customized original monster models**, custom animation timing, selective VFX. Do not ship a mixed bag of unrelated Marketplace designs.

### Licenses verified in research (Nov 2026)
- https://kenney.nl/support: individual downloadable game assets CC0; permitted for commercial projects.
- https://quaternius.com/license.html: QAL v1.0 current license with commercial rights and explicit prohibition on redistribution of assets as standalone assets.
- https://create.roblox.com/docs/production/creator-store: official asset marketplace overview.
**Check license again at download date; specific packs may have separate terms.** Never commit paid source meshes to this public repo without written distribution rights.

## 3. Acquisition checklist per asset
1. Locate creator's original pack (avoid reuploads).
2. Record asset name, creator, URL, download date, exact license version/screenshot/receipt, attribution, edit permission, publication use.
3. Confirm rights for Roblox public distribution and sublicensing if applicable; unresolved = blocked for release.
4. Check asset formats, poly count, materials, rigs, animation quality, and model parts.
5. Inspect imported files in Blender and virus/script exposure in Studio; external mesh imports do not imply scripts, but Toolbox models may contain Lua.
6. Clean textures/UV, correct face normals, pivot at ground center, rename all meshes.
7. Rebuild final look to match palette; use atlas/trim sheets where appropriate.
8. Record final Roblox asset IDs only after authorized upload, verify permissions.
9. Link to source and derivative transform record in Asset Registry.

## 4. MVP asset list and budgets (initial hypotheses, tune in Studio)
| Asset | Quantity | Visual notes | Target triangles |
|---|---:|---|---:|
| Factory island / floor kit | 1 modular set | 8 plot bases, pathways | 500–3,000 per module |
| Coin Press | 1 (+skins) | rhythmic piston and coin VFX | 1,500–4,000 |
| Incubator | 1 | clear glass-like egg dome w/ reduced transparency | 2,000–5,000 |
| Gene Lab | 1 | tubes, rotating arms, light strip | 2,000–5,000 |
| 6 mutant species | 6 | unique silhouettes, 2–3 materials each | 1,000–4,000 each |
| Decoration props | 12 | signs, crates, lamp, railings, neon tubes | 200–1,200 each |
| Egg mesh | 1 | stretch animation | 300–800 |
| VFX | 5 | burst, coin, hatch, upgrade, mutation | bound emitter count and lifetime |
| SFX | 12 | coin, hatch, machine, menu, warning | short and compressed |
These are **design estimates**, not Roblox engine hard limits. Benchmark draw calls, visible tris, network bandwidth and memory on real target hardware.

## 5. Creature art brief
Each creature needs:
- front/side/3/4 concept views, silhouette test, color-swappable regions,
- mesh, rig or simple bone setup, idle/walk/hatch/celebrate/work/tired animation,
- thumbnail, icon, 3-tier VFX intensity, sound identity, FBX/glTF master exports,
- performance fallback and optional simplified mesh LOD.

**Blobbit:** round pear-like body, 2 hornlets, one oversized eye and tiny tail; bouncy idle.
**Sparkleech:** segmented electric worm, wired antenna, lightning-shaped fins; zippy idle.
**Cogtoad:** squat toad with gear-like shoulder armor and wrench tongue; heavy hop.
**Mossmunch:** tiny mossy grazing beast, leaf mane and triangular teeth; chew animation.
**Glimwing:** tiny crystal dragonfly with diamond wing pairs; hovering.
**Prismox:** six-sided armored beetle/ox hybrid with prism horns; slow weighty steps.

## 6. Blender → Roblox Studio import
1. Download original `.fbx` / `.glb` / `.gltf` / `.obj` files and record rights.
2. Open in Blender, inspect dimensions, normals, rig bones, textures, topology, materials.
3. Apply transforms (Ctrl+A → rotation/scale), consistent scale and origins; remove nonessential hidden meshes.
4. Simplify topology as necessary; bake textures into a small atlas; use efficient materials.
5. Rig articulated monsters with suitable bones; create idle/walk/hatch animations.
6. Export FBX/glTF in a Studio-compatible fashion; verify FBX rig transformations and animation clips.
7. In Roblox Studio import using 3D Importer; inspect MeshParts, scaling, materials, collision, animations and permissions.
8. Anchor static props, set collision complexity sensibly; use invisible simplified collision parts if suitable.
9. For live animation: import animation through supported Studio animation workflow, capture real asset IDs, play via Animator/AnimationTracks.
10. Test 8 plots × 6 monsters simultaneously on target mobile device; cut detail/FX before compromising frame rate.

## 7. Premium polish order (most value first)
1. Strong composition, color palette and readable silhouettes.
2. Character animation and satisfying reveal cinematics (<3 seconds, skippable).
3. Scene lighting and volumetric cues with low client cost.
4. Sound effects and subtle camera feedback.
5. UI typography, click feedback, rarity treatment and visual cohesion.
6. Extra geometry LAST.

## 8. Asset acceptance review
- Source license recorded and permitted; no questionable rip from commercial games/anime/film.
- Mesh clean, correct scale, light enough for mobile, proper collision.
- No unknown scripts inside Toolbox assets, no suspicious require() modules or hidden remotes.
- Has low-quality setting fallback, no unbounded particles/sound stacking.
- Animation understandable from idle screen; effects do not obscure UI; no flashing hazard.
- Final model independently reviewable in Studio; no unsupported claim that the repo contains ready-to-use meshes.

## 9. Asset Registry fields
`assetKey,category,sourceURL,creator,license,licenseVersion,downloadDate,attributionRequired,distributionRightsVerified,sourceLocalPath,robloxAssetId,polyCount,textureSize,rigged,reviewer,status,notes`
Store metadata; keep files in a separate private storage location if redistribution rights do not permit a public repo.


## 10. FREE-ONLY ASSET POLICY (MANDATORY, overrides optional paid sources above)
**Budget for third-party 3D asset licenses: $0.** Do not buy assets, use trials that expire, download rips, or assume a model is free just because it is visible online. All game models, environments, animation clips, textures, SFX and icons must be sourced from verified free-for-commercial-use licenses, made in-house, or created from those sources under permitted derivative terms. "Premium" refers to polish, not purchase price.

### Approved acquisition shortlist
1. **Kenney** — https://kenney.nl/assets — modular 3D, props, UI, audio. Check specific asset license, generally CC0. Search keywords: industrial, platformer, sci-fi, UI, interface.
2. **Quaternius** — https://quaternius.com — stylized 3D packs, characters, creatures and environments. Review pack-specific license and QAL; commercial game use can be permitted but do not redistribute raw asset packs. Search animated, monsters, robots, modular, sci-fi.
3. **Poly Pizza** — https://poly.pizza — curated stylized props. FILTER to CC0 where possible; CC BY requires attribution and license record. Never assume every listing is CC0.
4. **OpenGameArt** — https://opengameart.org — free game art and audio; licenses vary per listing (CC0, CC BY, GPL, etc.). Prefer CC0/CC BY with attribution, reject ambiguous licenses.
5. **Roblox Creator Store** — https://create.roblox.com/store — Studio-native models, materials, audio. Verify permissions, license, script contents and asset provenance; never import unknown scripts.
6. **Mixamo** — https://www.mixamo.com — useful humanoid animations if compatible with creature rigs. Verify current account/license and Roblox commercial use before publishing; not a source for arbitrary quadruped rigs.
7. **Blender** — https://www.blender.org — free tool to adapt legally obtained meshes, optimize and animate. Blender itself is not an asset library.

**Not approved by default:** paid CGTrader/Sketchfab listings, subscription-only libraries, ripped Roblox games, copyrighted franchises, AI-generated clones of existing characters, assets with "personal use only", assets whose license cannot be proven. A free Sketchfab listing is not automatically commercially licensed.

### Suggested download order (do not invent pack URLs)
- Factory floor/walls/paths: Kenney modular 3D packs; fallback Quaternius modular environment.
- Machines: Quaternius sci-fi props and Kenney industrial props; assemble 3 distinctive machines in Studio from scriptless MeshParts.
- Creatures: Quaternius free stylized/animated creatures **as bases only**; adjust silhouette, eyes, colors, attachments and animations to create the six original mutant species. If unsuitable, build simple original meshes in Blender from primitive forms.
- UI/icons: Kenney UI assets, then original vector-style icons.
- FX/SFX: Kenney audio packs or verified CC0 OpenGameArt; record source and license.
- Animation: native Roblox Animation Editor for creatures; Mixamo only when rig-compatible and license verified.

### Exact asset selection worksheet
For each proposed asset, add one row to `assets/ASSET-REGISTRY.csv` BEFORE import:
`assetKey,category,sourceURL,creator,license,licenseVersion,downloadDate,attributionRequired,distributionRightsVerified,sourceLocalPath,robloxAssetId,polyCount,textureSize,rigged,reviewer,status,notes`.
Status progression: CANDIDATE → LICENSE_VERIFIED → IMPORTED → OPTIMIZED → QA_APPROVED. Do not ship CANDIDATE assets. Record attribution in game credits where required. Keep downloaded raw packs out of public GitHub unless license explicitly allows redistribution.

### Import checklist (Windows)
1. Download from official creator page, save a copy of license/URL.
2. Extract to a private local `AssetSource/` directory (gitignored).
3. Open in Blender and inspect geometry, textures, bones and material slots.
4. Remove irrelevant meshes, reduce polygons, fix normals and pivots; export FBX or glTF.
5. Studio → 3D Importer → import, verify orientation, scale, collisions and materials.
6. For animation, use Animation Editor/import supported clips, publish and record actual IDs.
7. Inspect all descendants of Creator Store models; remove unexpected scripts/modules.
8. Test all assets with 8 plots and 48 visible mutants on low-end mobile quality settings.
9. Update registry with final ID, review evidence, attribution and QA status.

### Codex asset instructions
Codex must NOT claim it downloaded, imported, licensed, rigged, or animated an asset without proof. It may produce search lists, asset registry entries, scripts to instantiate approved models, and exact manual Studio actions. No guessed Roblox asset IDs. Provide a placeholder fallback for every unavailable asset.
