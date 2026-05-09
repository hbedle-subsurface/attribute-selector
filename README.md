# Seismic Attribute Selection Guide

**Developed by [Dr. Heather Bedle](mailto:hbedle@ou.edu)**  
AASPI (Attribute Assisted Seismic Processing & Interpretation)  
University of Oklahoma, School of Geosciences  
[aaspi.ou.edu](https://www.ou.edu/mcee/labs/aaspi)

 **[Launch the tool](https://hbedle-subsurface.github.io/attribute-selector/)**

---

## The short version

Walk into any seismic interpretation project and you will find one of two problems. Either someone has computed three attributes — envelope, coherence, and curvature — because those are the ones they always compute. Or someone has computed forty-seven attributes because the software let them, and now they are wondering what to do with all of them.

Neither approach is thoughtful. Both are common.

This tool asks you about your geologic target, your data quality, and your workflow — then recommends a tiered starting attribute set with plain-language explanations of what each attribute measures and why it is relevant to your specific situation.

---

## Why attribute selection matters

The right attribute set for mapping deepwater turbidite channels is not the same as the right set for carbonate reef detection, basement fault mapping, or DHI identification. Your data quality, your target depth, whether you have pre-stack data, and whether you are feeding the result into machine learning all change what you should compute.

Starting with the wrong attributes — or too many attributes — does not just waste computation time. It actively degrades machine learning results by introducing redundant or irrelevant information, and it makes direct interpretation harder by burying the geologic signal in noise.

**The right order of operations:** start with the geology, let the geology tell you which physical properties matter, then find the attributes that measure those properties.

---

## What it covers

**Section 1 — Geologic target**
- Primary interpretation target (seismic facies, channels, reefs, faults, DHI, MTCs, thin beds, basement)
- Secondary targets (fractures, porosity, fluids, thickness, unconformities)
- Depositional setting
- Target resolution relative to tuning thickness

**Section 2 — Your seismic data**
- Data dimensionality (3D, 2D)
- Amplitude preservation status
- Pre-stack data availability
- Signal-to-noise ratio
- Acquisition footprint
- Structure-oriented filtering status

**Section 3 — Your workflow**
- Workflow goal (direct interpretation, unsupervised ML, supervised ML, or both)
- Software environment
- Target attribute set size

**Output — three-tiered recommendations:**
- **Core set** — compute these first; tailored to your specific target and data
- **Consider adding** — context-dependent additions based on secondary targets and data quality
- **AASPI-specific attributes** — capabilities not widely available in standard platforms, flagged separately

---

## Attributes covered

The tool recommends from a library of attributes spanning:

| Category | Attributes |
|---|---|
| Amplitude | Envelope, RMS amplitude, Sweetness, AVT (Amplitude Volume Transform) |
| Phase | Cosine of instantaneous phase, Wavelet frequency |
| Frequency | Instantaneous frequency, Spectral decomposition (CWT/CMP/max-entropy), Peak frequency, Teager-Kaiser energy/variation |
| Geometric | Dip magnitude/azimuth, Similarity/coherence, Most-positive/negative curvature (k1/k2), Shape index/curvedness, Aberrancy, Amplitude curvature, Reflector convergence |
| Texture | GLCM texture attributes, Nonparallelism, Disorder |
| AVO / DHI | Distance Quadrant (DQ) trace, Theta PX, Isochron/Half Isochron, StickOgram |
| Impedance | Relative acoustic impedance |

---

## AASPI connection

Many of the attributes in this tool were developed at or are uniquely implemented by AASPI at the University of Oklahoma. These include:

- **Distance Quadrant (DQ) trace and Theta PX** — novel AVO attributes that provide relative porosity and hydrocarbon pore volume estimates at every seismic sample across all AVO classes without requiring a priori knowledge of petrophysics or wavelets
- **Aberrancy** — third derivative of structure, detecting subtle flexures that coherence and curvature miss; particularly valuable for basement-involved faults
- **Nonparallelism** — quantifies lateral variation in reflector parallelism; a powerful discriminator of ordered stratigraphy vs. chaotic facies
- **Teager-Kaiser energy/variation** — physics-based local energy estimation using a mass-spring model; most effective applied to spectral voice components
- **Disorder** — volumetric signal-to-noise estimate that maps interpretation confidence across the survey
- **StickOgram** — reduces seismic to center-of-peaks, center-of-troughs, and zero crossings; high-grades data for ML input

The tool works with any seismic interpretation software. AASPI-specific attributes are flagged clearly so users know what is available in the AASPI environment versus what is available in standard commercial platforms.

More information: [aaspi.ou.edu](https://www.ou.edu/mcee/labs/aaspi)

---

## Who it is for

- **Geoscientists** starting a new seismic interpretation project who want a principled starting point for attribute selection
- **AASPI students** learning which attributes are appropriate for which geologic problems
- **ML practitioners** building multi-attribute seismic classification workflows who need a defensible, non-redundant input set
- **Anyone** who has ever opened the attribute library in their interpretation software and felt slightly overwhelmed

No attribute expertise required. Every recommendation includes a plain-language explanation of what the attribute measures and why it is relevant to your situation.

---

## How to use it

Visit the link above. The tool runs entirely in your browser — nothing is stored or transmitted anywhere.

Work through three short sections of questions (~5 minutes). At the end you receive a three-tiered attribute recommendation with:
- Plain-language descriptions of what each attribute measures
- Specific notes on why each attribute is relevant to your target
- Warnings where data quality may limit an attribute's reliability
- ML-specific notes where workflow is ML-oriented
- AASPI program names for every attribute

Print or save the results as a PDF to document your attribute selection rationale.

---

## Companion tool

This tool is designed to be used alongside the **AASPI Seismic ML Uncertainty Assessment** — a structured questionnaire that helps geoscientists critically evaluate the uncertainty in their ML-based seismic interpretation results.

🌐 [Seismic ML Uncertainty Assessment](https://hbedle-subsurface.github.io/seismic-ml-assessment/)

**Recommended workflow:**
1. Use this tool to select a thoughtful, defensible starting attribute set
2. Compute and QC the attributes
3. Run your ML workflow
4. Use the uncertainty assessment tool to evaluate the result before using it for decisions

---

## This is a living document

Version 1.0 covers the most commonly used attribute categories. Planned additions include:

- AVO gradient and intercept attributes
- Azimuthal anisotropy attributes for fracture characterization
- Structure-oriented filtering guidance
- Spectral balancing recommendations
- Additional geologic settings and play types

If you find a target or data situation the tool handles poorly, have suggestions for attributes to add, or want to contribute geologic use cases — please get in touch.

**Contact:** [hbedle@ou.edu](mailto:hbedle@ou.edu)  
Feedback, suggestions, and war stories all welcome.

---

## Citation

If you use this tool in research, teaching, or a publication, please cite it as:

> Bedle, H. (2025). *Seismic Attribute Selection Guide*, v1.0. AASPI, University of Oklahoma. https://hbedle-subsurface.github.io/seismic-attribute-selector/

---

## License

This work is licensed under [Creative Commons Attribution ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

You are free to share and adapt this tool provided you give appropriate credit and distribute any adaptations under the same license.

---

## Acknowledgments

Developed at AASPI (Attribute Assisted Seismic Processing and Interpretation), University of Oklahoma, School of Geosciences. Built on the AASPI attribute library developed over more than a decade of research by the AASPI consortium and its sponsors.
