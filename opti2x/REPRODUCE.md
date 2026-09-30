# Reproduce F-S2-QT baseline / QUALIFIED candidate

```bash
ROOT=/workspace/opti2x/runs/regenie-v4.1.3
export OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 OPENBLAS_NUM_THREADS=1 GOTO_NUM_THREADS=1
IN=$ROOT/benchmarks/dev_fixture_s2_qt/inputs/regenie_s2_n2000
PRED=$ROOT/benchmarks/dev_fixture_s2_qt/inputs/pred.list

# Baseline (clean pin)
taskset -c 0 $ROOT/source/regenie --step 2 \
  --bed $IN --covarFile ${IN}.covar --phenoFile ${IN}.pheno \
  --bsize 400 --threads 1 --pred $PRED --out /tmp/s2_base/assoc

# QUALIFIED candidate (H-S2-10 stack)
taskset -c 0 $ROOT/deliverable-s2-qt/regenie.s2qt --step 2 \
  --bed $IN --covarFile ${IN}.covar --phenoFile ${IN}.pheno \
  --bsize 400 --threads 1 --pred $PRED --out /tmp/s2_cand/assoc

cmp /tmp/s2_base/assoc_Y1.regenie $ROOT/benchmarks/dev_fixture_s2_qt/oracle/assoc_Y1.regenie
cmp /tmp/s2_cand/assoc_Y1.regenie $ROOT/benchmarks/dev_fixture_s2_qt/oracle/assoc_Y1.regenie
```

Formal baseline: `benchmarks/dev_fixture_s2_qt/scripts/run_formal_baseline.sh`  
Bench helper: `experiments/_helpers/bench_s2.sh`  
Paired cert: `experiments/_helpers/cert_paired_s2_qt.sh 30 202609292`
