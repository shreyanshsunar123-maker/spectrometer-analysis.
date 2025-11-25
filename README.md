This repository contains a complete data-processing pipeline for analyzing emission spectra obtained from Laser-Induced Breakdown Spectroscopy (LIBS).
The objective of this pipeline is to transform raw spectral intensity measurements into clean, baseline-corrected, peak-identified spectra, and finally match detected peaks to known atomic emission lines.
The workflow is designed for research environments where multiple spectra must be averaged, smoothed, corrected, and interpreted reliably.


Overview
Laser-Induced Breakdown Spectroscopy produces spectra consisting of:
•	sharp atomic emission lines,
•	superimposed on a large, slowly-varying plasma continuum,
•	with additional noise from detectors and background light.

This pipeline performs the following steps:
	1.	Load multiple raw spectra from CSV files.
	2.	Interpolate all spectra onto a unified master wavelength axis.
	3.	Average all spectra to enhance signal-to-noise ratio.
	4.	Apply Savitzky–Golay smoothing to reduce high-frequency noise.
	5.	Estimate and subtract a median-filtered baseline.
	6.	Detect spectral peaks using scipy.signal.find_peaks.
	7.	Match detected peaks to known atomic emission wavelengths from a reference table.
	8.	Report identified elements in the sample.



Features
	•	Supports any number of input spectra.
	•	Automatically aligns spectra of different lengths or wavelength steps.
	•	Fully vectorized using NumPy for performance.
	•	Configurable smoothing and baseline parameters.
	•	Robust peak detection with intensity and distance thresholds.
	•	Referencing against a customizable emission line database (NIST-style table).



Data Requirements
Each input CSV file must contain two columns: wavelength, intensity
with:
•	wavelength in nanometers
•	intensity in arbitrary units

Place all files inside a directory named: Datas/
Structure Example:
Spectrometer Analysis/
│── min.py
│── specline.csv
│── Datas/
│     ├── data1.csv
│     ├── data2.csv
│     ├── data3.csv
│     └── data4.csv



Reference Line Database
The specline.csv file contains known spectral transitions with the format:
element,ion,wavelength
Fe,I,248.327
Mg,II,279.553
Na,I,589.592
...

This allows the script to identify elements present in the sample by comparing detected peak wavelengths to tabulated atomic transitions.



Method Summary

Interpolation
All spectra are mapped onto the wavelength axis of the first file to ensure uniform point-to-point comparison.

Averaging
Stacked spectra are averaged along the intensity dimension, improving signal clarity while suppressing random noise.

Smoothing
A Savitzky–Golay filter (51-point window, 3rd-order polynomial) is applied to suppress high-frequency fluctuations without distorting peak shape.

Baseline Correction
A broad median filter removes the slow plasma continuum. Subtracting this baseline isolates true emission peaks.

Peak Detection
Peaks are extracted based on:
•	minimum height,
•	minimum separation,
•	corrected intensity.

Spectral Identification
Each detected peak is compared to reference wavelengths. If the difference is below the tolerance (default: 0.3 nm), the corresponding element and ionization state are reported.



Running the Analysis
Run: python3 min.py

The script automatically:
•	processes all spectra in the dataset,
•	produces intermediate and final corrected spectra,
•	prints detected elements with wavelengths.

Dependencies
	•	Python 3.8+
	•	NumPy
	•	Pandas
	•	Matplotlib
	•	SciPy

Install via: pip install numpy pandas matplotlib scipy

Applications
This analysis pipeline is suitable for:
	•	LIBS elemental analysis
	•	Plasma diagnostics
	•	Combustion studies
	•	Emission spectroscopy
	•	Academic laboratory work
	•	Automated material identification


License
This project is open for academic and research use.
