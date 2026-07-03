# Neocron Classic — ReShade Quality (AO + SSR, DX11)

Tier 0 of the graphics-remaster plan. Adds **depth-based ambient occlusion**
(qUINT MXAO) and **screen-space reflections** (qUINT SSR) to the DirectX 11
Neocron Classic client, on top of the base
[ReShade addon](https://github.com/igwtech/neocron-classic-reshade).

It requires `neocron-classic-reshade` (the launcher installs that first) — this
addon only adds the depth-dependent shaders and a **Quality** preset that layers
AO + SSR under the base clarity/sharpen pass.

## What it does

- **MXAO** — contact-shadow ambient occlusion, grounding objects and adding depth
  in interiors. Derives surface normals from the depth buffer (no normal maps
  needed — those come later, in the RenoDX tier).
- **SSR** — subtle screen-space reflections on floors and shiny surfaces.
- Keeps the base preset's Clarity / Colourfulness / Adaptive Sharpen / Curves.

Both effects read the game's **depth buffer**, which the DX11 engine already
renders (`sceneDepth`). This addon is also the plan's **probe**: if the depth
buffer is accessible under your Proton/DXVK setup, AO/SSR work here — and the
same access unblocks the deeper RenoDX PBR tiers.

## Install

Launcher: **Play ▸ Addons ▸ Browse ▸ ReShade Quality** (it pulls in the base
ReShade addon automatically). Needs `7z` on PATH, like the base addon.

## If AO looks wrong — verify depth (important)

Depth-buffer orientation isn't yet confirmed for this engine under Proton/DXVK,
so this is a one-time check:

1. In-game, press **Home** to open the ReShade overlay.
2. Enable the **DisplayDepth** effect (from the base shader library).
3. You want a clean gradient — **near = dark, far = light**, not flat white,
   not inverted, not vertically mirrored.
4. If it's wrong, edit `ReShade.ini` next to `Neocron.exe` (or use the overlay's
   *Edit global preprocessor definitions*) and flip:
   - `RESHADE_DEPTH_INPUT_IS_REVERSED` (near/far swapped)
   - `RESHADE_DEPTH_INPUT_IS_UPSIDE_DOWN` (mirrored vertically)
5. Reload (Ctrl+Shift+... / the overlay's reload). MXAO should now shade correctly.

If the depth preview is **flat/empty** even after toggling, the depth buffer
isn't reaching ReShade under your runtime — try **Enable DXVK off** in the
launcher (WineD3D), and report back so we can adjust the plan's Tier 1–2 (shader
injection) approach.

## Tuning

Open the overlay (Home), pick the **Neocron Classic — Quality** preset, and
adjust `MXAO_SSAO_AMOUNT` / `MXAO_SAMPLE_RADIUS` / `SSR_REFLECTION_INTENSITY` to
taste. Changes save back into `NeocronClassic-Quality.ini`.

## Credits & licensing

- **qUINT** MXAO/SSR © Pascal Gilcher (martymcmodding), fetched from the official
  repo at install time. This repo ships only the manifest, preset, and ReShade
  config text.
