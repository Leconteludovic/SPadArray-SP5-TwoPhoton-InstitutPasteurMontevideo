# SPadArray-SP5-TwoPhoton — Institut Pasteur de Montevideo

Documentation repository for the **custom two-photon SPAD array FLIM system** built at Institut Pasteur de Montevideo, operated on a Leica SP5 stand.

**Author**: Ludovic Leconte — Institut Pasteur de Montevideo / MicroPICell, Nantes Université (France BioImaging)  
**Collaborators**: Vicidomini Lab (IIT Genoa) — BrightEyes project

---

## System overview

| Component | Model |
|---|---|
| Microscope stand | Leica SP5 |
| Laser | Coherent InSight X3 (two-photon, 680–1300 nm) |
| Scanner | ScannerMAX galvo-galvo |
| Detector | Genoa Instruments PRISM (5×5 SPAD array) |
| TCSPC / acquisition | BrightEyes-MCS (IIT Genoa) |
| AOTF | AA Opto-Electronic MPDS1C-D65-34-85.135-RS |
| Z-drive | Leica FocusDrive (CTR6500) |

---

## Repository structure

```
protocols/          # Operator protocols (PDF/DOCX)
hardware/
  optomechanics/    # Optical layouts, mounting diagrams
  solidworks/       # CAD files — NOT public (.gitignore)
  electronics/      # Signal chain, wiring — NOT public (.gitignore)
images/             # Setup photos, screenshots, beam profiles
software/           # BrightEyes configs, Micro-Manager — NOT public (.gitignore)
```

> **Note**: `hardware/solidworks/`, `hardware/electronics/`, and `software/` are excluded from this public repository. Contact the author for access.

---

## Protocols

| File | Content |
|---|---|
| `protocols/01_optical_alignment.pdf` | Two-photon beam alignment procedure |
| `protocols/02_brighteyes_mcs.pdf` | BrightEyes-MCS acquisition guide |
| `protocols/03_spad_array_usage.pdf` | SPAD array operation + ISM/APR post-processing |

---

## Analysis pipelines

See companion repository: [brighteyes-pipelines](https://github.com/Leconteludovic/brighteyes-pipelines)

---

## Key technical notes

- Scan lens focal length: ~45.3 mm (empirically established, supersedes nominal 40 mm)
- Defective pixel: ch10 (DFD ≈ 0.259)
- Z-stack backlash correction: 20 µm downward overshoot protocol
- FLIM integration: pending InSight X3 master-clock / BrightEyes-MCS slave mode (in development, IIT Genoa)
- Signal conditioning chain: SYNC OUT → ZX60-43-S+ → ADCMP600 → 3.3V TTL → DIO26
