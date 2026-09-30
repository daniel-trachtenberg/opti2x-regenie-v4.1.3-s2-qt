# Opti2x F-S2-QT — QUALIFIED report

## Fixture
| | |
|--|--|
| Path | `benchmarks/dev_fixture_s2_qt/` |
| N, M | 2000, 45337 (MAF≥0.05 from M_RAW=50000) |
| Step2 `--bsize` | 400 (114 blocks) |
| `--pred` | frozen Step1 LOCO (`step1_frozen/`; not timed) |
| seed | 202609292 |
| Baseline binary | clean pin `4bd69197…` (NO H1, NO L1) |
| Formal N=30 median | **1.47 s**, oracle_all=True, NLWP=1 |

## Best candidate (Wave 2)
- Stack: int8→**double bed LUT** + raw-pointer / all-in-analysis decode + prep slim/setZero + **force dense QT** (H-S2-2/2b/3/6/7/8 + **H-S2-10**)
- Paired N=30 candidate median **0.65 s**; session baseline median 1.48 s
- vs formal 1.47 s: **~2.26×**; paired aggregate **exp(mean log)=2.29×**, **lower_95=2.25×**
- Oracle: `assoc_Y1.regenie` **byte-identical** (all 30 pairs)
- NLWP=1

## Status
**QUALIFIED** — `aggregate_speedup_lower_95 ≥ 2.0` under contract
`regenie-v4.1.3-serial-eigen-step2-qt-n2000-m45337-b400-v1`.

Cert: `benchmarks/certification_s2_qt/summary.json`  
See `experiments/s2_qt/SUMMARY.md` for Wave1+Wave2 slate.
