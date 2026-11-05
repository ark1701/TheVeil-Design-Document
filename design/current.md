# Current design decisions

Design baseline · November 4, 2026 · Arlen’s reconciled Unity handoff, sections 1–6. These are requirements and provisional values, with the distinctions retained. For detailed docking, weapon and targeting behavior see [systems](systems.md). For milestone order and open questions see the [roadmap](../production/roadmap.md).

## 1. What is being built

*The Veil* is a turn-based, faction-asymmetric 3D strategy/board game intended to feel like manipulable miniatures in Mixed Reality, with tabletop-scale zoom and inspection. The primary target is Quest 3 with bare-hand input, intuitive visual/audio feedback and three distinct virtual management tablets. It is a **strategic, round-based** game, not real-time fleet combat.

Maps: Galaxy and within-star-system navigation use 2D hex cells; battles use a logical 3D board packed with **rhombic-dodecahedral cells**, up to 12 face-adjacent neighbors (six roughly horizontal, three above, three below). Encounter geometry, strategic-to-tactical mapping, third-party reinforcements, and zoomed overview are covered by earlier documents. All gameplay logic lives in authoritative, serializable state, **never in mesh/LOD/collider position**. Use stable instance IDs, owner-seat IDs, team/faction IDs and deterministic event/command resolution.

**Phase/match model confirmed:** launch modes **1v1v1** and **2v2v2**, three independent factions, each assigned a different simultaneous Economy / Expansion / Combat phase. All factions are locked in their assigned phase until the global barrier (all Ready, or host-selected deadline), any already accepted actions finish resolving, and then everyone rotates **together**. A player finished early can inspect/plan but cannot execute off-phase actions. Inactive tablet CONTENT can be viewed; action eligibility depends on current phase and explicit defensive/special exceptions. Two-party **3v3** is a possible **later architecture-compatible** mode, but do NOT implement it now. Six-way independent free-for-all is deferred. Do not assume arbitrary opposing factions can occupy Combat simultaneously in the launch modes.

## 2. Occupancy, precise placement, motion and linked formations — ALREADY DECIDED

### Logical occupancy

| Unit type | Logical space | Legal combinations in one cell |
|---|---|---|
| Capital Ship or Colony Ship | entire cell | only that one ship |
| Light Cruiser | 1/2 cell | two Light Cruisers, OR one Light Cruiser + two small craft |
| Fighter or Bomber | 1/4 cell | four small craft of any Fighter/Bomber mix; combinations with a Light Cruiser as above |

Friend and enemy ships may share a main cell if **logical occupied subregions do not overlap**. These are **volumes**, not merely a numeric capacity pool: check legal regions and paths, not just 2 × half = whole.

- **Light Cruiser:** twelve face-relative half-cell candidate locations. A plane through the exact cell center perpendicular to the chosen face-center vector defines the selected half. The UI softly highlights/flips the prospective occupied portion as the player changes choices.
- **Fighter/Bomber:** predefined quarter-cell positions from two perpendicular, face-directed planes crossing the cell center. Validate exact region non-overlap and valid subplacements as a separate geometry test; a fractional capacity count alone does not prove it.
- **Attacker-only free intra-cell translation:** the attacker can reposition within legal free parts of its current cell at zero **translation** movement points; it must choose and resolve this before that unit attacks. Cannot pass through occupied logical regions, use blocked paths, or gain free pitch/yaw/roll. Firing solutions use the selected actual logical position. Earlier notes include full-path clearance and face-entry blocking.
- **Rotation is always in place:** pitch/yaw/roll change orientation **without moving the ship's position or assigned occupied subregion**. A decorative mesh extending past a logical region boundary does not alone invalidate a legal orientation.
- Ships show a draggable preview/ghost with legal snapping; logical state updates only on commitment, not arbitrary preview movement.

### Orientations and arcs

- Ship nose / rear engines align with opposite **face-relative directions**; normal translation is in the permitted heading relative to the rear engine, with exact per-class budgets unspecified.
- Pitch/yaw follow predefined face-aligned orientations; roll provides **12 discrete 30-degree choices around the ship's own nose/engine axis**. A freely rotated manipulation ghost snaps toward the next legal orientation after the previously settled **51% threshold**. Don't force the old simplistic “24 vs 48 rotations” question back onto the user: it is superseded by this more specific system.
- Facing **does matter** tactically (movement, attack arcs and cover), but exact quantitative cone/arc/range costs may remain data-driven. The Puritan upper-hull coverage, Pythagorean tentacle broad coverage and Ionian heavy-beam line/collateral rules appear in the companion Gameplay document.

### Linked formations

