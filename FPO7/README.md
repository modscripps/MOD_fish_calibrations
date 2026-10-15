# FPO7 bench noise floor

`FPO7_benchnoise.mat` was copied from `MOD_fish_lib`'s
`EPSILOMETER/CALIBRATION/FPO7/FPO7_notdiffnoise.mat` and renamed for
clarity in this repo (`chi_processing` branch of `MOD_fish_processing`,
2026-07-27).

- **Original source:** `MOD_fish_lib/EPSILOMETER/CALIBRATION/FPO7/FPO7_notdiffnoise.mat`
  (also present, identical, under `MOD_fish_lib/EPSILOMETER/CALIBRATION/ELECTRONICS/`).
- **Why "notdiffnoise" became "benchnoise":** the original name refers to
  this being the noise floor for a "not differentiated" FP07 signal - i.e.
  raw thermistor voltage, as opposed to an analog-differentiated dT/dt
  signal (`FPO7_noise.mat`, the "Tdiff" variant, not copied here - every
  deployment `MOD_fish_processing` processes uses raw, undifferentiated
  FP07 voltage; see `MODprocess_L2_fpo7_transfer_function.m`'s docstring).
  "Bench noise" describes what the file actually contains more directly:
  a polynomial fit to the FPO7 channel's noise floor measured on the bench
  (n0..n3, `noise(f) = n0 + n1*log10(f) + n2*log10(f)^2 + n3*log10(f)^3`,
  in log10(volts^2/Hz)), used by `MODprocess_L2_fpo7_cutoff.m` to find the
  frequency where an observed spectrum drops into instrument noise.
- **Not per-probe.** This is one shared bench-measured constant, not keyed
  by probe serial number like `SBE/` or `SHEAR_PROBES/` - every FP07
  channel with undifferentiated output uses the same file.
- **Not otherwise modified** - contents (`n0`, `n1`, `n2`, `n3`) are
  identical to the `MOD_fish_lib` original, only the filename changed.
