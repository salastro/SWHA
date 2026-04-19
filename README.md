# Solar Wind Helios 2 Analysis — Bi-Kappa VDF Fitting & Radial Evolution

[![Python](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Data: Helios 2](https://img.shields.io/badge/data-Helios%202%20%280.29–1%20AU%29-orange)](https://doi.org/10.1007/s11207-018-1377-3)

Analysis of **Helios 2** solar wind proton velocity distribution functions (VDFs) using bi-kappa distribution fitting, and a comprehensive power-law study of the radial evolution of 50+ plasma and field parameters between **0.29 and 1 AU**.

---

## Overview

The solar wind is a weakly collisional plasma, meaning its particle velocity distributions routinely deviate from a Maxwellian — developing temperature anisotropies, field-aligned beams, and enhanced suprathermal tails. The **bi-kappa distribution** captures these non-Maxwellian features through a single spectral index κ, which converges to a Maxwellian as κ → ∞ and produces progressively heavier power-law tails as κ decreases.

This project fits bi-kappa distributions to every Helios 2 proton VDF and extracts the proton core number density *n*_p, bulk velocity **v**, parallel temperature *T*_∥, perpendicular temperature *T*_⊥, and spectral index κ. These fitted parameters, together with the concurrent magnetic field, are used to compute a comprehensive set of derived plasma quantities and characterise their radial dependence via power-law fits *f* ∝ *r*^α.

### Key results

| Quantity | Power-law index α | CGL / Parker prediction |
|---|---|---|
| Proton density *n*_p | −2.075 | −2 (mass flux) |
| Total magnetic field *B* | −1.530 | — (mixed) |
| *B*_r (radial component) | −1.873 | −2 (Parker) |
| *B*_t (tangential component) | −1.214 | −1 (Parker) |
| Perp. temperature *T*_⊥ | −1.315 | −2 (CGL) |
| Par. temperature *T*_∥ | −0.825 | 0 (CGL) |
| Bulk speed *V* | ≈ 0 | 0 (terminal velocity) |
| Perp. entropy *S*_⊥ | +2.290 | 0 (CGL) |
| Par. entropy *S*_∥ | +0.559 | 0 (CGL) |
| Proton magnetic moment μ_p | +0.215 | 0 (CGL invariant) |

The κ index decreases systematically from ~4–5 at 1 AU to ~2.5–3 at 0.3 AU, indicating progressively stronger non-Maxwellian character closer to the Sun — consistent with greater wave activity and velocity filtration effects in the inner heliosphere.

---

## Notebook

### `SWHA_Helios2_FINAL.ipynb`

The single self-contained analysis notebook. It runs end-to-end from raw Helios 2 data to all figures and the power-law index table. The notebook is structured as follows:

| Section | Description |
|---|---|
| **1. Data loading** | Load Helios 2 proton and magnetic field data via `heliopy` |
| **2. VDF inspection** | Plot raw 3D VDFs, identify population boundaries (maxima/minima) |
| **3. Population cut** | Isolate proton core from beam and alpha particles in *E/q* space |
| **4. Bi-kappa fitting** | Fit drifting bi-kappa distributions to each proton VDF |
| **5. Parameter extraction** | Extract *n*_p, **v**, *T*_∥, *T*_⊥, κ; rotate to RTN frame |
| **6. Derived quantities** | Compute pressures, betas, Mach numbers, entropy proxies, etc. |
| **7. Radial fits** | Power-law OLS fits *f* ∝ *r*^α for all quantities |
| **8. Figures** | Generate all publication figures |

---

## Bi-Kappa Distribution

The proton core is modelled as a drifting, gyrotropic bi-kappa distribution in the magnetic-field-aligned frame:

$$f_\kappa(v_\parallel, v_{\perp 1}, v_{\perp 2}) = \frac{n_p}{\pi^{3/2} \kappa^{3/2} w_\parallel w_\perp^2} \frac{\Gamma(\kappa+1)}{\Gamma(\kappa - 1/2)} \left[ 1 + \frac{1}{\kappa} \left( \frac{(v_\parallel - u_\parallel)^2}{w_\parallel^2} + \frac{v_{\perp 1}^2 + v_{\perp 2}^2}{w_\perp^2} \right) \right]^{-(\kappa+1)}$$

with κ > 3/2 and physical temperatures

$$T_{\parallel/\perp} = \frac{m_p w_{\parallel/\perp}^2}{2k_B} \cdot \frac{\kappa}{\kappa - 3/2}$$

Fitting is done by minimising squared residuals in **linear** (not log) phase-space density using a bounded Levenberg–Marquardt algorithm, with the fitting window restricted to the proton core region below the beam saddle point.

---

## Installation

```bash
git clone https://github.com/salastro/swha.git
cd swha
pip install -r requirements.txt
jupyter notebook notebooks/SWHA_Helios2.ipynb
```

### Requirements

```
numpy
scipy
sympy
matplotlib
pandas
seaborn
mpl-scatter-density
```

A full `requirements.txt` is provided. Python 3.9+ is recommended. The notebook was developed and tested on Python 3.11.

### Data

The Helios 2 proton dataset used here is the reprocessed dataset from [Stansby et al. (2018)](https://doi.org/10.1007/s11207-018-1377-3), downloaded automatically via `heliopy`. Magnetic field data are from the Helios E2/E3 fluxgate magnetometers, also available through `heliopy`. No manual data download is required.

---

## References

- **Parker (1958)** — Dynamics of the interplanetary gas and magnetic fields. *ApJ*, 128, 664.
- **Vasyliunas (1968)** — A survey of low-energy electrons in the evening sector of the magnetosphere. *JGR*, 73, 2839.
- **Marsch et al. (1982)** — Solar wind protons: three-dimensional velocity distributions and derived plasma parameters measured between 0.3 and 1 AU. *JGR*, 87, 52.
- **Stansby et al. (2018)** — A new inner heliosphere proton parameter dataset from the Helios mission. *Sol. Phys.*, 293, 155. [[DOI]](https://doi.org/10.1007/s11207-018-1377-3)
- **Stansby et al. (2019)** — Alpha particle thermodynamics in the inner heliosphere fast solar wind. *A&A*, 623, L2. [[DOI]](https://doi.org/10.1051/0004-6361/201834900)
- **Perrone et al. (2019)** — Radial evolution of the solar wind in pure high-speed streams: HELIOS revised observations. *MNRAS*, 483, 3730. [[DOI]](https://doi.org/10.1093/mnras/sty3348)
- **Livadiotis (2015)** — Kappa distribution in the solar wind. *JGR*, 120, 1607.

---

## License

MIT — see [LICENSE](LICENSE).
