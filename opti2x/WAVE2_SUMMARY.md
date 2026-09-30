# Wave 2 SUMMARY — F-S2-QT

**Date:** 2026-09-29 PT  
**Starting best:** H-S2-8 force dense + decode stack **0.79 s = 1.86×**  
**Formal baseline:** 1.47 s

## Reprofile (H-S2-8 candidate)

`parseSnpfromBed` still ~53% self; Eigen GEMV ~16%; score/print/`get_logp` ~5–8%.

## Wave 2 results

| ID | Status | Median (N=8) | vs formal 1.47 | Oracle | NLWP | Notes |
|----|--------|-------------:|---------------:|--------|------|-------|
| H-S2-10 double LUT + all-in decode + slim prep | **ACCEPT** | **0.66 s** | **2.23×** | **PASS** | 1 | on H-S2-8 stack |

## Certification (N=30 paired, seed 202609292)

| metric | value |
|--------|------:|
| baseline median (session) | 1.48 s |
| candidate median | **0.65 s** |
| median ratio | **2.28×** |
| aggregate exp(mean log) | **2.29×** |
| **lower_95** | **2.25×** |
| oracle_all | True |
| NLWP | 1 |
| **status** | **QUALIFIED** |

vs formal 1.47 s median: 1.47/0.65 ≈ **2.26×**.

## Forbidden check
No H1/L1 credit; no threads/MKL/-march primary; no oracle edits; no public push.
