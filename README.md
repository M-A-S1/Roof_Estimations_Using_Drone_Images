# Roof measurements from drone imagery

**Drone photos in → roof measurement report out.**

I build computer-vision pipelines that turn drone imagery into accurate roof measurements: every roof facet with its area, pitch and facing direction, plus ridge, hip, valley, eave and rake lengths. It's the same data roofers, solar installers and insurance adjusters buy as "roof measurement reports".

<p>
  <img src="images/cross-gable_plan.png" width="48%" alt="Plan diagram of a cross-gable roof with labelled facets and colour-coded edges">
  <img src="images/hip_plan.png" width="48%" alt="Plan diagram of a hip roof with labelled facets and colour-coded edges">
</p>

## What you get

For each roof:

- **Totals:** sloped roof area in m², ft² and roofing squares; predominant pitch
- **Per facet:** area, pitch (x/12 and degrees), facing direction (N, SE, …)
- **Edges:** ridge, hip, valley, eave and rake lengths in metres and feet
- **Files:** a two-page PDF report, plus JSON and CSV for your own software

Sample reports:

| Roof | Report | Data |
|---|---|---|
| Cross-gable (with valleys) | [PDF](examples/cross-gable/roof_report.pdf) | [JSON](examples/cross-gable/roof.json) · [CSV](examples/cross-gable/facets.csv) |
| Hip | [PDF](examples/hip/roof_report.pdf) | [JSON](examples/hip/roof.json) · [CSV](examples/hip/facets.csv) |
| Gable | [PDF](examples/gable/roof_report.pdf) | [JSON](examples/gable/roof.json) · [CSV](examples/gable/facets.csv) |

## Accuracy

Measured on simulated drone scans of houses with exactly known dimensions. Each scene includes walls, sloped ground, a tree beside the house and sensor noise.

| Roof | Pitch | Area error | Ridge | Hip | Valley | Eave | Rake |
|---|---|---|---|---|---|---|---|
| Gable | 6/12 | −0.0 % | 13.78 / 14.00 m | – | – | 27.5 / 28.0 | 20.0 / 20.1 |
| Hip | 6/12 | +1.0 % | 5.01 / 5.00 | 26.3 / 27.0 | – | 45.6 / 46.0 | – |
| Cross-gable | 8/12 | +0.8 % | 22.3 / 23.0 | – | 8.80 / 8.44 | 34.4 / 35.2 | 27.9 / 28.1 |
| Shed | 3/12 | −1.0 % | – | – | – | 13.8 / 14.0 | 18.3 / 18.6 |
| Hip, 3 cm noise | 9/12 | +2.8 % | 5.05 / 5.00 | 28.0 / 28.8 | – | 45.7 / 46.0 | – |

Lengths are measured / true, in metres. Facet count was correct in every case and pitch was within 0.2°.

These are simulated results. Validation on real flights is in progress and will be added here.

## How it works

<img src="images/hip_3d.png" width="45%" align="right" alt="3D view of a reconstructed hip roof">

1. **3D reconstruction:** drone photos with GPS become a georeferenced point cloud in real-world metres.
2. **Building extraction:** ground, trees and neighbouring structures are separated from the target building.
3. **Roof plane detection:** each roof face is found as a plane, and walls and clutter are rejected.
4. **Measurement:** facets are measured in their own plane, so areas are true sloped areas, not footprint.
5. **Edge classification:** where facets meet, ridges, hips and valleys are computed from exact plane intersections. Outer edges become eaves and rakes.

Inputs accepted: drone photos (nadir grid plus an oblique orbit works best), or an existing point cloud from Pix4D, Metashape, DroneDeploy, OpenDroneMap or LiDAR (`.las`, `.laz`, `.ply`, `.xyz`).

<br clear="right">

## About me

Computer vision engineer with 7+ years producing 3D models and ground truth from drone imagery for roofing, solar and telecom inspection platforms. MSc Electrical Engineering (AI & Autonomous Systems). PhD researcher in robotic perception.

## Try it on your roof

Send me drone photos (or a point cloud) of one roof and I'll send back a free measurement report.

- Email: YOUR_EMAIL
- LinkedIn: YOUR_LINKEDIN_URL

The processing software is proprietary and is not included in this repository. © 2026 Muhammad Ali. All rights reserved.
