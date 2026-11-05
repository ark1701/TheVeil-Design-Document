# Systems reference

Earlier November gameplay record. Read [current decisions](current.md) first. In particular, defensive cards have a separate allowance, and a host deadline can close a phase alongside the all-Ready condition. Those later decisions supersede the older timing language below. Balance numbers marked provisional need playtesting.

## 1. Phases, teams and seats

- Combat, Expansion and Economy are simultaneous phases assigned to three opposing sides. Teammates have synchronized phases. Enemies never share the same phase.
- All three tablets' information is readable at any time. Actions require the corresponding current phase.
- Players plan actions, then commit. Once everyone has committed, orders resolve with the current phase assignments and protections; only then do phases rotate.
- During a player's Expansion phase, that player's Expansion units and factories cannot be attacked. During Economy, their Economy assets cannot be attacked. Combat Navy has no phase-based immunity. These protections do not remove other eligibility checks.
- Navy means pieces that can be attacked/destroyed, with Combat and Non-Combat categories. Colony Ships are Non-Combat Navy: movement only in Expansion; attackable outside their owner's Expansion protection.
- Supported structures: 1v1v1 and 2v2v2. Four/five humans can fill six seats using AI, or a willing teammate can control two seats by toggling between their sets. Both seats retain separate resources, card hands and ownership, even under one human. Both share the team phase.
- Exact phase cycle order, startup assignment and seat/team commitment mechanics remain unspecified. Each opponent must receive a Combat phase between another team's consecutive Combat phases.
- Combat Veil Cards are playable only in the owner's Combat phase. Economy/Expansion cards are tentative; if created, each requires its matching phase.

## 2. Three physical tablets

- Three separate tablets: Combat, Expansion, Economy. Default central position/tilt follows the user's seated, tilted desk-display image reference; exact dimensions and tilt are not measured.
- Two side positions flank the center at 30 degrees relative to it, six inches apart at the tablets' bottoms. Exact measurement anchors should be explicit in implementation.
- Players can grab, place and independently resize tablets. Released tablets stay where placed. Returning to a snap location restores default size.
- Three persistent snap positions form one movable arrangement. Each has a handle beneath it; any handle moves the entire arrangement, preserving relative spacing/angles. Docked tablets follow; undocked tablets remain where placed.
- Automatic rotation means CONTENT changes, not tablets flying between positions. Upcoming phase is assigned to left, active to center, previous to right. This content cycling continues on tablets removed from their assigned locations. Example: Economy content moves to the center-role tablet while the tablet beside the chair changes to another phase.
- Rotation can be disabled: phase content stays on each tablet. In either mode, green rim/trim/backplate indicates active phase content; red indicates inactive. World placement does not determine active status.
- Keep stable tablet role identity separate from displayed phase and physical docking state. Re-docking to different role slots and exact resize limits are not decided.
- Speak prompts/feedback; player input is touch confirmation, not speech recognition. Every spoken prompt also appears on the relevant tablet. Do not implement player voice commands.

## 3. Damage, targeting and provisional faction values

All rolls use six-sided dice. Values below are provisional but express intended faction differences.

| Subsystem | HP | Hit on d6 |
|---|---:|---|
| Puritan turret | 1 | 1–2 |
| Pythagorean tentacle/turret | 10 | 1–5 |
| Ionian eye/turret | 5 | 1–3 |

- Fighters normally deal exactly 1 damage per successful hit against any eligible piece/system.
- Only Fighters normally target subsystems of Colony Ships, Capital Ships and Light Cruisers. A Bomber Veil Card granting this ability is tentative.
- Fighters have 1 HP; Bombers 2 HP. Neither has targetable subsystems.
- Subsystem damage is separate from hull HP. Destroying a turret removes one attack die without subtracting hull HP, per established baseline. Do not generalize the turret HP/hit thresholds to every subsystem.
- Veil Cards are a core, card-by-card exception framework for all Combat and Non-Combat Navy. Specific effects remain TBD unless stated here.
- All damaged AND destroyed subsystems are fully repairable: Puritan resource repair; Pythagorean resource repair or parasite healing through a tentacle attached to a friendly/enemy ship; Ionian cocoon healing over X turns. Resource definitions, costs, timing, healing amounts and cocoon details are unresolved.
- Nearest quick target: nearest eligible enemy ship anywhere in attacker range, not just a selected cell. Distance metric/ties remain TBD.
- Random fire distributes dice among eligible enemy ship hulls anywhere in range, never subsystems. Distribution weighting and allocation details remain TBD.
- Keep manual, focus, random and blind fire on shared rule validation. Normal targeting respects protection, firing arcs, range, visibility and blocking.

## 4. Cell occupancy and sub-placement

