# SpatialBlend

**Relational Reconstruction for Cross-Space Presence in Heterogeneous Spaces**

*ACM UIST 2026 — The 39th Annual ACM Symposium on User Interface Software and Technology, November 2–5, 2026, Detroit, MI, USA*

![Freeform Collaboration in heterogeneous spaces: User A in an office and User B in a living room interact through avatars across three moments in time — watering the plant, going to the chessboard, and playing chess together. The top row shows Space A from an external camera; the bottom row shows the same activity as reconstructed in User B's living room.](docs/assets/teaser.jpg)

> **Freeform Collaboration in heterogeneous spaces.** User A (office) and User B (living room) interact through avatars while remaining in different rooms. SpatialBlend preserves interaction-relevant structure across the two spaces, keeping Avatar A interpretable in User B's room despite differences in geometry, topology, and affordances.

**[Paper (ACM DL)](https://doi.org/10.1145/3830398.3830690) · [Demo video](https://www.youtube.com/watch?v=3TWBkZOWY_4)**

---

## Demo video

[![SpatialBlend demo video: two users in an office and a living room walk, converse, and interact with shared virtual and physical objects while each remote avatar is continuously adapted to the local room.](https://img.youtube.com/vi/3TWBkZOWY_4/maxresdefault.jpg)](https://www.youtube.com/watch?v=3TWBkZOWY_4)

---

## Code release

**The source code is not published yet.** We will open-source SpatialBlend in this repository at the time of the conference presentation — **early November 2026, during UIST '26 (Nov 2–5, Detroit)**.

Until then this repository holds only the project description (this README and `docs/assets/`). Watch or star the repo to be notified when the code lands.

---

## What is SpatialBlend?

*Cross-space presence* lets people in different physical rooms communicate as if they shared one space — each person stays in their own room, while their behavior is realized in the other's room. This supports low-friction, spontaneous interaction without asking anyone to leave or rearrange their everyday space.

The central obstacle is **spatial heterogeneity**. Real rooms differ in layout, geometry, topology, and affordances, so directly replaying motion — or relying on a rigid globally shared layout — makes behavior invalid, unreachable, or hard to interpret. An avatar that walks the source user's path may end up inside a wall; an avatar restricted to pre-authored zones vanishes the moment its user steps outside one.

**SpatialBlend** is a runtime system for cross-space presence built on **relational reconstruction**: instead of copying coordinates between rooms, it continuously describes *how* the user is interacting with nearby people and objects, then reconstructs avatar behavior in the viewer's room while respecting that room's geometry and affordances.

SpatialBlend does not replace the mixed-reality platform or the avatar backend. It inserts the missing conversion layer between them.

## How it works

Each participating space runs its own SpatialBlend instance. An instance observes its local user, transmits a compact relation-centric description of ongoing behavior, and reconstructs incoming remote behavior under local constraints — in three steps:

1. **Shared interaction hierarchy.** A tree of rooms, regions, objects, and affordances that every space agrees on. Each node carries a logical identity shared across spaces and a spatial definition instantiated in local geometry — the same board may hang on opposite walls in the two rooms.

2. **Relation extractor.** The source user's embodied behavior is encoded as relations over that hierarchy — whom they face, what they approach, how their body is arranged against an affordance — rather than as coordinates.

3. **Heterogeneity-aware realizer.** Those relations are realized as a locally feasible avatar state. Because the avatar must be *somewhere* at every moment, before the user's intent is clear, reconstruction is treated as an online decision problem: the realizer keeps the avatar where the most active relations are jointly preserved, and lets the destination emerge as one relation becomes dominant.

The realizer emits avatar-control targets rather than finished animation, which a dispatch layer routes to existing motion backends for navigation, facing, root placement, and animation retargeting.

## Freeform Collaboration prototype

We instantiate SpatialBlend as *Freeform Collaboration*, in which users walk, converse, and interact freely with shared virtual and physical objects while the runtime continuously adapts each remote avatar to the local room. It supports three categories of spatial elements:

- **Pre-defined physical objects** — large furniture registered through scene anchors.
- **Arbitrary physical objects** — any real object (a potted plant, a teddy bear, a chair) added at runtime through a single-photo reconstruction pipeline.
- **Virtual objects** — networked interactables such as a chalkboard or chessboard, freely placed in each user's space.

Implemented in Unity on Meta Quest 3, with a native runtime core and a Unity plugin.

## Results

We deployed SpatialBlend across a real office and a living room that differ in connectivity and topology, and ran a within-subjects study in which each dyad experienced both SpatialBlend and a zone-based baseline that rendered the remote user only inside fixed interaction zones.

![Paired violin plots comparing the zone-based baseline against SpatialBlend on a Likert scale. Left: overall social presence and its five subscales, where co-presence shows the largest gain. Right: overall perceived adaptation quality and its seven items; all but layout awareness improve under SpatialBlend.](docs/assets/eval-deployment-study.png)

> **Deployment study outcomes.** (Left) Social presence: overall and per subscale. (Right) Perceived adaptation quality: overall and per item. Violins show per-condition score distributions; the shaded *Overall* columns also show per-participant paired shifts.

- **Social presence improved**, driven mainly by co-presence — partners felt present *throughout* the session rather than only near shared objects.
- **Perceived adaptation quality improved**, with gains concentrated in continuity, trackability, gaze stability, and spatial coherence.
- Participants described the baseline's avatar as "appearing and disappearing on repeat," leaving almost no feeling of shared space; with continuity sustained, they began treating the avatar as a socially situated body — walking side by side, throwing a chess piece at their partner.

Runtime cost stays a small fraction of the frame budget across the range of geometric and semantic complexity found in real indoor spaces, leaving ample headroom for avatar rendering on Quest 3.

## Paper

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

## Related publication

**Probing Ambient Co-Presence as Social Infrastructure: First Encounters with Connected Rooms**
Jingyu Lee and Youngki Lee (Seoul National University)
*4th International Workshop on Social Infrastructure Systems (SocialSys '26)*
*ACM BuildSys workshop — June 22, 2026, Banff, AB, Canada · ACM Sustainability Week Companion '26*
DOI: [10.1145/3765611.3815495](https://doi.org/10.1145/3765611.3815495)

Ambient co-presence may involve more than making remote communication immersive — it changes how distant others become present in everyday life. Reading first encounters with connected rooms as an encounter with *social infrastructure* rather than with a communication tool, this workshop paper asks what follows when presence is continuous instead of summoned: how persistent remote presence might become embedded in everyday routines, under what conditions of access it could feel appropriate and for whom, and what seams and asymmetries spatial adaptation introduces into a home. It draws on the same Freeform Collaboration deployment.

## Contact

Jingyu Lee — <jingyu.lee@hcs.snu.ac.kr>

Human-Centered Computer Systems Lab, Seoul National University.
