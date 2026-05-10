---
name: roms
description: >
  Progressive-disclosure skill for ROMS (Regional Ocean Modeling System),
  the terrain-following, finite-difference, free-surface, primitive-equation
  regional ocean model widely used for coastal, shelf, and basin-scale
  studies. Covers the build (CMake and legacy makefile), the User/Apps
  case directory convention, the ocean.in namelist, the analytical vs
  data-driven forcing options, the ESM/ coupling layer (e.g., COAWST,
  ROMS-CICE), and the official documentation.
version: 0.1.0-scaffold
tags:
  - earth-science
  - regional-ocean
  - coastal-ocean
  - terrain-following
  - sigma-coordinate
  - roms
  - rutgers
  - ucla
  - fortran
---

# ROMS (Regional Ocean Modeling System) Guide

> **ROMS** = Regional Ocean Modeling System.
> Maintainer: ROMS group / Rutgers University, with significant UCLA and community contributions
> Source: https://github.com/myroms/roms
> Docs: https://www.myroms.org / https://www.myroms.org/wiki
> License: see `License_ROMS.md`
> Skill author: Koutian Wu (ktwu01@gmail.com)
> Skill version: 0.1.0-scaffold

**What ROMS does:** Solves the hydrostatic, primitive equations of the ocean on a curvilinear **Arakawa C-grid** with **generalized terrain-following S-coordinates** (often loosely called "sigma", but actually stretched, controlled by `Vtransform`, `Vstretching`, `theta_s`, `theta_b`, `hc`). Free-surface, finite-difference, split-explicit time stepping (a fast barotropic mode and a slower baroclinic mode). Designed for regional studies: coastal, shelf, basin, eddy-resolving and submesoscale-permitting domains.

**Heads up:** there are several historical ROMS forks (Rutgers/myroms, UCLA, AGRIF). This skill targets the **Rutgers/myroms** branch hosted at `github.com/myroms/roms`. The other forks have different build systems and file structures; do not conflate them.

**Who this skill is for:** Researchers running a regional ocean configuration (estuary, shelf, regional basin), people coupling ROMS to wave (SWAN) or atmosphere (WRF) models via COAWST, and developers extending ROMS physics or boundary conditions.

---

## Quick Decision Tree

```
"What do I need?"
│
├─ 🆕 What is ROMS and how is it different from MOM6/POP/NEMO/MITgcm?
│  └─ Read: reference/overview.md
│
├─ 🛠️ Build ROMS (CMake or legacy makefile)
│  └─ Read: reference/build.md
│
├─ 📁 Application directory (User/Apps/<MyCase>/) convention
│  └─ Read: reference/application-layout.md
│
├─ 📝 The ocean.in namelist
│  └─ Read: reference/namelist-ocean-in.md
│
├─ 🔧 Compile-time CPP options (analytical vs data forcing, mixing schemes, ...)
│  └─ Read: reference/cpp-options.md
│
├─ 🔗 Coupling: COAWST (with WRF + SWAN), ROMS-CICE, ESMF
│  └─ Read: reference/coupling.md
│
└─ 🐛 Common build/run failures
   └─ Read: reference/debugging.md
```

---

## Repo Layout (verified from clone)

```
roms/
├── CMakeLists.txt    # Modern CMake build
├── makefile          # Legacy makefile build
├── Compilers/        # Per-compiler / per-machine config
├── Data/             # Sample data
├── docs/             # Pull request templates, changelog
├── ESM/              # ESMF/NUOPC coupling layer
├── License_ROMS.md   # ROMS license (BSD-style)
├── Master/           # Top-level driver source
├── ROMS/             # Core model source (Modules, Functionals, ...)
└── User/             # User application directory (case definitions live here)
```

---

## Critical Rules

1. **An "application" is a header file plus build options.** ROMS uses a CPP-based configuration system: each app has a header (`<app>.h`) listing compile-time `#define` switches (which physics, which forcing source, which boundary condition). Change the header → recompile.
2. **`ocean.in` controls run-time parameters.** Time steps, output frequencies, sponge zones, file paths, processor layout. Different from compile-time CPP switches.
3. **Two build paths.** CMake (`CMakeLists.txt`) is the modern path; the legacy `makefile` (with `build_roms.sh` / `build_roms.bash` wrappers) is still widely used.
4. **NLM, TLM, ADM, RPM are four model modes.** Nonlinear (forward), tangent linear, adjoint, representer. Most users only need NLM. ADM is needed for 4D-Var data assimilation.
5. **Sigma-coordinate ⇒ pressure-gradient errors over steep topography.** This is a known characteristic, not a bug. Smoothing topography or using carefully tuned vertical-stretching parameters mitigates it.
6. **For coupled atmosphere–ocean–wave**, COAWST is the conventional bundle (ROMS + WRF + SWAN with MCT coupler).

---

## Reference Index

| File | Topic |
|---|---|
| reference/overview.md | ROMS design, sigma coordinates, regional focus |
| reference/build.md | CMake and legacy makefile |
| reference/application-layout.md | User/Apps/ convention, app headers |
| reference/namelist-ocean-in.md | ocean.in walkthrough |
| reference/cpp-options.md | Compile-time switch families |
| reference/coupling.md | COAWST, ESMF/NUOPC |
| reference/debugging.md | Common errors |

## Critical agent gotchas (Gemini-reviewed)

- **`build_roms.sh` (or `.bash`) is the standard build wrapper** for the legacy makefile path, used by most HPC tutorials and community recipes. CMake is newer and increasingly supported but not yet dominant.
- **`varinfo.yaml` (or `varinfo.dat` in older versions) is required.** This metadata file maps internal variable names to NetCDF fields and is referenced from `ocean.in`. ROMS will not run without it.
- **Application selection.** The application header (e.g., `upwelling.h`) is selected by setting `ROMS_APPLICATION` (env var or in `cppdefs.h`). Each app has its own `.h` header listing the CPP `#define` switches.
- **NetCDF-Fortran is mandatory.** Build configs in `Compilers/` link both NetCDF-C and NetCDF-Fortran; either alone is insufficient.
- **C-grid means staggered dimensions.** $u$, $v$, and $\rho$ points have different grid sizes in NetCDF output. Always check the dimension name (`xi_u`, `xi_v`, `xi_rho`).
- **Vertical S-coordinates need `Vtransform`, `Vstretching`, `theta_s`, `theta_b`, `hc`.** These are in `ocean.in` and define how levels stretch with bathymetry.
- **COAWST coupling uses MCT**, not just the `ESM/` directory. MCT is a separate dependency. The `ESM/` directory provides the ESMF/NUOPC layer; COAWST workflows pre-date NUOPC and use MCT.

## Status

Scaffold (v0.1.0-scaffold). Source-grounded layout verified, with Gemini critique pass on 2026-05-09 to fix the S-coordinate description, remove a fabricated CPP switch, and add critical agent gotchas. Operational depth being filled in.