| Unit | Cell capacity |
|---|---|
| Capital Ship / Colony Ship | One whole cell |
| Light Cruiser | Half-cell |
| Fighter / Bomber | Quarter-cell |

One cell can therefore hold two Light Cruisers; one Light Cruiser plus two small craft in any Fighter/Bomber mixture; or four small craft in any mixture. Friendly and enemy units may share a main cell if occupied regions do not overlap.

- A Light Cruiser has 12 face-relative half-cell placement choices. Select a face; a plane through the exact cell center, perpendicular to that face-center direction, defines the half. Opposite faces give complementary halves.
- Quarter-cell placement is predefined using two perpendicular, face-directed planes through the center. Two complementary quarters can occupy the half opposite a Light Cruiser. Validate this geometry and legal combinations; capacity fractions alone are insufficient to prove non-overlap.
- Faint placement highlights flip as the player chooses a region.
- Translation within available sub-cell space has no movement-point cost, but is available only to the attacker. Repositioning is planned and resolves before that unit's attack; firing checks use its resulting position. Rotation is not free.
- Occupied regions block corresponding faces/entry points. A unit may route through open neighboring cells at normal movement/rotation costs. It may not pass through occupied space. Earlier clearance discussion required the full movement path, not just destination, to be clear.
- FINAL CORRECTION: pitch, yaw and roll all turn the ship IN PLACE. Position and assigned occupied region remain fixed. Supersede the earlier ghost-parent/cell-center orbit interpretation and any rotation-induced sub-cell relocation.
- Mesh protrusion beyond an assigned region is an art issue or tolerated visual compromise, not an illegal turn. Do not let decorative mesh overlap alone reject a legal orientation. Gameplay occupancy is logical region geometry.

## 5. Rotation, movement and linked formations

- Nose and rear/engines align with opposite cell-face directions. Movement is only opposite the engine-facing direction.
- Pitch/yaw use predefined face-aligned orientations. Roll has 12 positions at 30-degree intervals around the ship's own nose-to-engine axis; its top need not align to a face.
- Grabbed ghost freely rotates in XYZ; planned anchored orientation snaps after crossing the stated 51% threshold toward the next orientation, subject to allowance/legality. Do not confuse this planned preview with immediate authoritative execution.
- Puritan Battle Cruiser: fastest Capital Ship roll, slowest yaw, intermediate pitch. Exact budgets and costs are TBD.
- Four same-type Fighters or Bombers in a cell can link as a Squadron. Two same-type Squadrons in adjacent cells form a Group; three/four form a Wing.
- Two Light Cruisers in a cell form a Battle Group; adjacent Battle Groups can form Fleets. Adjacent Capital Ships can form Fleets. Mixed linked formations can form an Armada.
- Formations primarily simplify control; no automatic bonuses have been approved. Each translation/pitch/yaw/roll limit is constrained independently by its slowest member. Faster units keep unspent allowances and can unlink after moving together to continue.
- Linked Light Cruisers may rotate together. Multi-cell formations turn locally rather than orbiting a shared formation center. Under the final in-place correction, members retain positions/occupied regions while executing coordinated orientations.
- Formation adjacency, pivot/cost accounting, differing initial facings, rotation interpolation, and conflicting simultaneous destinations need explicit implementation decisions before dependent rules are finalized.

## 6. Firing coverage and cover

- Orientation has real tactical effects. Puritan Battle Cruiser fires broadly over its upper hull, covering approximately one-third of adjacent cells, spreading outward as a cone within range. The other roughly two-thirds below/partly beside lack firing solutions. Exact geometry remains TBD.
- Pythagorean Dreadnought tentacles give broad all-around coverage but cannot all focus on one target.
- Ionian Heavy Cruiser has one powerful heavy beam cannon along a direct path, hitting units along it subject to rolls/range. It ignores blocking and damages friends and enemies. Relationship between this cannon and individual eye/turret dice is not yet specified.
- Cover uses occupied whole/half/quarter regions, NOT visible hull meshes. A fully blocked connecting face blocks ordinary fire; an open connecting face allows a firing solution within other rules.
- To traverse an intervening cell, entry and opposite exit faces must be open. Ordinary shots do not bend around obstacles. Exact ray/face handling for oblique paths and partial-face openings remains to be specified without overriding this rule.
- Destroying an obstacle does not create a second attack wave in the same resolution. All attacks are planned before commitment; newly opened paths become usable on the player's next Combat phase.

## 7. Subsystem effects and Veil access

### Engines
- Each destroyed engine reduces movement, pitch, yaw and roll by undefined amounts.
- All engines destroyed means no translation, free sub-cell repositioning, or rotation until capability is restored, except an explicit Veil Card exception.

