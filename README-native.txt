PJ4 NATIVE POLAR VELOCITY AND JOINT-PDF PRODUCTS
Computed on Unity, November 2026; observations 2 February 2017.

Start with report.html (also published at https://npizzo.github.io/pj4-polar-winds/).
These are exploratory pass composites, not instantaneous velocity maps.
Northern pointing-adjusted fields are DIAGNOSTIC: the original broad-amplitude
synthetic linearity gate failed. Southern full pointing fits passed their gates.
Pointing success alone does not validate velocity gradients or absolute winds.
Read independent_image_agreement.csv before interpreting divergence.

FIELDS
fields_o2_N_all_L300.parquet and the corresponding S / L600 files:
  order 2 = quadratic optical-flow displacement; all = full-pass image set.
  L = map grid spacing in km, NOT the effective wavelength resolution.
  lat_deg: planetocentric latitude, degrees.
  lon_east_deg: east System III longitude, degrees.
  u_east, v_north: geographic physical velocity components, m/s.
  U, V: rotated physical velocity components in the polar-map basis, m/s.
  x_km, y_km: oblate conformal polar projection, true scale at pole.
  ix, iy: integer grid-cell indices.
  zeta, divergence, strain_normal, strain_shear, strain: s^-1.
  strain = hypot(strain_normal, strain_shear), nonnegative.
  f = 2*Omega*sin(planetographic latitude), signed, s^-1.
  zeta_over_f: positive for cyclonic rotation at both poles.
  strain_over_absf: nonnegative normalized total strain.
  div_over_absf: normalized divergence; negative means convergence.
  map_factor: projection scale; cell_km_true = L/map_factor.
  area_km2: physical cell area, (L/map_factor)^2.
  n: count of patch matches; pairs: distinct image pairs in the cell.
  se_U, se_V: scatter-derived descriptive errors from per-pair medians.
    They are not calibrated uncertainty intervals: pairs share images.
  pointing_released: original network gate passed.
  pointing_applied: fitted pointing adjustment was applied, including diagnostic.

The field files include a latitude buffer. Figures and summary PDF statistics
use |planetocentric latitude| >=80 degrees. A velocity cell requires >=20
matches from >=3 image pairs; derivatives require all centred neighbours.
No missing derivative cells are filled. Missing values are not zero.
Cell velocity is a componentwise median. Speed statistics describe sampled cells,
not a complete polar cap. Positions refer to the initial tracked surface point;
tracked velocity uses the surface midpoint's east/north basis.

JOINT PDFS
joint_pdf_divergence_L*.png/.pdf: quadratic, north/south, joint density plus
conditional divergence. joint_pdf_models_L*: translation/affine/quadratic.
Adapted from Lenain, Srinivasan, Barkan & Pizzo (2026), Nature Geoscience,
doi:10.1038/s41561-026-01943-0, Figure 4 (GoFlow).
https://pmc.ncbi.nlm.nih.gov/articles/PMC13167463/

joint_density_o*_N/S_L*.npz contains:
  x_edges = zeta/f bin edges; y_edges = strain/abs(f) bin edges.
  density[x_bin,y_bin], area weighted, normalized by all sampled cell area.
  conditional_mean_divergence_over_absf[x_bin,y_bin], >=5 cells per bin.
  counts and displayed_area_fraction.
Display clips pooled extreme tails without renormalizing the visible density.
The density integral over displayed bins equals displayed_area_fraction.
Cell counts are not independent sample counts. No statistical significance is
claimed from the smoothness of conditional contours.

CHECKS
independent_image_agreement.csv: random image-disjoint A/B and chronological
E/L comparisons. Early/late halves share the scalar prior selected using the full
pass but fit their winds and pointing offsets separately. Full validation requires
>=100 common cells, r>=0.8, RMS difference / pooled SD<=0.75, and both halves'
pointing gates. Numerical agreement is separately recorded. Sampling and real
cloud evolution also contribute to early/late differences.
conditional_divergence_agreement.csv: agreement of conditional means in shared
PDF bins. joint_pdf_sector_statistics.csv: physical-area fractions and divergence
means in cyclonic, anticyclonic and strain-dominated sectors.
model_agreement.csv compares common map cells between motion models.

network_checks/ preserves the original result.json, broad synthetic test,
held-out pair comparisons, code hashes, and the later actual-offset nonlinearity
audit. A small actual-offset error does not erase a failed original release gate.
Diagnostic exports are explicitly identified. Selected offsets use zero-mean and
zero-time-trend reference constraints; absolute pointing truth is not supplied.

native_resolution.png and resolved_wavelength_support.csv show the controlled
spatial response and a worst-axis geometric support screen. This configuration
fails the prerequisite for a full 250-1600 km SQG test. There is no validated
native SQG spectrum or radiance-vorticity correlation in this package. Failure
does not falsify SQG or exclude other estimators, supports or selected intervals.

PROVENANCE
provenance.json lists native source, geometry/camera/cache hashes and code hashes.
The 1,111 calibrated detector images (553 M-band) were downloaded directly to
Unity from the PDS orbit04 calibrated collection. These are not raw telemetry.
All native image sizes and label MD5 checksums passed. The JOVE ZIP has mapped
radiance and source identifiers but does not embed the native .IMG exposures.
The large archive, native images and complete candidate/accepted tracking tables
remain at the Unity location in provenance.json; they are not in this download.
SHA256SUMS verifies the compact derived products.

IMPORTANT LIMITS
Fixed detector-calibration uncertainty has not been fully propagated.
Spatial coverage, effective resolution and observation time vary.
A fine output grid does not imply resolved fine-scale flow.
A shared-image reference comparison is not independent absolute accuracy.
The published cropped northern map's origin remains insufficiently determined
for a new geographic point-by-point native-versus-TRACKER4 comparison.
