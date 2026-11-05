# Engineering handoff

**Unity editor baseline:** `6000.6.4f1` (revision `12bfff696524`), reported from the working project's `ProjectSettings/ProjectVersion.txt`. Use this exact editor for the first clean-checkout test.
## Project map
| Material | Intended home | Rule |
|---|---|---|
| Rules, tablet interactions, faction capabilities | design/ | Update the applicable decision with its date and status. |
| Milestones and validation criteria | production/ | Use GitHub Issues for work in progress. |
| Playable project, once reconciled | UnityProject/ | Track Assets with matching .meta files, Packages and ProjectSettings. Open this directory in Unity Hub. |
| Package builder and editable templates, once located | Tools/ShipPipeline/ | Generate from one source and import one runtime copy. Keep templates, stubs and package-building tools outside Unity's Assets. |
| Source art and large binaries | Inventory before import | Record source, rights, editable master, exported asset and owning prefab. Decide binary storage before adding assets. |
| Prior design text and conversations | archive/baseline/ | Cite a path and promote individual decisions after review. |

## Runtime responsibilities
A command identifies the acting seat, phase, ship instance, selected part and intended logical position. Validate the command against occupancy, ownership, allowances and visibility. Commit once, resolve events in order, then update the board, tablet display and audio from the resulting state. A grab ghost can move without committing. A lower-detail mesh can replace a render model without changing a target, occupied region or HP.

**First debugging scenario:** put two instances of one Light Cruiser prefab in a scene. Damage one subsystem on the first ship. Zoom until its rendering detail changes and inspect both tablets/ships. Only the selected instance should show the damage. Repeat after save/reload. Log instance ID, part ID and state transition.

## What has run
The November 4 import review recorded a v4 test ship with six passing asset checks, nine target areas, and full/medium/fleet meshes at 2,994 / 1,204 / 310 triangles. These are single-asset editor measurements. It also recorded duplicate runtime definitions when a generator distribution and Unity stubs were present under Assets. Keep these observations separate from the current project's compile and device status.

## How to bring in the working project
1. Locate the Unity root, exact patch revision, package manifest/lock, test scene and source generator. Record the dependencies needed by the scene.
2. Stage only redistributable project inputs. Keep caches, user settings, raw logs, signing material and generated builds local. Preserve existing Unity asset paths and GUIDs in the first import.
3. From a clean checkout, open in the recorded editor and rebuild the test ship. Compare the target count, manually edited Wing_P box and part IDs before and after. Record errors verbatim with local paths and identifiers removed where appropriate.
4. Build the same scene for Quest 3. Record build result separately from device launch, hand selection, visible tablet response and measured frame timing. Attach the observations to the issue.