- Four Fighters OR four Bombers sharing a cell can link into a **Squadron**. Two same-kind Squadrons in adjacent cells form a **Group**; three or more adjacent Squadrons form a **Wing** (original example describes three/four).
- Two Light Cruisers in a cell can link into a **Battle Group**; adjacent Battle Groups may link into **Fleets**.
- Adjacent Capital Ships may also link into Fleets. Mixed linked formations/ships can create an **Armada**.
- These links are for **command convenience**, not unapproved bonuses. Each separate movement/rotation axis (translation, pitch, yaw, roll) is bounded by the **lowest remaining allowance for that axis** among linked members. Faster members retain unused allowances; unlinking may allow them to continue individually. Linked ships turn in place in their own logical spaces; do not orbit a common pivot unless the user explicitly changes the rule.
- Fine formation details (path conflicts, simultaneous destinations, exact interpolation, adjacency invariants) can initially be flagged **TBD**, not treated as prerequisites for the first vertical slice.

## 3. Three MR tablets and spoken UI — DECIDED

Three independent, simultaneously visible/summonable/dismissible/grabbable/resizable virtual tablets: **Economy**, **Expansion**, **Space Navy/Combat**. One reusable shell/interaction architecture, separate applications/role state. The detailed three-slot/default position, 30-degree flanking and handle/docking/content-rotation behavior is in the companion Gameplay document; a tablet's physical location is not itself the current phase. Inactive-phase tablets stay inspectable; they do not grant off-phase action rights.

Input: **hand/poke/pinch first**, controllers optional secondary. Game SPEAKS short prompts and feedback with equivalent on-screen text; **do not implement speech-command recognition** as a dependency. Provide muted/offline accessibility. Every interactable uses unified state-driven outline/highlight + spatial audio; Quest bare hands have no physical haptics. Avoid repeated hover sound spam. Tablet Navy targets actual unique ship INSTANCES and hierarchies (ship → systems → parts), not merely prefab types; includes hull-only quick modes and rule-validated manual subsystem selection. Full-detail ship-display copies reflect the selected instance state.

## 4. Ship subsystems, combat and targeting — DECIDED AT ARCHITECTURE LEVEL

- Hull HP and subsystem HP/damage are **independent**. Destroying a turret does not subtract hull HP and ordinarily removes **one attack die** for its associated weapon; unique piece IDs, avoiding double deduction. Other subsystem effects remain type-specific.
- Fighters ordinarily target individual subsystems on Capital/Colony/Light Cruisers; other normal attackers aim at hull, except explicit card/special-weapon exceptions. Fighters: 1 hull HP; Bombers: 2 hull HP, neither has subtargetable systems (per earlier design record).
- Representative **provisional** subsystem values from earlier decisions: Puritan turret 1 HP/hit d6 1–2; Pythagorean tentacle 10 HP/hit 1–5; Ionian eye 5 HP/hit 1–3. These are NOT universal values or final balance.
- Damaged **and destroyed** subsystems can eventually be repaired. Puritan uses resources, Pythagorean uses resources or parasite healing via eligible tentacle attachments, Ionian uses cocoon healing over turns. Costs, rates and timing TBD.
- Engines reduce each axis' capacities according to config; all destroyed engines normally block translation, intra-cell reposition and all rotations until restored or explicitly overridden by card.
- Local bridge/wing rules and same-race team Veil Card accessibility appear in the companion Gameplay doc. Other combat details include partial-cell cover and blocked connecting faces, fighter-only subsystem attacks, Nova Blast static/hull-only, cloaking without positional placeholders, hidden interception along the actual firing path, and visible destruction explosions. These rules are NOT an instruction to implement all features in the first sprint.
- Enemy cloaked ships are not selectable, outlined, announced or revealed simply on a hit. Authoritative targeting must prevent private-data leaks through hover, UI, audio or network events. The deliberate visible impact stop and explosion are allowed clues.
- Large continuous battles may expand the logical cell map toward entering units to maintain an arrival zone and separation; maintain sparse logical coordinates and separate visible overview from local-detail rendering; no fixed maximum logical zone size. Reference later design decisions; no requirement to simulate an infinite rendered grid.

## 5. Veil Card allowances and synchronous deadlines — NEWEST CONFIRMED CHANGES

