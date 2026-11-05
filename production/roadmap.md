# Next session and milestones
**Current task · Establish a reproducible Unity baseline.** In Unity Hub locate the project containing the working ship. Open its project folder and identify the directory containing Assets, Packages and ProjectSettings. Record the exact editor revision from ProjectSettings/ProjectVersion.txt and the scene/prefab paths. A ship demo and the intended game project may be different directories. Record both if relevant.

The first task is complete when another person can identify the same editor, project, source generator and test ship. Record the baseline commit or package version and any missing input. Keep a copy of the working project before experimenting.

| Order | Deliverable | Evidence required to advance |
|---|---|---|
| 0 | Reproducible import | Fresh project copy opens in the recorded editor. Ship compiles. Rebuilding preserves a deliberately edited Wing_P target box after reopening the prefab. |
| 1 | Stable ship and part IDs | Rebuilding or changing LOD keeps the selected ship instance and part ID. Two copies of a ship can take independent damage. |
| 2 | One playable encounter | Two small craft occupy legal regions. A Fighter selects one Light Cruiser subsystem and resolves an attack. Check independent hull/part HP and reload state. Repeat selection on Quest 3. |
| 3 | Phase and tablet loop | All three tablets remain inspectable. Illegal actions are rejected. All-ready or deadline closes new attacks, finishes accepted attacks, then rotates factions together. |
| 4 | Input portability | Complete the slice with hands on Quest 3. Additional gaze or controller paths are optional and require their own device evidence. |
| 5 | Fleet-scale playtest | Record frame timings with roughly 24 ships, three tablets and near/far zoom. Measure on the headset. Record device, scene, refresh target and build revision. |

**Deferred decisions:** quarter-region geometry for all pairings, exceptional card timing, reinstatement effects on allied orders, economy and victory balance. Bring one to Arlen only when the next playable test depends on it.
