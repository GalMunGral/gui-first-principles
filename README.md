# Graphical User Interfaces: First Principles

**Live demo:** https://galmungral.github.io/gui-first-principles/

## Rhetorical Design

### Purpose

Every programmer interacts daily with GUI toolkits, font renderers, and graphics
APIs, but the mechanism underneath — how geometric descriptions become a pixel
grid — is rarely exposed. This project demonstrates that the entire pipeline,
from drawing a polygon to rendering a font to applying a blur, reduces to a
small set of operations: scan-line rasterization, linear interpolation, matrix
multiplication, and convolution. None of it requires specialized machinery
beyond a straightforward algorithm.

### Strategy

**Interactive article.** Each section introduces a concept through prose and
mathematics, then immediately embeds a live canvas where the reader can
manipulate the parameters and verify the claim. The progression is incremental —
each concept builds on the previous — so by the time the reader reaches font
rendering and GPU acceleration, every piece of it is already familiar.

This is the foundational piece in a series. [vector-rendering](https://github.com/GalMunGral/vector-rendering)
continues from here, examining what introducing a GPU changes and what it
doesn't. [michelangelo](https://github.com/GalMunGral/michelangelo) builds a
component framework on top of the same primitives. [svg.c](https://github.com/GalMunGral/svg.c)
reimplements the rasterization pipeline in C as a Unix filter chain.

## Technical Overview

The article covers the following pipeline in order:

1. **Event loop** — the interplay between `pick` and `draw` that makes a GUI interactive
2. **Scan-line rasterization** — filling the interior of closed curves by finding horizontal intervals at each y
3. **Transformations** — translation, rotation, and scaling as composable 3×3 matrices in homogeneous coordinates
4. **Discretization** — converting smooth curves to polygons by sampling; elliptic arcs as a concrete example
5. **Stroking** — thick curves as a sequence of rectangles joined by ellipses
6. **Gradients** — linear interpolation of color across pixels
7. **Bézier curves** — recursive linear interpolation via De Casteljau's algorithm
8. **Fonts** — OpenType/TrueType glyphs as quadratic and cubic Bézier curves, rasterized with the same scan-line algorithm
9. **View tree** — containment hierarchy rendered by pre-order traversal; hit-detection by reverse linear search
10. **Alpha blending** — the Porter-Duff over operator for transparent layers
11. **Filtering** — Gaussian blur as convolution with a kernel
12. **Hardware acceleration** — offloading rasterization to the GPU via triangle meshes