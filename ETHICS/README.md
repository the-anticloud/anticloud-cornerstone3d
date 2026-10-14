# Ethics — CORNERSTONE3D

**Project:** CORNERSTONE3D  
**Category:** MEDICAL_HEALTH  
**Upstream:** https://github.com/cornerstonejs/cornerstone3D  
**Pinned commit:** `9221dd884473069308a754adb2acb5a2fc4fe46a`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `cab466db9bf500914eb79eb4c08f24cf1823ae84c94ad11c27d27e71fd548b14`  
**Date:** October 2026

## Position

CORNERSTONE3D is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
