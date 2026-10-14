# Students — CORNERSTONE3D

**Project:** CORNERSTONE3D  
**Category:** MEDICAL_HEALTH  
**Upstream:** https://github.com/cornerstonejs/cornerstone3D  
**Pinned commit:** `9221dd884473069308a754adb2acb5a2fc4fe46a`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `cab466db9bf500914eb79eb4c08f24cf1823ae84c94ad11c27d27e71fd548b14`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `9221dd884473069308a754adb2acb5a2fc4fe46a`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `cab466db9bf500914eb79eb4c08f24cf1823ae84c94ad11c27d27e71fd548b14`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
