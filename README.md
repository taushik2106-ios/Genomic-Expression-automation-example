# TCGA-BRCA Molecular Subtype Classifier

**Author:** Taushik  
**Organization:** Genomic Expression (Internship Project)  
**Date:** May 2026

---

## 🧬 Overview

This Python script automates the classification of breast cancer patients
from the TCGA-BRCA dataset into three clinically meaningful molecular subtypes
based on receptor status from the `molecular_tests` annotations in the GDC
clinical JSON file.

Previously this was done manually (3 patients at a time). This script
processes the **entire TCGA-BRCA cohort (~1,085 female patients)** in seconds.

---

## 🎯 Molecular Subtypes

| Subtype | Definition | Clinical Significance |
|---|---|---|
| **HR+** | ESR1 Positive OR PGR Positive (ERBB2 Negative) | Treated with hormone therapy (Tamoxifen, Aromatase inhibitors) |
| **HER2+** | ERBB2 Positive (any ER/PR status) | Treated with HER2-targeted therapy (Trastuzumab) |
| **TNBC** | ESR1 Negative AND PGR Negative AND ERBB2 Negative | Treated with chemotherapy only — most aggressive subtype |

> Gene names: `ESR1` = ER (Estrogen Receptor), `PGR` = PR (Progesterone Receptor), `ERBB2` = HER2

---

## 📁 Repository Structure

```
tcga_brca_subtype_classifier/
│
├── tcga_brca_classifier.py   # Main classifier script
├── requirements.txt          # Python dependencies
├── README.md                 # This file
│
└── output/                   # Auto-generated on run
    ├── per_patient_subtypes.csv
    ├── subtype_summary.csv
    ├── stage_subtype_summary.csv
    ├── age_subtype_summary.csv
    └── TCGA_BRCA_Subtype_Report.xlsx
```

---

## ⚙️ Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/tcga_brca_subtype_classifier.git
cd tcga_brca_subtype_classifier

# Install dependencies
pip install -r requirements.txt
```

---

## 🚀 Usage

```bash
python tcga_brca_classifier.py --input clinical_project-tcga-brca.json
```

With custom output folder:
```bash
python tcga_brca_classifier.py --input clinical_project-tcga-brca.json --output results/
```

---

## 📊 Output Files

| File | Description |
|---|---|
| `per_patient_subtypes.csv` | One row per female patient with Patient ID, Age, Stage, ESR1, PGR, ERBB2, Subtype |
| `subtype_summary.csv` | Count and % for HR+, HER2+, TNBC, Unclassified |
| `stage_subtype_summary.csv` | Stage I–IV × Subtype crosstab |
| `age_subtype_summary.csv` | Age group × Subtype crosstab |
| `TCGA_BRCA_Subtype_Report.xlsx` | Formatted Excel report with all 4 sheets colour-coded |

---

## 🔬 Classification Logic

```python
def classify_subtype(esr1, pgr, erbb2):
    if erbb2 == "Positive":
        return "HER2+"
    elif esr1 == "Positive" or pgr == "Positive":
        return "HR+"
    elif esr1 == "Negative" and pgr == "Negative" and erbb2 == "Negative":
        return "TNBC"
    else:
        return "Unclassified"  # marker data not reported
```

---

## 📋 Example Output

### per_patient_subtypes.csv
```
patient_id,age_at_diagnosis,age_group,race,vital_status,stage,ESR1,PGR,ERBB2,molecular_subtype
TCGA-BH-A18V,45,40-49,white,Alive,Stage II,Negative,Negative,Negative,TNBC
TCGA-AR-A0TX,52,50-59,white,Alive,Stage I,Positive,Positive,Positive,HER2+
TCGA-BH-A1FB,61,60-69,black or african american,Dead,Stage II,Positive,Positive,Negative,HR+
```

### subtype_summary.csv
```
Subtype,Count,% of Cohort
HR+,563,51.9%
HER2+,174,16.0%
TNBC,190,17.5%
Unclassified,158,14.6%
TOTAL,1085,100.0%
```

---

## 🔮 Next Steps (Planned)

- [ ] Integrate GTEx normal breast tissue samples for comparison
- [ ] Add Stage 3+ filtering flag (`--stage-filter 3`)
- [ ] Connect to secondary pipeline input format
- [ ] Add visualization (subtype pie chart, age distribution bar chart)
- [ ] Extend to GTEx normal blood samples

---

## 📬 Contact

**Genomic Expression**  
Beverly, MA  
info@genomicexpression.com  
www.genomicexpression.com
