# Eidoverse

**An agent framework for creating animated videos in [three.js](https://threejs.org/) with [Deno](https://deno.com/).**

Eidoverse is the render pipeline and authoring layer that lets an AI agent
direct a short animated film end to end — source the world, stage it, animate
the characters, score it, render it, and ship a finished `.mp4` — without a
human in the loop for any individual frame.

It is **GPU-only and NodeMaterial-only**: Deno + WebGPU + three.js + TSL
(Three Shading Language), with Rapier physics, VRM characters, and a library
of GPU showpiece effects. Agents write a scene as a small script against a set
of installed globals; the renderer turns it into video.

---

## What it is

A frame generator on a timer is not the goal. Eidoverse gives an agent a
**world-building API** and a **director's workflow**:

1. **Source real assets before geometry** — fetch 3D models, PBR textures, and
   HDRI environments from open libraries, then read the generated preview of
   every mesh before placing it.
2. **Stage with intent** — placement helpers (`placeOn`, `placeAgainst`,
   `snapToGround`, `scatterOn`) seat props on surfaces and against walls
   instead of hand-guessed coordinates.
3. **Animate** — VRM characters with a locomotion controller, foot IK, default
   animation clips, facial expressions, and viseme-based lip sync.
4. **Score it** — generative music and narration tracks, mixed under the runtime.
5. **Render** — Deno + WebGPU, real-time-ish on a modern GPU.
6. **Verify** — the pipeline reads its own output back and audits for the
   failure modes that pass a naive "it rendered" check (empty voids, floating
   props, frozen frames, camera clipping, T-posed subjects).

## The stack

| Layer | Tech |
|---|---|
| Runtime | Deno |
| Renderer | three.js WebGPU renderer |
| Materials / shaders | TSL NodeMaterials (no raw GLSL, no WebGL fallbacks) |
| Physics | Rapier |
| Characters | VRM (loader, locomotion controller, IK, expressions, lip sync) |
| Procedural geometry | lofting / sweep tools, terrain heightfields |
| Effects | GPU post-processing stack + particle / fluid / cloth / morph showpieces |
| Audio | generative music + TTS narration, mixed to the render length |
| Output | H.264 `.mp4` with muxed audio |

## Design principles

- **Constraints over exhortation.** Quality is enforced by automated audits
  that demand a re-render, not by documentation asking nicely.
- **Helpers own the hard logic.** Locomotion, placement, and material setup
  live in reusable tools — scenes are thin harnesses that call into them.
- **Source, don't fabricate.** A real fetched asset beats a primitive; a real
  texture beats a flat color.
- **The bar is "looks like a real produced short,"** not "rendered without
  errors."

---

> Staging repository. Framework source not yet published here.
