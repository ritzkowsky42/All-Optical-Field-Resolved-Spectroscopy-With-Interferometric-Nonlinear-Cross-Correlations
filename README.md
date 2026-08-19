# All-Optical Field-Resolved Spectroscopy With Interferometric Nonlinear Cross-Correlations

Felix Ritzkowsky<sup>1,2,\*</sup>, Gian Luca Dolso<sup>1</sup>, Benjamin M. Mazur<sup>1</sup>, Matthew Yeung<sup>1</sup>, and Phillip D. Keathley<sup>1,\*</sup>

<sup>1</sup> Massachusetts Institute of Technology, 77 Massachusetts Avenue, Cambridge MA 02139, USA
<sup>2</sup> Deutsches Elektronen-Synchrotron (DESY) and Center for Free-Electron Laser Science (CFEL), Notkestraße 85, 22607 Hamburg, Germany

<sup>\*</sup> Corresponding authors. pdkeat2@mit.edu, felix.ritzkowsky@desy.de

Preprint: [arXiv:2608.04493](https://arxiv.org/abs/2608.04493)

This repository provides the analysis scripts and measured data needed to
reproduce the results presented in the manuscript.

## Contents

The notebooks follow the path of the data, from raw oscilloscope records to the
figures in the manuscript.

| Notebook | What it does |
| --- | --- |
| [`SI_AnalysisPipeline_550SPNoMaterial.ipynb`](SI_AnalysisPipeline_550SPNoMaterial.ipynb) | Supplementary Information: the complete processing pipeline that turns the raw interferometric scans into coherently averaged, delay-calibrated complex fields. The raw CSV scans are several GB and are available from the corresponding authors on request. |
| [`field_retrieval_analysis.ipynb`](field_retrieval_analysis.ipynb) | The retrieved electric fields and their validation against independent references. |
| [`fid_analysis.ipynb`](fid_analysis.ipynb) | The free induction decay of ambient H₂O, and a causal Kramers-Kronig model of it built from HITRAN line data. |

The last two run directly on the data included here.

### `field_retrieval_analysis.ipynb`

| Figure | Content |
| --- | --- |
| `dscan_field_signal_gate` | Temporal intensity of the D-Scan-retrieved signal and gate pulses |
| `Gate_magnitude` | Effective detection transfer function \|H(ω)\|² and phase for N = 3 and N = 5 |
| `intensity_comparison_dscan_n3_n5` | Retrieved temporal intensity, N = 3 vs N = 5 vs D-Scan |
| `comparison_combined_time_spec_phase_triplet_0` | Retrieved fields (a, c), spectra against the OSA reference (b) and spectral phase (d) |
| `stft_window_schedule` | Time-dependent STFT window length used for the spectrograms |
| `spectrogram_compar_n3_n5` | Spectrograms of the retrieved fields for N = 3 and N = 5, next to the detection bandwidth |

### `fid_analysis.ipynb`

| Figure | Content |
| --- | --- |
| `fid_composite_h2o` | HITRAN H₂O absorption and the spectral comparison of prompt / measured tail / KK model (a), and the band-resolved FID in the time domain at 2.7 µm and 1.9 µm (b–d) |

The model's only length parameter, the effective path length `L_eff ≈ 2.9 cm`, is
fixed *a priori* from the lab humidity logs — nothing is fitted to the measured FID.

## Getting started

```bash
pip install -r requirements.txt
jupyter lab
```

Run the cells of either notebook top to bottom. Figures are written to `figures/`
as PDF and PNG.

`scienceplots` is optional — it only supplies the manuscript figure style, and the
notebooks fall back to the matplotlib defaults if it is not installed. No network
access is needed: the HITRAN absorption coefficients are shipped precomputed, so
neither HAPI nor a HITRAN API key is required to run the analysis.

## Data

All measured data is in `data/field_retrieval_data.h5` (HDF5, ~5 MB):

| Group | Datasets | Description |
| --- | --- | --- |
| `dscan/{signal,gate}/` | `time` (s), `field_complex` | D-Scan retrieval of the pulse under test and of the gate pulse |
| `electric_field_traces/<method>/<triplet_i>/` | `time` (s), `up_field_centered_filtered` | Electric fields retrieved from the interferometric nonlinear cross-correlation |
| `osa/<trace>/` | `wavelength_nm`, `power_dBm` | Optical spectrum analyser traces |
| `spectrometer/` | `wavelength` (m), `intensity` | Fundamental spectrum of the driving pulse |
| `hitran/h2o/` | `wavelength_um`, `alpha_per_cm` | H₂O absorption coefficient, Voigt profiles at 296 K / 1 atm, computed from HITRAN line data with HAPI |

Detection methods: `method1_550` is the fifth-order (N = 5) cross-correlation,
`method3_thg` the third-order (N = 3) one, and `method2_antenna` an antenna
reference. Each holds three independent measurement triplets; the manuscript
figures use `triplet_0`.

OSA traces: `20250610_SIGCLIP` is the reference spectrum used in the manuscript,
and `noise_floor` is the instrument detection floor (Yokogawa AQ6375, 2 nm
resolution, 10 averages).

HITRAN α is computed for **pure H₂O** at the stated pressure — HAPI scales the line
strengths by the total number density — so the laboratory absorption is `x_H2O · α`.
The FID notebook absorbs the mixing ratio into the path length,
`L_eff = x_H2O · L_geo`; see the notebook for the derivation. The line data was
processed with HAPI:

> R. V. Kochanov, I. E. Gordon, L. S. Rothman, P. Wcisło, C. Hill, J. S. Wilzewski,
> *HITRAN Application Programming Interface (HAPI): A comprehensive approach to
> working with spectroscopic data*, JQSRT **177**, 15–30 (2016).

Alongside the HDF5 file:

- `data/dscan_signal.txt`, `data/dscan_gate.txt` — the original D-Scan text exports
  (`time [fs], Re(E), Im(E), normalized intensity`) from which the `dscan/` group
  was built, included for provenance.
- `data/ambient/room_{temperature,humidity,pressure}.csv` — lab monitoring logs,
  from which the effective path length of the FID model is derived.

Poke around the HDF5 file with:

```python
import h5py
with h5py.File('data/field_retrieval_data.h5') as f:
    f.visititems(lambda name, obj: print(name, getattr(obj, 'shape', '')))
```

## Acknowledgement

Parts of the analysis code in this repository were written with the assistance of
[Claude Code](https://claude.com/claude-code) (Anthropic) under human supervision.
All processing steps, parameters and results were reviewed and validated by the
authors, who take full responsibility for the content.

## License

MIT, see [LICENSE](LICENSE).
