# Clinical Report Information Extraction

This project extracts **key medical information** (such as clinical terms, anatomical locations, diagnoses, procedures, and medical codes like ICD-10, CPT, and HCPCS) from **clinical reports in PDF format** using two different approaches.

---

## 📂 Project Overview

The goal is to automate the extraction of structured medical information from **unstructured PDF reports** and export the output as JSON.

The project consists of **two main approaches**:

1. **Approach 1 – Traditional Keyword & Regex Extraction**
2. **Approach 2 – Advanced NER + Hybrid Extraction using spaCy/scispaCy**

Both scripts process the clinical PDF report and produce structured data for easy analysis and storage.

---

## ⚙️ Requirements

Install the required libraries before running the scripts.

```bash
pip install PyMuPDF
pip install spacy
pip install scispacy
pip install https://s3.amazonaws.com/allenai-scispacy/releases/v0.5.0/en_core_sci_lg-0.5.0.tar.gz
```

> **Note**:  
> `en_core_sci_lg` is a large biomedical model from **scispaCy**, suitable for clinical and scientific text analysis.

---

## 📘 Input Files

| File | Description |
|------|--------------|
| `clinicalreport.pdf` | PDF file containing clinical text reports |
| `json.txt` *(used in Approach 2)* | Text file containing JSON reference from Approach 1 (keyword-based extraction) |

## Approach 1 – Traditional Extraction

**File:** `approach1_traditional.py`

This approach uses **keyword matching** and **regular expressions** to extract information from the text.

---

### Steps

1. Extract text from PDF using **fitz (PyMuPDF)**.  
2. Define keyword lists for:
   - Clinical terms  
   - Anatomical locations  
   - Procedures  
3. Use regex to identify:
   - **ICD-10 codes:** `[A-Z]\d{1,2}\.?\d*`  
   - **CPT codes:** `\d{5}`  
   - **HCPCS codes:** `[A-Z]\d{4}`  
4. Create structured JSON output for each report.

---

### 🧾 Output Example

```json
[
  {
    "ReportID": "Report 1",
    "Clinical Terms": ["gastritis", "ulcer"],
    "Anatomical Locations": ["stomach", "duodenum"],
    "Diagnosis": ["ulcer"],
    "Procedures": ["colonoscopy"],
    "ICD-10": ["K29.6"],
    "CPT": ["45380"],
    "HCPCS": []
  }
]
```

## Approach 2 – NER + Hybrid Extraction (scispaCy Model)

**File:** `approach2_hybrid.py`

This approach uses a **Named Entity Recognition (NER)** model (`en_core_sci_lg`) from **scispaCy** to intelligently detect medical entities and combine them with keyword and regex-based extraction.

---

### Steps

1. Load the **scispaCy model** (`en_core_sci_lg`) for biomedical entity detection.  
2. Load a **reference JSON** (from Approach 1) to use as keyword knowledge.  
3. Extract text from PDF and split into individual reports.  
4. Perform:
   - **NER-based entity extraction** for clinical terms  
   - **Keyword-based matching** using reference JSON  
   - **Regex-based pattern matching** for ICD, CPT, and HCPCS codes  
5. Merge all extracted data into a unified JSON output.

---

### 🧾 Output Example

```json
[
  {
    "Report ID": "Report 1",
    "Clinical Terms": ["gastritis", "Barrett's esophagus", "bleeding"],
    "Anatomical Locations": ["duodenum", "esophagus"],
    "Diagnosis": ["ulcer"],
    "Procedures": ["colonoscopy", "biopsy"],
    "ICD-10": ["K22.7", "K29.6"],
    "CPT": ["45380"],
    "HCPCS": []
  }
]
```

## 🔄 Workflow Summary

| Step | Approach 1 | Approach 2 |
|------|-------------|-------------|
| **Text Extraction** | ✅ | ✅ |
| **Keyword Matching** | ✅ | ✅ |
| **Regex for Codes** | ✅ | ✅ |
| **NER (Entity Recognition)** | ❌ | ✅ |
| **Reference Knowledge** | ❌ | ✅ *(Uses JSON from Approach 1)* |
| **Output Format** | JSON | JSON |

---


---

## Improvements & Future Work

- Integrate ICD, CPT, and HCPCS validation via external datasets.  
- Build a web interface to upload and visualize results.  
- Add multilingual medical report support.  
- Enable fine-tuning with domain-specific NER models.  

---

## Summary

| Approach | Type | Strength | Use Case |
|-----------|------|-----------|-----------|
| **Approach 1** | Rule-Based | Simple, fast, interpretable | Basic extraction or small datasets |
| **Approach 2** | Hybrid (NER + Rules) | Intelligent, context-aware, robust | Clinical-grade, large datasets |

---

## Author

**Harshitha T S**  
AI Engineer | Data Science Enthusiast 