### Bridges and Ionian wings
- Capital Ship bridge/wing capacity gates Combat Veil Cards locally to a combat zone. No qualifying Capital Ship means no cards there.
- Puritan/Pythagorean functioning Capital Ship bridges provide access. Other possible bridge effects are undecided.
- Friendly teammates can provide support ONLY for seats of the same race. Separate ownership/hands remain intact.
- Ionian wings pool across same-race teammates in that zone: 2+ functioning wings = all cards; 1 = each player independently has floor(hand count / 2) cards randomly greyed out; 0 = no cards. Thus five cards means two greyed and three available.
- Disabled selections stay fixed until wing capacity changes, not rerolled each Combat phase. Hand changes and zero-to-one transitions need further definition.
- Repair or arrival of qualifying same-race friendly Capital Ship restores the corresponding access immediately when resolved; phase restrictions still apply. No effect on independent zones.

### Hangars and Ionian conversions
- Each intact hangar on a Capital Ship or Light Cruiser carries one Fighter OR Bomber. Destroying an occupied hangar also destroys its carried craft.
- Ionian ships have no hangars. They split/merge between ship classes. Examples: two Fighters to Bomber; X Bombers to Light/Heavy Cruiser; two Light Cruisers to Heavy Cruiser. Numbers are provisional; the general mechanic is confirmed.
- Conversion timing, costs, conservation of damage/allowances, space checks and effects on wing-based card access are TBD.

### Tentacles, parasite healing and destruction cascades
- Destroying a latched tentacle severs that connection and stops parasite healing through it. Other intact attachments remain.
- Destroying an attached Pythagorean Dreadnought destroys ALL ships still attached to it, friendly or enemy. Destruction of another attached Dreadnought cascades through its remaining attachments. Implement each destruction once; cycles must terminate.
- Capital Ships and Light Cruisers are confirmed attachment targets; this expands the former Capital-only scope. Colony/small-craft eligibility and directionality of reciprocal links remain unspecified.
- Normally deliberate own-ship attacks are illegal. Exception: attack one's own Pythagorean Dreadnought while attached to another Capital Ship or Light Cruiser. This requires ordinary attacks reducing hull HP to zero; no self-destruct command.
- Attachment happens in one Combat phase; deliberate own-Dreadnought attacks are available no earlier than its owner's NEXT Combat phase. Opponents have intervening Combat phases to sever attachments. If none remain when the next attack setup begins, own-ship targeting is unavailable.
- CORRECTION: the earlier 'tentacle attacks first' discussion assumed enemies sharing Combat. The final three-side schedule removes that premise. Do not invent out-of-phase defensive attacks or same-round reactions. Remaining allied/concurrent resolution edge cases are TBD.

## 8. Maps, combat zones and reinforcements

- Galaxy and inside-Star-System maps use 2D hexes. Opposing fleets occupying the same strategic hex open a 3D rhombic-dodecahedral combat zone.
- Further arrivals join that existing battle as reinforcements or a third opposing side, not separate combat zones.
- Same-race friendly functioning bridge/wings count for card access as soon as arrival resolves.
- Combat-zone dimensions, entry placements, retreats, strategic/tactical time coupling and exact reinforcement ordering remain TBD.

## 9. Inspection, cloaking and Nova Blast

- Normally grabbing any ship displays all its ship data on the Combat tablet, including enemy data. This is inspection permission, not permission to move an enemy or view its owner's private card hand/orders.
- Enemy cloaked ships are invisible and uninspectable: no ship data and no static placeholder. A Break Cloak card can reveal them under that card's rules.
- Cloak is not damage immunity. If visible enemy C is targeted but cloaked enemy B is nearer along the SAME firing path, ordinary fire secretly resolves against B. A hidden ship elsewhere in range does not redirect that shot.
- Hits do not reveal the ship or its data. Gunfire stops visibly where it hits the hidden ship, allowing location inference. Its destruction explosion is visible. Beam penetration remains a separate exception.
- Blind fire at suspected empty locations is permitted within firing rules. Own-ship hits are prohibited except established Ionian-beam collateral and qualified attached-Dreadnought targeting. Teammate-specific restrictions are not yet defined.
- Nova Blast permits grabbing/selecting enemy ships, but the Combat tablet shows analog-TV snow/static. Targeting is hull-only. Prior knowledge of a subsystem location DOES NOT restore subsystem targeting.
- Veil Cards remain usable unless explicitly affected by Nova Blast, subject to ordinary phase and Capital Ship requirements.
- Nova/cloak durations, scopes, timing, feedback under misses, and hidden-unit participation in automatic nearest/random modes need explicit card/rule definitions. Never leak hidden data through selection highlights, target lists, hover sounds or display copies. Intended projectile stop/explosion clues are allowed.
