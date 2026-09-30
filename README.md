# Opti2x optimized regenie v4.1.3 — Step2 QT

This repository contains an Opti2x-optimized regenie **v4.1.3** source tree for the **Step2 quantitative-trait (S2 QT)** association fixture.

**Status: QUALIFIED.** The H-S2-10 stack measured a **~2.26× median speedup** versus the formal **1.47 s** baseline, with a **2.25× one-sided 95% lower bound** over **N=30** paired runs (candidate median **0.65 s**).

## Claim scope

The claim is **fixture-scoped**, not package-wide: a serial, Eigen-only Step 2 QT association fixture with **N=2000**, **M=45337**, and Step2 **`--bsize 400`**, under one-thread controls with a frozen `--pred`. The credited change is the **H-S2-10** stack (double bed LUT + related decode/prep densify paths) on the Step2 genotype hot path.

Baseline is the **CLEAN upstream pin** (no H1 Level-0 and no L1 QT credit). This package does **not** re-claim the separate L0 H1 ~3.14× or L1 QT ~2.29× results.

This is **not** a package-wide regenie claim, **not** a Step1/L0/L1 claim, **not** a `-march=native` claim, and **not** an MKL/OpenBLAS claim.

## Upstream pin

Upstream: <https://github.com/rgcgithub/regenie>  
Pin: `5ff08954c5ada4c8fd037a8d38cbb57acbf0dd01` (`v4.1.3`)

Related (separate claims):
- [opti2x-regenie-v4.1.3](https://github.com/daniel-trachtenberg/opti2x-regenie-v4.1.3) — L0 H1 QUALIFIED (~3.14×); **not** credited here
- [opti2x-regenie-v4.1.3-l1-qt](https://github.com/daniel-trachtenberg/opti2x-regenie-v4.1.3-l1-qt) — L1 QT QUALIFIED (~2.29×); **not** credited here

## Build

Eigen-only build (no MKL/OpenBLAS and no `-march=native`):

```bash
make BGEN_PATH=/path/to/bgen/v1.1.7 HAS_BOOST_IOSTREAM=1 -j"$(nproc)"
```

The packaged source intentionally excludes compiled binaries and object/archive files. See `opti2x/REPRODUCE.md` for the pinned patch, fixture contract, correctness oracle, and timing procedure.

## Evidence

- `opti2x/OPTIMIZATION_REPORT.md` — qualification report and limitations
- `opti2x/CLAIM_SCOPE.md` — explicit claim boundaries
- `opti2x/REPRODUCE.md` — build and certification procedure
- `opti2x/certification.json`, `opti2x/attestation.json`, `opti2x/stats.json` — certification evidence
- `opti2x/WAVE1_SUMMARY.md`, `opti2x/WAVE2_SUMMARY.md` — wave summaries
- `opti2x/optimized.patch` — H-S2-10 stack patch
