# Seismic Attribute Selection Guide

**Developed by [Dr. Heather Bedle](mailto:hbedle@ou.edu)**  
**Co-authors: [Dr. David Lubo-Robles](mailto:davidlubo@ou.edu), [Dr. April Moreno-Ward](mailto:April.MorenoWard-1@ou.edu)**
AASPI (Attribute Assisted Seismic Processing & Interpretation)  
University of Oklahoma, School of Geosciences  
[aaspi.ou.edu](https://www.ou.edu/mcee/labs/aaspi)

🌐 **[Launch the tool](https://hbedle-subsurface.github.io/attribute-selector/)**

---

## The short version

Walk into any seismic interpretation project and you will find one of two problems. Either someone has computed three attributes — envelope, coherence, and curvature — because those are the ones they always compute. Or someone has computed forty-seven attributes because the software let them, and now they are wondering what to do with all of them.

Neither approach is thoughtful. Both are common.

This tool asks you about your data conditioning, your geologic target, your data quality, and your workflow — then recommends a preprocessing checklist followed by a tiered starting attribute set with plain-language explanations of what each attribute measures and why it is relevant to your specific situation.

One thing this tool emphasizes that is often overlooked: **every geometric attribute — coherence, curvature, aberrancy, GLCM texture, nonparallelism — requires structural dip as input.** Computing these attributes without dip-guided windows produces vertically-smeared results that mix geology from different layers. The tool flags this clearly before recommending any geometric attributes.

---

## Why attribute selection matters

The right attribute set for mapping deepwater turbidite channels is not the same as the right set for carbonate reef detection, basement fault mapping, or DHI identification. Your data quality, your target depth, whether you have pre-stack data, and whether you are feeding the result into machine learning all change what you should compute.

Starting with the wrong attributes — or too many attributes — does not just waste computation time. It actively degrades machine learning results by introducing redundant or irrelevant information, and it makes direct interpretation harder by burying the geologic signal in noise.

**The right order of operations:** start with the geology, let the geology tell you which physical properties matter, then find the attributes that measure those properties.

---

## What it covers

**Section 1 — Your data and preprocessing** (9 questions)

Everything about your data and its conditioning in one place — no jumping between sections to answer related questions.

- Data type — modern 3D (post ~1995), legacy 3D (pre ~1995, narrower azimuth), limited 3D, 2D grid, or sparse 2D. Legacy data triggers specific guidance on footprint severity and reduced curvature reliability.
- Amplitude preservation status
- Pre-stack data availability (gathers or partial stacks)
- Signal-to-noise ratio in the target interval
- Spectral balancing status
- Structural dip computation — with a strong reminder that coherence, curvature, aberrancy, GLCM, and nonparallelism ALL require dip-guided computation as input
- Structure-oriented filtering (SOF) status
- Acquisition footprint status
- Coherent noise (ground roll, multiples, migration artifacts)

**Section 2 — Your geologic target** (4 questions)

Primary targets are organized into six labeled groups for easier scanning:

- *Stratigraphy & Depositional Systems* — seismic facies, channels, deepwater lobes/fans, thin beds
- *Carbonates & Reefs* — carbonate reefs, buildups, and platforms
- *Structural & Fault Systems* — faults/fractures, basement, unconventional reservoirs
- *Fluids & Direct Indicators* — DHI/fluid indicators, gas chimneys/fluid migration
- *Chaotic & Complex Bodies* — mass transport complexes, salt bodies, volcanics
- *Hazards & Near-Surface* — shallow hazards

Secondary targets include fractures, porosity, fluids, thickness, unconformities, deep structure, geomechanics, and salt flanks.

Depositional setting and target resolution relative to tuning thickness round out the section.

**Section 3 — Your workflow** (3 questions)
- Workflow goal (direct interpretation, unsupervised ML, supervised ML, or both)
- Software environment
- Target attribute set size

**Output — preprocessing checklist followed by two-tiered attribute recommendations:**
- **Preprocessing steps needed** — shown before any attribute recommendations, color-coded by urgency. Steps flagged "DO FIRST" (crimson) include missing dip computation, unaddressed footprint, and coherent noise. Steps flagged "RECOMMENDED" (amber) include spectral balancing and SOF. The tool is direct: skipping preprocessing doesn't save time — it means computing attributes from noise.
- **Core set** — compute these first; tailored to your specific target and data. AASPI attributes appear here when they are primary for the target — not segregated into a separate section.
- **Consider adding** — context-dependent additions based on secondary targets and data quality.

AASPI attributes are identified with an **AASPI** badge inline on every card, alongside the exact AASPI program name and output file names. They are integrated into both tiers where appropriate, not treated as an afterthought.

---

## Attributes covered

The tool recommends from a library of 43 attributes spanning:

| Category | Attributes |
|---|---|
| Amplitude | Envelope, RMS amplitude, Sweetness, AVT (Amplitude Volume Transform), Amplitude gradient, Amplitude change, Normalized amplitude |
| Phase | Cosine of instantaneous phase, Instantaneous phase, Wavelet frequency, Reflection character/polarity |
| Frequency / Spectral | Instantaneous frequency, Spectral decomposition voice components (CWT/CMP/max-entropy/VMD), Peak spectral frequency and statistics, Spectral decomp RGB blend, Thin bed decomposition and bandwidth extension, Spectral probe, Low-frequency shadow, Teager-Kaiser energy/variation |
| Geometric | Dip magnitude/azimuth, Similarity/coherence (including multispectral and multi-input), Most-positive/negative curvature (k1/k2), Shape index/curvedness, Mean curvature, Strike and dip curvature (Euler curvature), Aberrancy, Amplitude curvature, Reflector convergence, Long-range reflector continuity, Volumetric curvature, Salt edge detection, Ant tracking/fault probability |
| Texture | GLCM texture attributes, Nonparallelism (dip deviation, gradient deviation, total deviation), Disorder, Chaos, Geostatistical/variogram texture |
| AVO / DHI | Distance Quadrant (DQ) trace, DQ Average and DQ Sum, Theta PX, Isochron/Half Isochron, StickOgram, Near Optimized/Far-Near Optimized, Vp/Vs ratio |
| Impedance | Relative acoustic impedance, Model-based or simultaneous inversion |
| ML-specific | Waveform classification/SOM output, Seismic saliency |

The tool also generates a **preprocessing checklist** covering:

| Step | AASPI Program |
|---|---|
| Spectral balancing | spectral_balance (or internal balancing in spec_cwt / spec_cmp) |
| Structural dip computation | dip3d (GST or discrete semblance algorithm) |
| Dip quality improvement | filter_dip_components |
| Structure-oriented filtering | sof3d |
| Acquisition footprint suppression | kx-ky footprint workflow or CWT footprint workflow |
| Coherent noise suppression | coh_noise_suppression_workflow or filter_spectral_components |

---

## AASPI connection

Many of the attributes in this tool were developed at or are uniquely implemented by AASPI at the University of Oklahoma. These include:

- **Distance Quadrant (DQ) trace and Theta PX** — AVO attributes that provide relative porosity and hydrocarbon pore volume estimates at every seismic sample across all AVO classes without requiring a priori knowledge of petrophysics or wavelets. DQ = √(A²+B²) on the AVO crossplot; Theta PX is the polar angle. Together with Half Isochron as the z-axis, they enable full 3D AVO cluster analysis.
- **Isochron and Half Isochron** — time thickness (peak-to-trough) and half-thickness (peak-to-zero crossing) attributes. Half Isochron distinguishes homogeneous vs. heterogeneous lithology and is the z-axis of AASPI 3D AVO crossplots.
- **StickOgram** — reduces seismic to center-of-peaks, center-of-troughs, and zero crossings from near-stack traces; high-grades data for ML input and reveals initial time-structure on timeslices.
- **Aberrancy** — third derivative of structure, detecting subtle flexures that coherence and curvature miss; particularly valuable for basement-involved faults and fracture corridors.
- **Nonparallelism** — quantifies lateral variation in reflector parallelism via three named output volumes (dip_deviation, gradient_deviation, total_deviation); a powerful discriminator of ordered stratigraphy vs. chaotic facies.
- **Teager-Kaiser energy/variation** — physics-based local energy estimation using a mass-spring model; most effective applied to individual spectral voice components from spec_cwt.
- **Disorder** — volumetric signal-to-noise estimate that maps interpretation confidence across the survey; developed by Al-Dossary (2013), normalized by RMS amplitude to remove amplitude sensitivity.
- **Reflector convergence** — maps the azimuth and magnitude of bed thinning; directly encodes clinoform geometry and progradation direction.
- **Thin bed decomposition (thin_bed_decomposition)** — parameterizes the seismic trace as thin-bed doublet responses; outputs bandwidth-extended seismic and 0°/90°/180°/270° phase components, with the 270° component particularly highlighting thin hydrocarbon-charged reservoirs.
- **Spectral decomposition suite** — spec_cwt (continuous wavelet transform), spec_cmp (complex matching pursuit, highest resolution), spec_max_entropy (improved low-frequency temporal resolution), spec_vmd (variational mode decomposition). All output per-frequency voice volumes and statistical spectral summaries.

The tool works with any seismic interpretation software. Every attribute card displays the exact AASPI program name and, where applicable, the specific output file names so results can be loaded directly.

More information: [aaspi.ou.edu](https://www.ou.edu/mcee/labs/aaspi)

---

## Who it is for

- **Geoscientists** starting a new seismic interpretation project who want a principled starting point for attribute selection
- **AASPI students** learning which attributes are appropriate for which geologic problems and how the programs chain together
- **ML practitioners** building multi-attribute seismic classification workflows who need a defensible, non-redundant input set
- **Anyone** who has ever opened the attribute library in their interpretation software and felt slightly overwhelmed

No attribute expertise required. Every recommendation includes a plain-language explanation of what the attribute measures, why it is relevant to your target, any data quality warnings, ML-specific notes where applicable, and the exact AASPI program to run.

---

## How to use it

Visit the link above. The tool runs entirely in your browser — nothing is stored or transmitted anywhere.

Work through three short sections of questions (~5 minutes). At the end you receive a preprocessing checklist followed by a two-tiered attribute recommendation with:
- Plain-language descriptions of what each attribute measures
- Warnings where data quality may limit an attribute's reliability
- ML-specific notes where workflow is ML-oriented
- Exact AASPI program names and output file names for every attribute
- An **AASPI** badge on every card that requires AASPI software

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

Version 1.4 covers 14 primary geologic targets, 43 attributes, and 6 depositional settings. All AASPI program names and output file references have been verified against the official AASPI software documentation (geometric attributes, single trace calculations, spectral attributes, and DQ workflow).

Planned additions for future versions include:

- AVO gradient and intercept attributes
- Azimuthal anisotropy attributes for fracture characterization (azimuthal coherence, azimuthal amplitude gradient)
- Pre-stack spectral decomposition
- Additional play types (geothermal, CCS/sequestration, mining)

If you find a target or data situation the tool handles poorly, have suggestions for attributes to add, or want to contribute geologic use cases - please get in touch.

**Contact:** [aaspi@ou.edu](mailto:aaspi@ou.edu)  
Feedback, suggestions, and war stories all welcome.

---

## Citation

If you use this tool in research, teaching, or a publication, please cite it as:

> Bedle, H., Lubo-Robles, D., and Moreno-Ward, A. (2025). *Seismic Attribute Selection Guide*, v1.4. AASPI, University of Oklahoma. https://hbedle-subsurface.github.io/attribute-selector/

---

## License

This work is licensed under [Creative Commons Attribution ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

You are free to share and adapt this tool provided you give appropriate credit and distribute any adaptations under the same license.

---

## Acknowledgments

Developed at AASPI (Attribute Assisted Seismic Processing and Interpretation), University of Oklahoma, School of Geosciences. Built on the AASPI attribute library developed over more than a decade of research by the AASPI consortium and its sponsors.
