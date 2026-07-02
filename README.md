# SAP-PO-Creation-Automation-using-Uipath-RPA-


An enterprise-grade **UiPath RPA Workflow** designed to fully automate the end-to-end process of creating Purchase Orders (PO) in **SAP ERP** using structured data from **Excel**. 

This repository contains the complete automation pipeline, including environmental configuration steps, dynamic selector stabilization, transactional error-handling loops, and post-processing output extraction.

---

## 🗺️ Detailed Workflow Architecture

The automation is built linearly across 5 distinct execution stages, maximizing modularity and maintainability:

```text
[Stage 1: Pre-requisites] ──> [Stage 2: Authentication] ──> [Stage 3: Transaction Input]
                                                                        │
[Stage 5: Termination]   <──  [Stage 4: Post-Processing] <──────────────┘

## SAP PO Creation UiPath workflow 
```

[PO Creation Diagram](/artifacts/PO%20Creation%20Workflow%20Diagram.png)