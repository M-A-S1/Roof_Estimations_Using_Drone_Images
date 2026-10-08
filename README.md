# Roof Measurements from Drone Images

**Drone photos in → roof measurement report out.** Facet areas, pitch, and ridge, hip, valley, eave, rake, flashing and step flashing lengths, generated automatically from a 3D point cloud.

<p align="center">
  <img src="images/real_pipeline.gif" width="560" alt="Animation on a real building: LiDAR point cloud, ground removed, roof facets detected, edges measured">
</p>

<p align="center">
  <img src="images/real_pipeline.png" alt="Pipeline stages on a real building: LiDAR point cloud with trees, ground removed, roof facets detected, edges measured">
</p>

<p align="center"><sub>Real apartment building in Eden Prairie, Minnesota, measured from public USGS LiDAR. Trees and neighbouring ground are in the raw data; the roof is extracted and measured automatically in under 3 seconds.</sub></p>

## Who this is for

- **Roofing contractors:** quote jobs from a 10-minute drone flight instead of climbing the roof with a tape measure.
- **Solar installers:** get the pitch, facing direction and usable area of every roof plane for panel layout.
- **Insurance and claims inspectors:** consistent, documented roof measurements for every claim.
- **Drone service companies:** add roof reports to your flights without building the processing in-house.

## What you get

Every report contains:

| | |
|---|---|
| **Totals** | Sloped roof area (m², ft², roofing squares), plan area, predominant pitch |
| **Per facet** | Area, pitch (x/12 and degrees), facing direction (N, SE, …), fit quality |
| **Edges** | Ridge, hip, valley, eave, rake, flashing and step flashing lengths in metres and feet |
| **Files** | Two-page PDF report, JSON and CSV for your estimating software, colour-coded 3D point cloud |

### Sample report: real building

<p>
  <img src="images/real_report-1.png" width="49%" alt="Report page 1 for a real building: totals, plan diagram with labelled facets, edge lengths">
  <img src="images/real_report-2.png" width="49%" alt="Report page 2 for a real building: facet table and 3D view">
</p>

Full reports:

| Roof | Source | Report | Data |
|---|---|---|---|
| **Apartment building, Eden Prairie, MN** | **Real: USGS LiDAR** | [PDF](examples/real-lidar-eden-prairie/roof_report.pdf) | [JSON](examples/real-lidar-eden-prairie/roof.json) · [CSV](examples/real-lidar-eden-prairie/facets.csv) |
| Cross-gable (with valleys) | Simulated | [PDF](examples/cross-gable/roof_report.pdf) | [JSON](examples/cross-gable/roof.json) · [CSV](examples/cross-gable/facets.csv) |
| Hip | Simulated | [PDF](examples/hip/roof_report.pdf) | [JSON](examples/hip/roof.json) · [CSV](examples/hip/facets.csv) |
| Gable | Simulated | [PDF](examples/gable/roof_report.pdf) | [JSON](examples/gable/roof.json) · [CSV](examples/gable/facets.csv) |
| Stepped gable (step flashing) | Simulated | [PDF](examples/step/roof_report.pdf) | [JSON](examples/step/roof.json) · [CSV](examples/step/facets.csv) |
| Gable + porch (flashing) | Simulated | [PDF](examples/porch/roof_report.pdf) | [JSON](examples/porch/roof.json) · [CSV](examples/porch/facets.csv) |

### Plan diagrams

<p>
  <img src="images/real_plan.png" width="32%" alt="Real apartment building plan with step flashing between roof sections and flashing at porch roofs">
  <img src="images/step_plan.png" width="32%" alt="Stepped gable roof plan: step flashing where the lower roof meets the taller section">
  <img src="images/porch_plan.png" width="32%" alt="Gable house with a porch roof: flashing where the porch meets the wall">
</p>
<p>
  <img src="images/cross-gable_plan.png" width="32%" alt="Cross-gable roof plan with valleys">
  <img src="images/hip_plan.png" width="32%" alt="Hip roof plan">
  <img src="images/gable_plan.png" width="32%" alt="Gable roof plan">
</p>

Edge colours: 🟥 ridge · 🟧 hip · 🟦 valley · 🟩 eave · 🟪 rake · ⬛ flashing · 🟫 step flashing

| Edge | Meaning |
|---|---|
| Ridge, hip, valley | Where two roof facets meet (level peak, sloped peak, sloped trough) |
| Eave | Level roof edge with nothing above it (gutter line) |
| Rake | Sloped roof edge with nothing above it (gable end) |
| Flashing | Level roof edge against a wall rising above it (e.g. a porch roof meeting the house) |
| Step flashing | Sloped roof edge against a wall rising above it (e.g. a lower roof section meeting a taller one) |

## Accuracy

### Real building

