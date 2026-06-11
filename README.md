# MatCalib Pro


LS-DYNA material model calibration tool. Upload test data (CSV/Excel) or a plot
image, declare units and specimen geometry, and the tool fits the constitutive
parameters and exports a ready-to-run LS-DYNA `.k` card plus a professional PDF
report and Excel summary.

## What it does

- 10 material families: metals, polymers, rubber, composites, foam, shape
  memory alloys, ceramics, biomaterials, biocomposites, piezoelectric.
- 17 LS-DYNA card generators (primary + secondary cards per family).
- User-selectable card per family: the tool recommends a default but you can
  pick a different card in the same family (e.g. a metal as MAT_024, MAT_015
  Johnson-Cook, or MAT_018 power-law; a polymer as MAT_024 / MAT_089 / MAT_187
  SAMP-1; rubber as MAT_077 / MAT_181; foam as MAT_057 / MAT_083). The fit is
  re-run for the chosen model and the card is generated accordingly.
- 14 input test types (stress-strain, force-displacement, 3-point bend, shear,
  torsion, biaxial, planar, compression, hydrostatic, relaxation, creep, ...),
  each with per-axis unit declaration and the specimen geometry needed to
  convert to the measure the model requires.
- Multi-mode hyperelastic fitting (uniaxial + equibiaxial + planar).
- Image digitisation (Claude vision backend + classical pixel backend) with a
  user-correction layer and an honest elastic-modulus reliability warning.
- Confidence rating on every card; missing-data reporting for underdetermined
  models (e.g. composites).

## Engineering notes / known limitations

- Elastic modulus from a plot image: the digitiser traces the curve with a
  dual-direction sub-pixel tracer (it follows near-vertical regions by rows, not
  just one value per column) and resamples in arc length, so the steep elastic
  branch is preserved and E is recovered to a few percent on typical curves
  (validated end-to-end: mean ~6% across mild steel, aluminium, titanium,
  copper, brass, magnesium). The hard residual case is high-modulus / low-yield-
  strain materials (e.g. high-strength steel) on a FULL-range plot, where the
  elastic region is only a handful of pixels wide and is genuinely pixel-starved;
  there the tool still reports its best estimate but flags it "approximate" and
  invites an optional datasheet E. For best accuracy on such materials, digitise
  a plot zoomed to the low-strain region.
- Composite (MAT_054) and ceramic (MAT_110) calibration is underdetermined from
  a single curve; the tool reports exactly which tests are missing rather than
  inventing values.
- Piezoelectric e-matrix uses an isotropic-stiffness fallback; supply the full
  transversely-isotropic constants for best accuracy.
- Card field layouts shift slightly between LS-DYNA R-releases; verify one card
  of each type in your installed version before use.

## Important note

Generated material cards should always be reviewed and validated by the user before use in engineering simulation.