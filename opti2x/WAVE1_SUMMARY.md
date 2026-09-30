# Wave experiments SUMMARY — F-S2-QT (Step2 QT association)

**Date:** 2026-09-29 PT  
**Pin:** `5ff08954…` clean Eigen-only (`4bd69197…`) — **NO H1, NO L1 QT stack**  
**Contract:** `regenie-v4.1.3-serial-eigen-step2-qt-n2000-m45337-b400-v1`  
**Formal baseline median (N=30):** **1.47 s** (`benchmarks/dev_fixture_s2_qt/baseline.json`)  
**Session remasure (N=5):** 1.48 s  
**Envelope:** `--threads 1`, `OMP/MKL/OPENBLAS/GOTO_NUM_THREADS=1`, `taskset -c 0`, NLWP=1  
**Oracle:** `assoc_Y1.regenie` byte-identical  
**Do NOT cite H1 L0 or L1 QT speedups as Step2 evidence.**

## Results table

| ID | Status | Median (N=8) | vs formal 1.47 | Oracle | NLWP | Notes |
|----|--------|-------------:|---------------:|--------|------|-------|
| H-S2-1 n_zero in bed decode | **REJECT** | 1.750 s | **0.84×** | PASS | 1 | pre-flip n_zero; regression |
| H-S2-2 fast autosomal decode | **ACCEPT** | 1.160 s | **1.27×** | PASS | 1 | branch-lite decode + missing idx |
| H-S2-2b raw pointer decode | **ACCEPT** | 1.015 s | **1.45×** | PASS | 1 | `double*` + memset |
| H-S2-3 sparse mean-impute | **ACCEPT** | 1.375 s | **1.07×** | PASS | 1 | fill only missing idx |
| H-S2-4 residualize noalias | **REJECT** | 1.435 s | 1.02× | PASS | 1 | noise |
| H-S2-5 batch resid GEMM | **REJECT** | 0.995 s | 1.48× | **FAIL** | 1 | FP order ≠ byte oracle |
| H-S2-6 int8 LUT decode | **ACCEPT** | 0.910 s | **1.62×** | PASS | 1 | static `int8_t lut[256][4]` |
| H-S2-7 prep_snp_stats setZero | **ACCEPT** | 0.880 s | **1.67×** | PASS | 1 | stack on H-S2-6 |
| **H-S2-8 force dense QT** | **ACCEPT** | **0.790 s** | **1.86×** | **PASS** | 1 | skip sparse G path; best |
| H-S2-9 flip/genocount micro | ACCEPT | 0.790 s | 1.86× | PASS | 1 | no added wall vs H-S2-8 |

**Best oracle-pass wall:** **1.86×** (median 0.79 s) — **below 2× gate → NOT_YET**.

## Profile (baseline → best)

| region | baseline ≈% | after H-S2-8 (approx) |
|--------|------------:|----------------------:|
| `parseSnpfromBed` | ~43–62% | still ~45%+ of remaining |
| `mean_impute_g` | ~14% | largely removed (idx fill) |
| `check_sparse_G` + sparseView | ~11–14% | removed by H-S2-8 |
| `residualize_geno` | ~9% | larger share (all dense) |
| `compute_score_qt` / print | ~10% | residual |

## Why not 2×

Step2 QT wall is **bed decode dominated**. After aggressive unpack + skipping sparse QT path,
serial Eigen-only residualize + score + sumstats formatting still leave ~0.79 s vs 1.47 s
baseline (**1.86×**). Batch residualize GEMM breaks byte-identical oracle (H-S2-5).
Further 2× would need substantially faster bed→double unpack (SIMD/ISA) or a renegotiated
numeric oracle — both outside this wave’s accepted primary levers (`-march` excluded as primary).

## Forbidden check

- No H1 / L1 QT credit  
- No threads / MKL / `-march` as primary win  
- No oracle edits; Firth/SPA remain off  
- No public git push  