The Eden Prairie building above was measured from public USGS 3DEP LiDAR (2022, about 85 points/m²):

| Check | Result |
|---|---|
| Roof area | 464 m² (49.95 squares), 10 facets |
| Pitch | All six main slopes at 4.0/12 (18.3–18.4°) |
| Plane fit | 1.1–1.5 cm RMS on every main facet |
| Plan area vs. mapped footprint | 418 m² vs. 410 m² in OpenStreetMap (+2 %, consistent with eave overhang) |
| Edges | Ridge 26.6 m · eave 92.4 m · rake 70.5 m · step flashing 30.5 m between the stepped sections · flashing 7.2 m at the porch roofs |
| Repeatability | An identical building on the same street: ridge 26.84 m vs. 26.61 m, main slopes 116 m² vs. 114 m² |

No tape measurements were available for this building, so these are consistency checks, not ground-truth error.

### Simulated roofs with known answers

<p align="center">
  <img src="images/pipeline.png" alt="Pipeline stages on a simulated cross-gable house: point cloud, building isolated, roof facets detected, edges measured">
</p>

Tested on simulated drone scans of houses with exactly known dimensions. Each scene includes walls, sloped ground, a tree beside the house, sensor noise, and real-world (UTM) coordinates.

| Roof | Pitch | Area error | Ridge | Hip | Valley | Eave | Rake | Flashing | Step flashing |
|---|---|---|---|---|---|---|---|---|---|
| Gable | 6/12 | **−0.0 %** | 13.78 / 14.00 | – | – | 27.5 / 28.0 | 20.0 / 20.1 | – | – |
| Hip | 6/12 | **+1.0 %** | 5.01 / 5.00 | 26.3 / 27.0 | – | 45.6 / 46.0 | – | – | – |
| Cross-gable | 8/12 | **+0.8 %** | 22.3 / 23.0 | – | 8.80 / 8.44 | 34.4 / 35.2 | 27.9 / 28.1 | – | – |
| Shed | 3/12 | **−1.0 %** | – | – | – | 27.6 / 28.0 | 18.3 / 18.6 | – | – |
| Hip, 3 cm noise | 9/12 | **+2.8 %** | 5.05 / 5.00 | 28.0 / 28.8 | – | 45.7 / 46.0 | – | – | – |
| Stepped gable | 6/12 | **−0.7 %** | 15.7 / 16.0 | – | – | 31.2 / 32.0 | 30.3 / 30.2 | – | 9.72 / 10.06 |
| Gable + porch | 6/12 + 3/12 | **+0.5 %** | 13.7 / 14.0 | – | – | 33.8 / 34.3 | 25.1 / 25.3 | 6.00 / 6.30 | – |

Lengths are measured / true, in metres.

- Facet count was correct on every roof.
- Pitch was within 0.2° on every facet.
- Each roof takes 2–3 seconds to process.

Results from real drone flights with tape-measured ground truth will be added here.

## How it works

<img src="images/hip_3d.png" width="40%" align="right" alt="Hip roof: detected facets and measured edges in 3D">

1. **3D reconstruction.** Drone photos with GPS become a georeferenced point cloud in real-world metres.
2. **Building isolation.** Ground, trees and neighbouring structures are separated from the target building.
3. **Roof plane detection.** Every roof face is found as a plane. Walls and clutter are rejected.
4. **True sloped measurement.** Each facet is measured in its own plane, so areas are the real surface that shingles cover, not the footprint.
5. **Edge classification.** Ridges, hips and valleys are computed from exact plane intersections. Outer edges are checked for a wall or higher roof rising beside them: level edges become eaves or flashing, sloped edges become rakes or step flashing.

<br clear="right">

### Inputs accepted

- **Drone photos** with GPS. A nadir grid plus one oblique orbit gives the best results. Any consumer drone, such as a DJI Mini or Mavic, is fine.
- **Existing point clouds** from Pix4D, Metashape, DroneDeploy, OpenDroneMap or LiDAR (`.las`, `.laz`, `.ply`, `.xyz`).

## About

Built by **Muhammad Ali**, a computer vision engineer with 7+ years of experience producing 3D models and ground truth from drone imagery for roofing, solar and telecom inspection platforms. MSc Electrical Engineering (AI & Autonomous Systems); Researcher in robotic perception.

I also build custom pipelines: photogrammetry and point-cloud processing, digital twins, and detection and segmentation models deployed to the cloud or edge devices.

## Get a free report

Send me drone photos, or a point cloud, of one roof and I'll send back a measurement report free of charge.

- **Email:** muhammad.ali.sid25@gmail.com
- **GitHub:** [@M-A-S1](https://github.com/M-A-S1)

---

<sub>The processing software is proprietary and is not included in this repository. © 2026 Muhammad Ali. All rights reserved.</sub>
