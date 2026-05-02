![Python](https://img.shields.io/badge/Python-3.8+-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![Streamlit](https://img.shields.io/badge/Streamlit-app-red)
![License](https://img.shields.io/badge/License-MIT-green)

# AlzIA: Web Application for Dementia Prediction Using EEG
 
## Description
AlzIA is a web application built with Streamlit that uses a multi-task deep learning model (based on TensorFlow/Keras) to predict the dementia group (e.g., Control, Alzheimer, etc.) and the MMSE score from EEG data. It supports loading EEG files in .set (EEGLAB) or CSV format with pre-extracted features, automatic preprocessing, spectral feature extraction (absolute/relative powers, entropy, band ratios), predictions with confidence intervals via bootstrap resampling, and PDF report generation.
 
This app is designed to be a precise and efficient prototype, with an emphasis on modularity, error handling, and logging for debugging. Based on an original script (`alz_ia_v3.py`), it focuses on medical/engineering accuracy, but is not a substitute for professional clinical diagnosis.
 
## Objectives
- Provide an intuitive interface for loading and analyzing EEG/CSV data.
- Perform multi-task predictions: dementia group classification and MMSE score regression.
- Incorporate confidence metrics (e.g., intervals via bootstrap resampling).
- Generate downloadable PDF reports with results, inputs, and visualizations.
- Ensure compatibility with standard EEG channels (10-20 system, minimum 19 channels).
- Maintain modular and extensible code for future expansions (e.g., more features or models).
## Main Features
- **File Loading**: Support for .set (EEGLAB) or .csv (pre-extracted features). Automatic channel and format validation.
- **Additional Inputs**: Age, gender, subject name (encoded with LabelEncoder).
- **Preprocessing & Features**: Bandpass filtering, PSD extraction (Welch), powers per band (delta, theta, alpha, beta, gamma), entropy, ratios (e.g., theta/alpha).
- **Prediction**: Multi-task DNN model loaded from `assets/`. Bootstrap predictions for robustness.
- **Results**: In-app visualization (group, MMSE, CI, metrics) and PDF download with tables and graphs.
- **Security & Cleanup**: Temporary file handling, automatic post-processing deletion.
- **Logging**: Detailed records for debugging.
## Technical Specifications
- **Main Framework**: Streamlit for the web UI.
- **Model**: Multi-task DNN (classification + regression), loaded from `dementia_model.keras`.
- **Dependencies**: See `requirements.txt` (includes TensorFlow, MNE, Pandas, NumPy, Matplotlib, Scikit-learn, Joblib, ReportLab).
- **Supported Formats**: EEG .set with 10-20 channels (e.g., Fp1, Cz); CSV with columns matching `feature_names.json`.
- **Environment**: Python 3.8+; recommended to use venv/conda. GPU not supported by default (adjust TensorFlow if needed).
- **Limitations**: Assumes sfreq=250 Hz for EEG; does not handle advanced artifacts (e.g., ICA); for investigative use only, not clinical.

## Installation
 
```bash
git clone https://github.com/JOMO67/alz_ai_v3
cd alz_ai_v3
 
# On Linux/Mac:
python -m venv venv && source venv/bin/activate
 
# On Windows:
python -m venv venv
venv\Scripts\activate
 
pip install -r requirements.txt
streamlit run app.py
```
 
<img width="1908" height="915" alt="web" src="https://github.com/user-attachments/assets/f069eb6a-c77f-45ba-be45-bcf842221362" />
## Usage
 
1. Launch the app with `streamlit run app.py`
2. Open your browser at `http://localhost:8501`
3. Upload your EEG file in `.set` (EEGLAB) or `.csv` format with pre-extracted features
4. Enter the subject data: age, gender, and name
5. Click **Predict** and wait for the analysis
6. Review the results: predicted dementia group, estimated MMSE score, and confidence intervals
7. Download the PDF report with all results and visualizations
### Accepted input formats
 
**EEG .set file (raw):**
- 10-20 electrode system, minimum 19 channels (Fp1, Cz, etc.)
- Sampling frequency: 250 Hz
**CSV file (pre-extracted features):**
- Columns must match `assets/feature_names.json`
## Results
 
The model was evaluated using bootstrap resampling (95% CI) on the test set.
 
### Classification (dementia group)
| Metric | Value | 95% CI |
|---|---|---|
| Accuracy | ~95.8% | [95.5%, 96.2%] |
 
<img width="750" height="500" alt="Training curves" src="https://github.com/user-attachments/assets/1a93c834-6ba8-49cf-a8e4-9b6b371ff4fe" />

### Regression (MMSE score)
| Metric | Value | 95% CI |
|---|---|---|
| R² | 0.895 | [0.880, 0.905] |
| MAE | 1.40 | [1.37, 1.43] |
| RMSE | 2.01 | [1.95, 2.10] |
 
Training curves show stable convergence without overfitting in both tasks.
 
<img width="750" height="500" alt="Bootstrap metrics" src="https://github.com/user-attachments/assets/142d5f3f-5a95-4a4d-ae3c-09d41a788f48" />
