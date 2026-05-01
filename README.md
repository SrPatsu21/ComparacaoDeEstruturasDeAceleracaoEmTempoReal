# ComparacaoDeEstruturasDeAceleracaoEmTempoReal

## compile

```shell
docker run --rm -v $(pwd):/data -w /data texlive/texlive bash -c "pdflatex main.tex; bibtex main; makeglossaries main; pdflatex main.tex; pdflatex main.tex"
```


## o q eu vou fazer
Rasterização (G-buffer pass)
ReSTIR DI (Direct Illumination)
Soft Shadows (Area Lights)
Reflexões — GGX Importance Sampling

## Hybrid Renderer Pipeline

Rasterization + ReSTIR DI + GGX Importance Sampling

## Overview

Hybrid real-time rendering pipeline combining:

- Rasterization for primary visibility
- Ray tracing for lighting and reflections
- Microfacet-based stochastic sampling (GGX)
- Temporal and spatial reuse for performance

---

## Pipeline Stages

### 1. Rasterization (G-buffer pass)

Generates base scene data.

Outputs (G-buffer):
- Position (world/view space)
- Normal
- Albedo
- Material parameters:
  - Roughness
  - Metalness
- Depth

Purpose:
- Resolve primary visibility efficiently
- Provide input data for ray tracing passes

---

### 2. Direct Illumination — ReSTIR DI

Uses ReSTIR (Direct Illumination) for efficient lighting.

Features:
- Light sampling across many light sources
- Reservoir sampling
- Spatial reuse (neighboring pixels)
- Temporal reuse (previous frames)

Result:
- Stable and efficient direct lighting
- Scales well with many lights

---

### 3. Soft Shadows (Area Lights)

Handled within ReSTIR DI.

Method:
- Sample points on area lights
- Trace shadow rays
- Evaluate partial visibility (penumbra)

Result:
- Physically plausible soft shadows
- Avoids high sample count cost

---

### 4. Reflections — GGX Importance Sampling

Based on microfacet BRDF.

Method:
- Importance sampling using GGX distribution
- Stochastic reflection ray generation

Configurations:
- 1 sample per pixel + temporal accumulation
- Multiple samples for higher quality

Supports:
- Rough surfaces
- Glossy reflections
- Metallic materials

---

### 5. Temporal Reuse

Reuses data across frames.

Applications:
- ReSTIR reservoirs
- GGX reflections
- Lighting accumulation

Benefits:
- Reduces noise
- Improves performance

---

### 6. Denoising

Required for visual stability.

Types:
- Spatial filtering
- Temporal filtering

Applied to:
- Shadows
- Reflections
- Lighting

---

## Pipeline Structure

[ Rasterization ]
    ↓
G-buffer

    ↓

[ ReSTIR DI ]
→ Direct lighting
→ Soft shadows

    ↓

[ Ray Tracing - GGX Sampling ]
→ Rough reflections
→ Stochastic sampling

    ↓

[ Temporal Reuse ]
    ↓

[ Denoising ]
    ↓

[ Final Frame ]

---

## Technical Characteristics

- High performance (raster-based primary pass)
- Real-time capable
- Scales to many light sources
- Physically plausible reflections
- Efficient soft shadows

---

## Suggested Name

Deferred Hybrid Renderer with ReSTIR DI + GGX Specular Sampling

---

## Possible Extensions

- ReSTIR GI (global illumination)
- Progressive path tracing
- Screen-space fallbacks (SSR, SSAO)
- AI upscaling