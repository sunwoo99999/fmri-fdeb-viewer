# PRD: SIGGRAPH 2026 Student Research Competition (SRC) Project

---

## 1. Project Overview

- **Goal:** Develop a 2-page abstract and poster graphics deliverable for submission to the SIGGRAPH ACM Student Research Competition (SRC).
- **Background:** Existing clinical studies analyze aging and disease-related brain networks using Multiscale Entropy (MSE) and ANCOVA derived from fMRI BOLD signals. However, rendering these networks in 3D space produces severe visual clutter due to the massive overlap of edges.
- **Core Contribution:** Beyond simple medical data mapping, this project proposes a pipeline that compresses and optimizes large-scale brain functional connectivity data using computer graphics algorithms (FDEB, spatiotemporal wavefronts, etc.), enabling intuitive, low-latency 3D visualization.

---

## 2. Candidate Algorithms for Feasibility Testing (Step 1)

Three candidate techniques to be individually prototyped (vibe coding) to evaluate feasibility in terms of FPS performance and visual quality.

### A. Dynamic Network Visualization via FDEB (Force-Directed Edge Bundling)

- **Overview:** Applies a spring-electrical attraction model to tangled inter-node connections, bundling them into neural-fiber-like bundles to eliminate visual clutter.
- **Feature:** Injects HRF fitting curve equations into GLSL shaders so that each edge's opacity and brightness animate over time as a wave-like pulsing effect.
- **Requirement:** Data must not be aggregated into 4 coarse ROIs (V1–V4); instead, voxel-wise separation is required so that directional edges can be generated based on time-to-peak ($t_{peak}$) differences.

### B. Spatiotemporal Hemodynamic Wavefront

- **Overview:** Maps hemodynamic arrival time ($t_{peak}$) along the visual hierarchy (V1 $\rightarrow$ V4) onto the 3D brain mesh surface as iso-contours or ripple-like patterns.
- **Feature:** Best fit with the current pipeline and dataset (highest compatibility); relatively low rendering implementation complexity — **highest probability of success**.

### C. 7D Latent Parameter Flow

- **Overview:** Visualizes the 7-dimensional mathematical parameter set obtained from HRF fitting as particle flow trajectories in 3D space, using dimensionality reduction algorithms such as UMAP or t-SNE.
- **Feature:** A high-efficiency graphics strategy that maximizes the dimensionality reduction capabilities demonstrated in prior HAR data publications.

---

## 3. System Architecture (Backend–Frontend Separation Strategy)

To overcome browser rendering limitations and achieve 60 frames per second, **heavy computation and lightweight rendering are strictly decoupled**.

| Layer        | Technology Stack                      | Role                                                                                                         |
| ------------ | ------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Backend**  | Python (`scipy`, `numpy`, `networkx`) | Performs voxel-wise HRF fitting (multi-start optimization) and FDEB physics simulation offline               |
| **Bridge**   | JSON                                  | Exports only pre-computed spline control point coordinates and per-timestep amplitude/opacity parameters     |
| **Frontend** | React Three Fiber / WebGL             | Generates smooth curves via `CatmullRomCurve3`; renders real-time pulsing animations via custom GLSL shaders |

---

## 4. Key Use Cases (Why It Matters)

- **Medical and Neuroscience Researchers:** Instantly explore and analyze disease-related structural changes in cerebrovascular networks without installing heavy software — directly in a web browser, with no visual clutter.
- **Technical Relevance (SIGGRAPH Appeal):** Dramatically improves accessibility between data analysis and visualization by compressing massive 4D fMRI data through graphics optimization algorithms.

---

## 5. Success Metrics and Constraints

| Criterion          | Standard                                                                                                                            |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------- |
| **Visual Quality** | Avoid straight-line connections; ensure anatomical organicity through mathematical curve interpolation or anatomical vessel routing |
| **Performance**    | Maintain WebGL-based 60 fps even when rendering tens of thousands of edges and particles                                            |
