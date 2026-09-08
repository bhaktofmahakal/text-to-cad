# text-to-cad ⚙️

> **Agentic CAD, Generative Hardware & 3D Manufacturing Engine**  
> Transform natural language prompts into parametric B-Rep solids, watertight 3D-printable meshes, and robot kinematics.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-3776AB.svg?style=flat-square&logo=python&logoColor=white)](skills/cad/requirements.txt)
[![TypeScript / Node](https://img.shields.io/badge/TypeScript-5.0+-3178C6.svg?style=flat-square&logo=typescript&logoColor=white)](packages/cadgen-js/package.json)
[![Tests](https://img.shields.io/github/actions/workflow/status/bhaktofmahakal/text-to-cad/test.yml?branch=main&style=flat-square&logo=githubactions&logoColor=white&label=CI)](https://github.com/bhaktofmahakal/text-to-cad/actions)
[![GitHub Stars](https://img.shields.io/github/stars/bhaktofmahakal/text-to-cad?style=flat-square&logo=github)](https://github.com/bhaktofmahakal/text-to-cad/stargazers)
[![LinkedIn](https://img.shields.io/badge/Author-Utsav_Mishra-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/utsav-mishra1/)

---

## 💡 Overview

Most generative 3D tools output "dumb" polygon meshes (hollow triangle soups) that cannot be edited in CAD software or manufactured with physical tolerances. 

`text-to-cad` is an **open-source runtime engine and agent skill library** designed for coding agents (Claude Code, OpenAI Codex) and engineers. It generates **deterministic parametric CAD scripts** that compile through an OpenCASCADE geometric kernel into exact mathematical B-Rep models (`.step`), manufacturing cut files (`.dxf`), sliced 3D-printing meshes (`.stl`, `.3mf`), and robot simulation definitions (`.urdf`, `.srdf`).

---

## 🏗️ Architecture

```
User / Agent Prompt ("ESP32 snap-fit enclosure with M2.5 standoffs and air vents")
                                    │
                                    ▼
       ┌─────────────────────────────────────────────────────────┐
       │             Agent Skills Layer (skills/cad)             │
       │   - Natural language to parametric script generation    │
       │   - Constraint planning & feature tree synthesis        │
       └────────────────────────────┬────────────────────────────┘
                                    │
                                    ▼
       ┌─────────────────────────────────────────────────────────┐
       │             cadgen Python Geometric Engine              │
       │   - OpenCASCADE (OCCT) B-Rep kernel evaluation          │
       │   - Exact solid booleans, fillets, chamfers & shells    │
       │   - Fastener catalog integration (M2/M3 inserts)        │
       └────────────────────────────┬────────────────────────────┘
                                    │
                  ┌─────────────────┴─────────────────┐
                  ▼                                   ▼
┌───────────────────────────────────┐ ┌───────────────────────────────────┐
│     Multi-Format Exporters        │ │   DfAM & Slicing Validation       │
│  - STEP / STP (Solid B-Rep)       │ │  - Watertight Manifold checks     │
│  - STL / 3MF (3D Mesh)            │ │  - Minimum wall thickness >1.2mm  │
│  - DXF (2D CNC / Laser profiles)  │ │  - SendCutSend preflight report   │
│  - URDF / SRDF (Robot Kinematics) │ │  - G-Code slicing (Bambu / FDM)   │
└─────────────────┬─────────────────┘ └─────────────────┬─────────────────┘
                  │                                     │
                  └─────────────────┬───────────────────┘
                                    ▼
       ┌─────────────────────────────────────────────────────────┐
       │          Interactive CAD Explorer (apps/viewer)         │
       │   - In-browser WebGL / Three.js 3D viewport             │
       │   - MeshBVH accelerated raycasting & orbit navigation   │
       │   - Real-time slice plane & bounding-box inspection     │
       └─────────────────────────────────────────────────────────┘
```

---

## 🧰 Core Skill Modules

`text-to-cad` provides modular skills that equip AI coding agents with deep domain capabilities:

| Module | Purpose | Artifacts Produced |
| :--- | :--- | :--- |
| **`skills/cad`** | Core parametric CAD synthesis via `cadgen` | `.step`, `.stl`, `.3mf`, `.glb` |
| **`skills/cad-viewer`** | Headless launch and inspection in local WebGL CAD Explorer | Interactive 3D review |
| **`skills/step-parts`** | Off-the-shelf mechanical hardware catalog (screws, nuts, standoffs, bearings, motors) | Verified STEP models |
| **`skills/dfam-check`** | Design for Additive Manufacturing (wall thickness, overhangs, support volume) | Printability report |
| **`skills/dxf`** | 2D technical drawings, gasket profiles, and laser cut layouts | `.dxf` vector files |
| **`skills/sendcutsend`** | Manufacturing preflight audit against SendCutSend machining/laser rules | Pre-order checklist |
| **`skills/gcode`** | Mesh slicing into printer-profiled FDM machine instructions | `.gcode` |
| **`skills/bambu-labs`** | LAN discovery, automated upload, and print job controls for Bambu Lab printers | Direct print execution |
| **`skills/urdf`** | Robot kinematic descriptions (links, joints, limits, visual/collision meshes) | `.urdf` |
| **`skills/srdf`** | MoveIt2 motion planning configurations, collision pairs, and semantic poses | `.srdf` |

---

## ⚡ Quick Start

### 1. Install via Skills CLI (Recommended)

Give your terminal coding agents instant CAD generation capabilities:

```bash
npx skills add bhaktofmahakal/text-to-cad
```

### 2. Provider-Native Plugins

For **Claude Code**:
```bash
claude plugin marketplace add bhaktofmahakal/text-to-cad
claude plugin install cad@text-to-cad
```

For **OpenAI Codex**:
```bash
codex plugin marketplace add bhaktofmahakal/text-to-cad
codex plugin add cad@text-to-cad
```

---

## 🛠️ Local Development & WebGL Viewer

### Prerequisites
- Python 3.11+
- Node.js 20+

### 1. Clone & Set Up Python Engine
```bash
git clone https://github.com/bhaktofmahakal/text-to-cad.git
cd text-to-cad

python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -r requirements-dev.txt
```

### 2. Launch Local 3D CAD Explorer
To inspect generated `.step`, `.stl`, or `.urdf` files interactively in the browser:

```bash
npm --prefix apps/viewer install
npm --prefix apps/viewer run dev
```

Open `http://localhost:5173` to inspect models with full orbit controls, cross-section slicing, and bounding-box measurements.

---

## 🧪 Benchmark Capabilities

The engine is benchmarked across complex mechanical parts:

| # | Part Target | Benchmark Prompt Description | Key Engineering Constraints |
|---|---|---|---|
| **01** | **Electronics Enclosure** | Open-top electronics housing with internal mounting bosses | 1.6mm uniform wall, 4x M2.5 standoff bosses with blind holes, corner radius fillets |
| **02** | **Planetary Gear Set** | Flat planetary gear stage with sun, planet, ring, and carrier | Exact gear ratios, tooth clearance tolerance, coaxial pin holes |
| **03** | **Circular Pipe Flange** | 80mm OD circular flange with 30mm bore and 6x bolt circle | 6x M6 through-holes on a 60mm bolt circle, edge chamfers |
| **04** | **Centrifugal Impeller** | Backplate, hub, through-bore, and 12 backward-curved blades | 45-degree blade sweep from root to tip, balanced center of mass |
| **05** | **Stepped Shaft** | 120mm transmission shaft with keyway | 20mm/30mm/20mm stepped diameters, transition fillets, rectangular keyway slot |
| **06** | **Clevis Mount Bracket** | Symmetric clevis bracket with dual rounded lugs | Base mounting holes, horizontal pin bore, weight-saving triangular cutouts |

---

## 🤝 Contributing & License

Contributions are welcome! Please branch from `main`, ensure tests pass, and submit a PR.

- **License:** MIT License — free for personal, educational, and commercial use.
- **Maintainer:** [Utsav Mishra](https://www.linkedin.com/in/utsav-mishra1/) (`utsavmishraa005@gmail.com`)
