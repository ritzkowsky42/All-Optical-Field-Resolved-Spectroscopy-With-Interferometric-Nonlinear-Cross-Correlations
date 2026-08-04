# All-Optical Field-Resolved Spectroscopy With Interferometric Nonlinear Cross-Correlations

Analysis code accompanying the manuscript of the same name.

> **Status — stand-in repository.** This README is currently the only content. The
> analysis scripts, processed data, and figure-generation code are still to be added.

## Manuscript

**All-Optical Field-Resolved Spectroscopy With Interferometric Nonlinear Cross-Correlations**

Felix Ritzkowsky<sup>1,2,\*</sup>, Gian Luca Dolso<sup>1</sup>, Benjamin M. Mazur<sup>1</sup>, Matthew Yeung<sup>1</sup>, and Phillip D. Keathley<sup>1,\*</sup>

<sup>1</sup> Massachusetts Institute of Technology, 77 Massachusetts Avenue, Cambridge MA 02139, USA
<sup>2</sup> Deutsches Elektronen-Synchrotron (DESY) and Center for Free-Electron Laser Science (CFEL), Notkestraße 85, 22607 Hamburg, Germany

<sup>\*</sup> Corresponding authors. pdkeat2@mit.edu, felix.ritzkowsky@desy.de

*Journal and year to be filled in on submission.*

## Purpose

This repository exists so that every figure in the manuscript and its supplementary
information can be regenerated from the underlying data by running a script. The
intent is full computational reproducibility of the reported results, covering the
raw interferometric cross-correlation traces through to the final field-resolved
spectra and free-induction-decay analysis.

Scope of what will live here.

- Processing of raw interferometric nonlinear cross-correlation (ICC) scans
- Spectral windowing, filtering, and continuum-floor handling
- Gate-kernel and transfer-function calculations
- Free-induction-decay analysis and the HITRAN comparison
- Spurious-free dynamic range (SFDR) and the mixer-theory benchmarking metrics
- Photodiode spectral-response error simulation
- One script per manuscript figure, producing the exact PDF used in the paper

## Planned contents

Figure assets as they appear in the manuscript source, each to be paired with the
script that generates it.

### Main text

| Figure | Label | Asset | Content |
|---|---|---|---|
| 1 | `fig:overview` | `OverviewV4.pdf` | ICC concept, thin-film ITO geometry, effective temporal gate |
| 2 | `fig:data` | `comparison_combined_time_spec_phase_triplet_0.pdf` | Time, spectrum, and phase comparison |
| 3 | `fig:spectrogram` | `spectrogram_compar_n3_n5.pdf` | Time-frequency analysis, n=3 against n=5 |
| 4 | `fig:fid_analysis` | `combined_figure.pdf` | Free-induction decay of the water ro-vibrational bands |

### Methods

| Label | Asset | Content |
|---|---|---|
| `fig:setup` | `interferometric_setup.pdf` | Interferometer schematic |
| `fig:transfer` | `Gate_magnitude.pdf` | Gate-kernel magnitude |
| `fig:dscan` | `dscan_field_spectra.pdf` | d-scan retrieved gate field and spectra |
| `fig:comparison` | `method_comparison_intensity.pdf` | Method intensity comparison |
| `fig:SuppFID` | `TiO2_pos4_spectral_comparison.pdf` | Spectral comparison |

### Supplementary information

| Label | Asset | Content |
|---|---|---|
| `fig:S1_raw` | `S1_raw_interferogram.pdf` | Raw interferogram |
| `fig:S3_freq_filter` | `S3_scan_filter.pdf` | Frequency-domain scan filter |
| `fig:S3_freq_filter_optical` | `S8_optical_filter.pdf` | Optical filter response |
| `fig:PD_Resp` | `pd_respons_figs_SI/TL_DET36A_Resp_Trunc.pdf` | Thorlabs DET36A responsivity, truncated to spec limits |
| `fig:FiltSim_n3` | `pd_respons_figs_SI/PD_resp_error_n3.pdf` | Photodiode-response error simulation, n=3 |
| `fig:FiltSim_n5` | `pd_respons_figs_SI/PD_resp_error_n5.pdf` | Photodiode-response error simulation, n=5 |

## Repository layout (planned)

```
.
├── README.md
├── data/               # processed measurement data, or a fetch script if too large for git
├── src/                # shared processing library (ICC pipeline, retrieval, metrics)
├── figures/            # one script per manuscript figure
├── notebooks/          # exploratory analysis, not required for reproduction
└── environment.yml     # pinned dependencies
```

## Reproducing the figures

To be written once the scripts land. The target is a single entry point that
regenerates every figure from the data directory without manual intervention.

## Data availability

To be decided. Raw scan data may exceed what is sensible to keep in git, in which
case this repository will carry the processing code plus a fetch script pointing at
an archived dataset with a DOI.

## Citation

To be added on acceptance.

## License

Not yet chosen. This needs deciding before the repository is made public, since code
released without a license is not legally reusable.
