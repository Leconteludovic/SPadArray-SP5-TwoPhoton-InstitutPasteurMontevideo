# Z-stack + Time-points automation — SPAD/SP5

Automated Z-stack acquisition with optional time-lapse, combining:
- **pymmcore-plus** — Leica FocusDrive Z control via CTR6500 (COM4, Micro-Manager 2.0)
- **BrightEyes REST API** — FLIM acquisition trigger via `http://127.0.0.1:8000`
- **brighteyes-ism** — optional APR + FRC post-processing after acquisition

**Author**: Ludovic Leconte — Institut Pasteur de Montevideo

---

## Files

| File | Description |
|---|---|
| `Zstack_TimePoints_SPAD_v5_upward.ipynb` | Main notebook — Z-stack + time-points automation |
| `Z-stack_time.bat` | Windows launcher — opens the notebook directly in Jupyter |

---

## Key parameters (cell 1)

| Parameter | Default | Description |
|---|---|---|
| `Z_RANGE_UM` | 5.0 µm | Total stack thickness |
| `Z_STEP_UM` | 0.3 µm | Z step (Nyquist axial at NA=1.4) |
| `N_TIMEPOINTS` | 1 | Number of time-points |
| `DELTA_T_S` | 60 s | Interval between time-points |
| `BACKLASH_OVERSHOOT_UM` | 20 µm | Leica FocusDrive backlash compensation |
| `REF_CHANNEL` | 12 | Central SPAD pixel (5×5 array) |

---

## Important notes

### Upward-only mode ⚠
This notebook acquires the Z-stack **upward only** — it starts at the current focus position (z_origin = bottom of stack) and moves up.  
**Set your focus at the lowest plane of interest before running.**  
The FocusDrive does not return to z_origin between time-points in this mode.

### Before running
1. Close Micro-Manager (COM4 conflict)
2. Start BrightEyes-MCS and launch the HTTP server from ScriptLauncher:  
   `FastAPIServerThread(main_window)`
3. Configure in BrightEyes: detector, XY pixels, range, dwell time, time bins, save folder
4. Set focus manually on the Leica controller — do not touch focus after launching

### Backlash compensation
The Leica FocusDrive has up to ~5 µm of backlash when reversing direction.  
The script applies a **20 µm downward overshoot** before returning to any target position (always approaches from below).

### File naming
Each acquired plane is automatically renamed:  
`T001_Z012_z10.234um.h5`

### Output structure
```
zstack_YYYYMMDD_HHMM/
  T001_Z001_z0.000um.h5
  T001_Z002_z0.300um.h5
  ...
  acquisition_log.txt
  diagnostic/          (if post-processing selected)
    *_images.png
    *_shifts_full.png
    *_frc.png
    ...
```

---

## Dependencies

Install via `brighteyes-env` (see [napari-installer](https://github.com/Leconteludovic/napari-installer)):

```bash
pip install pymmcore-plus requests brighteyes-ism tifffile matplotlib jupyterlab
```

Micro-Manager 2.0 must be installed separately: https://micro-manager.org
