# Developer Cookbooks — CORNERSTONE3D

**Project:** `CORNERSTONE3D`
**Category:** MEDICAL_HEALTH
**Domain:** medical health and clinical systems
**Date:** 2026-10-07

---

## Common Tasks

### Adding a New Feature
1. Create a feature branch
2. Write tests first (TDD)
3. Implement the feature
4. Run `python tools/run_bench.py`
5. Submit a pull request

### Debugging
```bash
python -m pdb src/cornerstone3d/main.py
```

### Profiling
```bash
python -m cProfile -o profile.out src/cornerstone3d/main.py
```

### Security Scanning
```bash
python tools/run_bench.py --only owasp
```

## Code Patterns

### Error Handling
All errors are logged to the AIOSS chain with full context.

### Configuration
Configuration is loaded from environment variables with sensible defaults.

### Testing
Tests use pytest with coverage reporting. Target: >80% coverage.

## Verification

All 16 checks PASS. Evidence: `ISOLATED_LAB_RESULTS/03_Result_Register.md`.
