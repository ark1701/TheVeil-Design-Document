# Engineering handoff

**Unity editor baseline:** `6000.6.4f1` (revision `12bfff696524`), reported from the working project's `ProjectSettings/ProjectVersion.txt`. Use this exact editor for the first clean-checkout test.
## Located test prefabs
Arlen verified both paths in the separate **Ship Asset demos** project. Paths are relative to that project's root.

- `Assets/_ShipPipelineTest/Pipeline Test Ship/Prefabs/PipelineTestShip.prefab`
- `Assets/_ShipPipelineTest/Pipeline Test Ship/Prefabs/PipelineTestShip_Display.prefab`

The filename inventory confirms their location. Verification of the saved Wing_P collider edit is a separate check. The editor revision above was read from The Veil game project.

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

## Saved collider baseline — November 5, 2026
Direct inspection of the uploaded PipelineTestShip.prefab and PipelineTestShip_Display.prefab found the same Wing_P Box Collider size in both: X = 0.43321174, Y = 0.3, Z = 0.14712915. Arlen confirmed that Y = 0.3 was his deliberate enlargement. This establishes the saved baseline for future rebuild comparisons. A fresh rebuild-preservation test remains pending.

## Main project package inventory — November 5, 2026
Inspected Arlen's uploaded Packages/manifest.json and Packages/packages-lock.json from The Veil project. All direct dependency names and every dependency referenced by the lockfile have entries in the lockfile. This is a static file check. Editor compilation, enabled XR features and headset behavior still require verification.

| Package | Locked version |
|---|---|
| Universal Render Pipeline | 17.6.0 |
| Input System | 1.20.0 |
| OpenXR | 1.18.0 |
| XR Management | 4.7.0 |
| XR Interaction Toolkit | 3.6.1 |
| XR Hands | 1.9.0 |
| Meta OpenXR | 2.6.1 |
| AR Foundation | 6.6.2 |
| XR Composition Layers | 2.6.0 |
| Android XR OpenXR | 1.4.0 |

The manifest requests Rider integration 3.0.38 while the lockfile resolves 3.0.40. The development feature package also requests 3.0.40. Record this during clean-checkout validation. The manifest includes AI Assistant 2.21.0-pre.2 and com.unity.pipeline 0.9.1-exp.1. These are distinct from the custom ShipPipeline assets. Package names alone do not establish whether a feature is enabled in the project.

The demo package comparison is recorded below.

## Demo-to-main package comparison — November 5, 2026
Compared the uploaded Ship Asset demos manifest(1).json and packages-lock(1).json with The Veil's manifest.json and packages-lock.json. All shared XR, Input System and render-pipeline package records match. All dependency references in the demo lockfile have entries.

| Package | The Veil | Ship Asset demos |
|---|---|---|
| com.unity.collab-proxy | 2.14.0 | Absent |
| com.unity.sdk.linux-arm64 | 1.1.0 | 1.1.1 |
| com.unity.sdk.linux-x86_64 | 1.1.0 | 1.1.1 |
| com.unity.toolchain.linux-x86_64-linux | 1.1.0 | 1.1.1 |
| com.unity.sysroot.base (transitive) | 1.1.0 | 1.1.1 |

These are the complete lockfile record differences. The matching XR and rendering versions reduce one source of migration uncertainty. Scene setup, project settings, custom scripts and asset references still require an audit and an actual import test. Next collect the main project's Assets folder structure to identify existing content before choosing import destinations.
