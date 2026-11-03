# AILeatherInspector

**AI-assisted, context-aware quality inspection of automotive leather.**

AILeatherInspector turns leather defect detection into a quality decision that depends on the context and can be traced: each piece gets **PASS**, **REWORK** or **REJECT**, with the reason behind it.

<p align="center">
  <img src="media/demo-preview.gif" alt="Short preview of AILeatherInspector: 3D inspection result, contextual decision, 3D wrinkle studio and quality dashboards" width="760">
</p>

▶️ **Demo video (1 min 43 s):** [`media/AILeatherInspector-demo.mp4`](media/AILeatherInspector-demo.mp4)

> This repository is a **showcase** of a project I built during my internship at **DRÄXLMAIER**. It contains screenshots, a demo video and a description of the project. The source code, the data and the trained models are not published.

---

## The problem

Automotive leather is inspected by eye. The verdict depends on the **customer**, the **leather type**, the **part** and the **inspection zone**: the same mark can be acceptable on a hidden area and unacceptable on a highly visible one.

That makes manual inspection:

- **subjective**: two inspectors can judge the same borderline mark differently;
- **hard to reproduce**: the criteria are complex and applied from experience, under fatigue and changing light;
- **hard to trace**: there is little structured history of what was found, where, how big it was, and why a piece was rejected.

## The idea

I designed and developed **AILeatherInspector**, an AI-assisted inspection system that keeps the inspector in the loop and makes the decision **objective, explainable and traceable**.

The AI does more than say "defect / no defect". It **measures** each defect. Then a **quality rule engine** decides whether that defect is acceptable *for this customer, this leather, this part and this zone*. Every decision is stored, can be explained and corrected by a human, and the corrections are used to improve the models.

```mermaid
flowchart LR
    A[Photo or camera capture] --> B[Localization]
    B --> C[Classification<br/>OK / defective]
    C --> D[Defect detection]
    D --> E[Segmentation]
    E --> F[Measurements<br/>size · orientation · severity]
    F --> G{Context-aware<br/>rule engine}
    G --> I[PASS / REWORK / REJECT<br/>+ reason]
    I --> J[Human validation<br/>or correction]
    J --> L[Retraining]
    L --> B
```

## Features

### Inspection
*Demo: Part 1*

- **Quality context**: the operator chooses the context before the analysis.
- **Image input**: upload a photo or capture the piece with a camera.
- **Step-by-step AI pipeline**: localization, classification, detection and segmentation, each with its confidence. Uncertain cases are flagged for review.
- **Measurements for each wrinkle**: length, width, orientation, area and an **explainable severity score**.
- **Contextual decision**: PASS / REWORK / REJECT with its reason, plus **correction tools** and a **PDF report**.

| | |
|---|---|
| ![Quality context](screenshots/04-inspection-context.png) **Quality context** | ![3D inspection result](screenshots/05-inspection-result-3d.png) **Result in 3D** |
| ![AI pipeline stages](screenshots/06-ai-pipeline-stages.png) **AI pipeline stages** | ![Contextual decision](screenshots/08-contextual-decision.png) **Decision with its reason** |

### 3D Wrinkle Studio
*Demo: Part 2*

Each inspected piece becomes a **3D surface built from its photo**. The studio offers:

- texture, relief, mesh and severity views;
- a before / after slider;
- a confidence threshold;
- a wrinkle list kept in sync with the image and the 3D view.

| | |
|---|---|
| ![3D Wrinkle Studio](screenshots/10-wrinkle-studio-3d.png) **Studio** | ![Wrinkle Studio relief](screenshots/15-wrinkle-studio-relief.png) **Estimated relief** |

### Batch inspection and traceability
*Demo: Parts 3 and 4*

- **Batch inspection**: one context, many pieces, processed one by one, with export to Excel.
- **Traceability**:
  - segmentation view;
  - searchable history;
  - PDF reports and Excel export;
  - side-by-side **3D comparison** of two pieces.

| | |
|---|---|
| ![Batch inspection](screenshots/11-batch-inspection.png) **Batch inspection** | ![Compare two pieces](screenshots/14-compare-inspections.png) **Compare two pieces** |

### Quality intelligence and continuous improvement
*Demo: Part 5*

- **Dashboards**: KPIs, configurable **alerts**, defect analysis, and a **3D cabin twin** coloured by reject rate.
- **Human-in-the-loop**: corrections are reviewed and exported for retraining.
- **Model monitoring**: confidence and latency.
- **Audit trail** and **role-based access**.

| | |
|---|---|
| ![Dashboard](screenshots/02-dashboard.png) **Dashboard** | ![3D cabin twin](screenshots/16-cabin-digital-twin.png) **3D cabin twin** |
| ![Feedback & active learning](screenshots/18-feedback-active-learning.png) **Corrections & active learning** | ![Audit trail](screenshots/19-audit-trail.png) **Audit trail** |

## Architecture and tech stack

```mermaid
flowchart LR
    U[React + WebGL2 frontend] -- REST / JWT --> B[Node.js / Express backend]
    B --> AI[Python / FastAPI AI service]
    B <--> DB[(PostgreSQL)]
```

| Layer | Technologies |
|---|---|
| Computer vision | PyTorch, YOLO11-OBB, EfficientNetV2-S, U-Net++, OpenCV |
| AI service | Python, FastAPI |
| Backend | Node.js, Express, JWT, role-based access |
| Database | PostgreSQL |
| Frontend | React, Vite, custom WebGL2 3D engine |
| MLOps | Kaggle notebooks, model monitoring |
| DevOps | Docker, Nginx, GitHub Actions CI/CD |

## Scope

This is a **proof of concept** focused on **wrinkle** defects. The rule engine's thresholds are illustrative examples, and the screenshots and video show test data.

## About

Final-year engineering project (PFA) by **Khadija Bensaad**, engineering student at the Institut Supérieur d'Informatique (ISI), Université de Tunis El Manar, carried out during an internship at **DRÄXLMAIER**, Zaouiet Sousse, Tunisia (2025–2026).
