# SpatialBlend

**Relational Reconstruction for Cross-Space Presence in Heterogeneous Spaces**

*ACM UIST 2026 (The 39th Annual ACM Symposium on User Interface Software and Technology), November 2–5, 2026, Detroit, MI, USA*

[Paper](https://doi.org/10.1145/3830398.3830690) · [Video](https://www.youtube.com/watch?v=3TWBkZOWY_4) · [Code](#code) · [Cite](#citation)

![Freeform Collaboration in heterogeneous spaces: User A in an office and User B in a living room interact through avatars across three moments in time — watering the plant, going to the chessboard, and playing chess together. The top row shows Space A from an external camera; the bottom row shows the same activity as reconstructed in User B's living room.](docs/assets/teaser.jpg)

> **Freeform Collaboration in heterogeneous spaces.** User A (office) and User B (living room) interact through avatars while remaining in different rooms. SpatialBlend preserves interaction-relevant structure across the two spaces, keeping Avatar A interpretable in User B's room despite differences in geometry, topology, and affordances.

## Video

[![SpatialBlend demo video: two users in an office and a living room walk, converse, and interact with shared virtual and physical objects while each remote avatar is continuously adapted to the local room.](https://img.youtube.com/vi/3TWBkZOWY_4/maxresdefault.jpg)](https://www.youtube.com/watch?v=3TWBkZOWY_4)

## Overview

*Cross-space presence* lets people in different physical rooms interact as if they shared one space: each person stays in their own room, and their behavior is realized in the other's room.

The central obstacle is **spatial heterogeneity**. Rooms differ in layout, geometry, topology, and affordances, so replaying motion directly, or forcing a rigid shared layout, makes behavior invalid, unreachable, or hard to interpret. An avatar that follows the source user's path may end up inside a wall; an avatar restricted to pre-authored zones vanishes as soon as its user steps outside one.

**SpatialBlend** is a runtime for cross-space presence built on **relational reconstruction**. Instead of copying coordinates between rooms, it continuously describes *how* the user is interacting with nearby people and objects, then reconstructs the avatar's behavior in the viewer's room under that room's geometry and affordances. It does not replace the mixed-reality platform or the avatar backend; it is the conversion layer between them.

## How it works

Each space runs its own SpatialBlend instance. An instance observes its local user, sends a compact relation-centric description of their behavior, and reconstructs incoming remote behavior under local constraints, in three steps:

1. **Shared interaction hierarchy.** A tree of rooms, regions, objects, and affordances that every space agrees on. Each node has a logical identity shared across spaces and a spatial definition in local geometry: the same board may hang on opposite walls in the two rooms.
2. **Relation extractor.** The source user's behavior is encoded as relations over that hierarchy (whom they face, what they approach, how their body is arranged against an affordance) rather than as coordinates.
3. **Heterogeneity-aware realizer.** The relations are realized as a locally feasible avatar state. Because the avatar must be somewhere at every moment, before the user's intent is clear, reconstruction is an online decision problem: the realizer keeps the avatar where the active relations are jointly best preserved, and the destination emerges as one relation becomes dominant.

The realizer emits avatar-control targets rather than finished animation; a dispatch layer routes them to existing motion backends for navigation, facing, root placement, and retargeting.

## Freeform Collaboration prototype

*Freeform Collaboration* is the SpatialBlend prototype: users walk, converse, and interact with shared virtual and physical objects while each remote avatar is continuously adapted to the local room. It supports three kinds of spatial elements:

- **Pre-defined physical objects:** large furniture registered through scene anchors.
- **Arbitrary physical objects:** any real object (a potted plant, a teddy bear, a chair) added at runtime from a single photo.
- **Virtual objects:** networked interactables such as a chalkboard or a chessboard, placed freely in each user's room.

It is implemented in Unity on Meta Quest 3, with a native runtime core and a Unity plugin.

## Results

We deployed SpatialBlend across an office and a living room that differ in connectivity and topology, and ran a within-subjects study in which each dyad used both SpatialBlend and a zone-based baseline that showed the remote user only inside fixed interaction zones.

![Paired violin plots comparing the zone-based baseline against SpatialBlend on a Likert scale. Left: overall social presence and its five subscales, where co-presence shows the largest gain. Right: overall perceived adaptation quality and its seven items; all but layout awareness improve under SpatialBlend.](docs/assets/eval-deployment-study.png)

> **Deployment study outcomes.** (Left) Social presence: overall and per subscale. (Right) Perceived adaptation quality: overall and per item. Violins show per-condition score distributions; the shaded *Overall* columns also show per-participant paired shifts.

- **Social presence improved**, driven mainly by co-presence: partners felt present throughout the session, not only near shared objects.
- **Perceived adaptation quality improved**, mainly in continuity, trackability, gaze stability, and spatial coherence.
- Participants described the baseline's avatar as "appearing and disappearing on repeat". With continuity sustained, they treated the avatar as a socially situated body: walking side by side, throwing a chess piece at their partner.

The runtime cost stays a small fraction of the frame budget across the geometric and semantic complexity of real indoor spaces, leaving headroom for avatar rendering on Quest 3.

## Code

The code is released in three repositories under the MIT License (a few files keep their own terms; see each repository's NOTICE):

| Repository | Contents | Prebuilt release |
|---|---|---|
| [snuhcs/spatialblend-unity](https://github.com/snuhcs/spatialblend-unity) | The Unity app for Meta Quest 3 (Freeform Collaboration) and the SpatialBlend Unity plugin. **Start here**: its README covers setup and the Quest build. | None: build the app yourself with your own Photon App IDs |
| [snuhcs/spatialblend-core](https://github.com/snuhcs/spatialblend-core) | The native runtime (`libspatialblend`, C++ with a C ABI and Python bindings): the interaction hierarchy, relation extraction, and the realizer. | [v1.0.0](https://github.com/snuhcs/spatialblend-core/releases/tag/v1.0.0): Android arm64, macOS arm64, Windows x64, Linux x64 |
| [snuhcs/spatialblend-reconstruction](https://github.com/snuhcs/spatialblend-reconstruction) | Object scanning: the SAM 3 / SAM 3D Objects gRPC scan server and the `libspatialblend_session` client library. | [v1.0.0](https://github.com/snuhcs/spatialblend-reconstruction/releases/tag/v1.0.0): Android arm64, macOS arm64, Windows x64, Linux x64 |

The Unity app's setup script downloads both native libraries from these releases and verifies their checksums.

## Citation

**SpatialBlend: Relational Reconstruction for Cross-Space Presence in Heterogeneous Spaces**
Jingyu Lee, Dongho Han, Jinmyeong Kim, and Youngki Lee (Seoul National University)
*UIST '26: Proceedings of the 39th Annual ACM Symposium on User Interface Software and Technology*
DOI: [10.1145/3830398.3830690](https://doi.org/10.1145/3830398.3830690)

```bibtex
@inproceedings{lee2026spatialblend,
  title     = {SpatialBlend: Relational Reconstruction for Cross-Space Presence in Heterogeneous Spaces},
  author    = {Lee, Jingyu and Han, Dongho and Kim, Jinmyeong and Lee, Youngki},
  booktitle = {Proceedings of the 39th Annual ACM Symposium on User Interface Software and Technology},
  series    = {UIST '26},
  year      = {2026},
  location  = {Detroit, MI, USA},
  publisher = {Association for Computing Machinery},
  doi       = {10.1145/3830398.3830690},
  isbn      = {979-8-4007-2856-3}
}
```

GitHub's "Cite this repository" uses [CITATION.cff](CITATION.cff).

## Related publication

**Probing Ambient Co-Presence as Social Infrastructure: First Encounters with Connected Rooms**
Jingyu Lee and Youngki Lee (Seoul National University)
*4th International Workshop on Social Infrastructure Systems (SocialSys '26), at ACM BuildSys, June 22, 2026, Banff, AB, Canada · ACM Sustainability Week Companion '26*
DOI: [10.1145/3765611.3815495](https://doi.org/10.1145/3765611.3815495)

Drawing on the same Freeform Collaboration deployment, this workshop paper reads first encounters with connected rooms as encounters with *social infrastructure* rather than with a communication tool. It asks what follows when presence is continuous instead of summoned: how persistent remote presence becomes part of everyday routines, under what conditions of access it feels appropriate and for whom, and what seams and asymmetries spatial adaptation introduces into a home.

## Contact

Jingyu Lee (<jingyu.lee@hcs.snu.ac.kr>), Human-Centered Computer Systems Lab, Seoul National University. [Author page](https://jingyulee.com)

## Contributors

- Jingyu Lee (Seoul National University)
- Dongho Han (Seoul National University)
- Jinmyeong Kim (Seoul National University)
