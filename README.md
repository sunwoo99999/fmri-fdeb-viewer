# Brain Network Visualizer

A web-based 3D brain functional connectivity visualization pipeline.
<img width="1420" height="813" alt="resultviewer" src="https://github.com/user-attachments/assets/e67acb6f-a25f-49e6-8818-a1fdc91edd84" />


## Overview

This project addresses the visual clutter problem that arises when rendering large-scale fMRI-derived brain networks in 3D space. Rather than simply mapping medical data, it proposes a graphics-optimized pipeline that compresses high-dimensional 4D fMRI data using algorithms such as **Force-Directed Edge Bundling (FDEB)** and **spatiotemporal wavefronts**, enabling intuitive, real-time visualization at 60 fps in a web browser.

## Features

- **FDEB-based edge bundling** — bundles tangled voxel-wise connectivity edges into neural-fiber-like structures, with HRF-driven pulsing animations via custom GLSL shaders
- **Spatiotemporal hemodynamic wavefront** — maps hemodynamic arrival time ($t_{peak}$) across the visual hierarchy (V1 → V4) as ripple/iso-contour overlays on a 3D brain mesh
- **7D latent parameter flow** — visualizes HRF fitting parameters reduced via UMAP/t-SNE as particle flow trajectories in 3D space
- **Backend–Frontend decoupled architecture** — heavy computation (HRF fitting, FDEB physics) runs offline in Python; only pre-computed spline control points and per-frame parameters are passed to the browser via JSON

## Tech Stack

| Layer    | Technologies                                      |
| -------- | ------------------------------------------------- |
| Backend  | Python · `scipy` · `numpy` · `networkx`           |
| Bridge   | JSON                                              |
| Frontend | React · React Three Fiber · Three.js · WebGL/GLSL |
| Bundler  | Vite                                              |

## Project Structure

```
.
├── prd.md                  # Product Requirements Document
├── generate_fdeb.py        # Python script: offline FDEB computation
├── network_data.json       # Pre-computed network data (root-level copy)
├── BrainNetworkViewer.jsx  # Standalone viewer component (prototype)
└── brain-viewer/           # Main Vite + React application
    ├── index.html
    ├── package.json
    ├── vite.config.js
    ├── public/
    │   └── network_data.json   # Network data served to the browser
    └── src/
        ├── main.jsx
        ├── App.jsx
        └── BrainNetworkViewer.jsx  # Core 3D visualization component
```

## Getting Started

### Prerequisites

- Node.js ≥ 18
- Python ≥ 3.9 (for backend data generation)

### 1. Generate network data (offline)

```bash
python generate_fdeb.py
```

This performs FDEB physics simulation and HRF fitting, then writes the resulting spline control points and animation parameters to `network_data.json`.

### 2. Copy data to the frontend

```bash
cp network_data.json brain-viewer/public/network_data.json
```

### 3. Install dependencies and run the dev server

```bash
cd brain-viewer
npm install
npm run dev
```

Open `http://localhost:5173` in your browser.

## Performance Target

- **60 fps** in WebGL with tens of thousands of animated edges and particles
- No visual clutter — edges are bundled and rendered with anatomically informed curve interpolation

## Research Context

Brain functional connectivity is analyzed from fMRI BOLD signals using Multiscale Entropy (MSE) and ANCOVA to study aging and neurological disease. This visualization pipeline makes those findings accessible and explorable directly in the browser, without requiring specialized software installation.

## License

MIT
