# Technical Architecture — CORNERSTONE3D

**Upstream:** [https://github.com/cornerstonejs/cornerstone3D](https://github.com/cornerstonejs/cornerstone3D)
**License:** MIT
**Category:** MEDICAL_HEALTH
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Medical imaging rendering engine

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local clinical decision support — air-gapped hospital deployment
2. AIOSS tamper-evident EHR audit chain (HL7 FHIR event log)
3. AES-256 encryption for all patient data at rest and in transit
4. Single-binary deployment on clinical workstations
5. HIPAA-compliant zero-cloud architecture
6. Offline diagnostic inference: image analysis, lab result interpretation
7. GPU/CPU equalizer: runs on clinical GPU workstation or standard CPU server
8. Open CLI for EHR integration replacing proprietary HL7 middleware

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_cornerstone3d.spec` or `go build -o cornerstone3d`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |