# Architecture of text-to-cad (cadgen)

Designed and developed by **Utsav Mishra** (<utsavmishraa005@gmail.com>).

## 1. High-Level System Architecture

`text-to-cad` bridges declarative prompt generation and high-precision mechanical engineering. It enables autonomous AI agents to design physically manufacturable mechanical parts and assemblies through parametric code execution, deterministic tessellation caching, and instant WebGL inspection.

```
       +---------------------------------------------+
       |         Autonomous AI Agent (Makeable)       |
       +---------------------------------------------+
                              |
                     Parametric CAD Script
                              v
       +---------------------------------------------+
       |       cadgen Core Engine & Door Layer       |
       |  (Python / build123d / OpenCASCADE OCCT)   |
       +---------------------------------------------+
          /           |             |             \
         v            v             v              v
     +--------+  +---------+  +-----------+  +------------+
     |  STEP  |  |   STL   |  |    DXF    |  |  3MF / GLB |
     | (AP214)|  | (Mesh)  |  | (Profiles)|  | (Assembly) |
     +--------+  +---------+  +-----------+  +------------+
          \           |             |             /
           v          v             v            v
       +---------------------------------------------+
       |   Content-Addressed Cache & Topology Index  |
       |            (SHA-256 Byte Fingerprint)       |
       +---------------------------------------------+
                              |
                 Binary surf streaming / JSON
                              v
       +---------------------------------------------+
       |       WebGL CAD Viewer (React + Three.js)   |
       |  - Real-time Orbit / Pan / Section Cut      |
       |  - Selection Raycaster & Tree Inspector     |
       |  - Multithreaded Web Worker Tessellation    |
       +---------------------------------------------+
```

## 2. Key Modules & Subsystems

- **`packages/cadgen`**: Python-based parametric compiler integrating OpenCASCADE 7.x via `build123d`.
- **`packages/cadgen-js`**: Zero-dependency browser client for unpacking `.surf` buffers and streaming tessellations.
- **`apps/viewer`**: Interactive single-page CAD inspection workbench built with Vite, React 18, and Three.js.
- **`apps/docs`**: Documentation portal with live part previews and code playgrounds.
- **`skills/`**: Comprehensive agentic skill definitions covering 3D printing (DFAM, Bambu Labs, G-Code), sheet metal laser cutting (SendCutSend), hardware fasteners (ISO/McMaster), 2D profiles (DXF), and robotics (URDF, SRDF, SDF).

## 3. Determinism & Integrity Guarantees

Every compiled 3D artifact is content-addressed by its source tokens and byte geometry. Re-running a model is guaranteed to be a bit-for-bit idempotent operation, preventing cache poisoning in high-concurrency multi-agent environments.