- Per **faction/team**, per **combat zone**, per **applicable combat phase/rotation**: **one attacking Veil Card** allowance and **one defensive Veil Card** allowance, separate from each other; not per individual teammate or fleet.
- If allied teammates submit competing attacking Veil Card proposals in one zone, let them discuss and select one; if unresolved by deadline, the **authoritative game rolls dice** to choose one eligible proposal. Only the chosen, accepted card is played. Unchosen proposals stay in their owners' hands and spend no allowance. Never retroactively authorize a card outside its permitted timing window.
- The card is **committed when server/authority validates and accepts** the play, not when merely highlighted/locally selected. Opponent counters may occur afterward. If subsequently CANCELED by another legal effect, the card **returns to its owner's hand but the faction's allowance in that same zone for that phase remains spent**. The card may be eligible elsewhere according to normal rules.
- Each **global synchronized phase rotation** refreshes every faction's defensive-card opportunity in each combat zone, including continuous encounters; it does not generate new cards or change each player's independent hand.
- Once a phase closes (Ready barrier or timer), **no new attack starts**. Existing accepted attacks, eligible defensive reactions and counters resolve fully against the OLD phase rules and card allowances. **Then** everyone rotates together and allowance resets occur.
- Combat-role attacker/defender is assigned per declared attack. Defender-favoring unresolved precedence and tie arbitration have prior decisions, but exceptional multi-target conflicts may remain deferred. Specific card text determines timing and legal counterplay; don't invent generic counter loops.

## 6. Mandatory AFK reaction and boot/rejoin policy — NEWEST CONFIRMED CHANGES

Do NOT let AFK handling grow into the next implementation rabbit hole. Keep gameplay core independent, and build this as a separate **match coordination policy** implemented in stages. The following decisions are confirmed, though some durations and rare race cases remain open.

### Defensive reactions
- A separate configurable **defensive response timer** applies even when main phases are untimed.
- At expiry for a solo player: **automatic ordinary Evasive Action** rolls against incoming hits, no Veil Card automatically spent, no Redeploy.
- In a team match: offer the timed-out player's teammate **10 additional seconds** to resolve that mandatory decision, with temporary authority **even without ordinary prior delegation**. Responding teammate may temporarily see/use the AFK player's eligible defensive cards, subject to shared faction allowance and legal timing. If nobody acts, automatic Evasive Action.
- The original owner, if actively returning during those 10 seconds, immediately regains priority over uncommitted teammate choices, **but the countdown never resets or pauses**. Previously authoritative accepted actions cannot be retracted or retroactively undone. Temporary card visibility ends when the emergency ends (already revealed information cannot be made unknown).
- Automatic emergency power is for **mandatory unresolved reactions only**; it never grants permission to initiate optional offensive orders or new attacks after the phase deadline. Existing voluntary delegation is a distinct mechanism.

### Untimed all-Ready barrier
- Potential AFK identified via **hybrid** meaningful-inactivity monitoring OR a teammate request (teammate requests allowed only where lack of Ready actually blocks phase advancement; repeated requests subject to cooldown). Neither inactivity nor an accusation alone proves AFK.
- Show explicit presence challenge for host-configurable duration (**30 seconds proposed default, not a final mandatory value**). If unanswered: teammate gets **10 seconds** to mark verified-AFK player Ready; if teammate also fails, authority automatically marks that player Ready. Previously accepted orders persist; the system invents no optional orders.
- Once verified AFK, status persists across later rotations until player actively returns, not a new 30+10-second loop every phase.

### Unanimous removal / reinstatement
- All OTHER active players may **unanimously vote to boot a verified AFK player**. If unanimously agreed, their sole remaining teammate takes temporary command of **both portions** of their faction (this is a special exception to limited emergency authority). If no unanimous result, baseline persistent-AFK handling continues.
- Booted player may request reinstatement, but **ALL remaining active players must unanimously approve**. Approval is immediate; transfer of command authority occurs at **next global synchronized phase rotation**, after ongoing accepted combat and mandatory reactions resolve.
- Original asset ownership is **preserved** while the active teammate temporarily commands both seats. Ships newly produced by a facility belong to that facility's owner. Newly awarded Veil Cards go into their originally intended recipient's distinct hand, although temporary commander may play them. Upon reinstatement, unused cards and surviving owner-assigned assets return to original player's private control.
- Temporary commander can move, attack, risk ships and legally use cards/resources, but **cannot permanently reassign ownership**, sell/scrap/dismantle absent player's assets, or explicitly destroy one as a deliberate sacrifice / activation cost. Restrict explicit sacrifice mechanics, **not** normal high-risk attacks; do not guess strategic motive.
- Upon reinstatement, returning owner **reviews and may keep/cancel accepted but not-yet-executed orders** placed on their assets. Completed actions are final. How cancellation should affect a teammate's linked/coordinated orders is **UNRESOLVED AND DEFERRED**, not approved as automatic cancellation or forced reconfirmation. Keep command dependencies representable/validatable without inventing the rule.
- Voting edge cases (abstentions, concurrent AFK players, disconnects) and abuse-prevention details are **TBD** and should NOT block initial pipeline work.
