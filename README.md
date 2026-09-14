# Atheer System: Simulation Evaluation Artifact for Offline-First Mobile Payments

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19383900.svg)](https://doi.org/10.5281/zenodo.19383900)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository (artifact **v3.0.0**, camera-ready release) serves as the **reproducibility artifact** for the simulation-based evaluation of the "Atheer" system, as detailed in **Section VII** of the research paper:

> N. Al-Mekhlafi and A. Al-Mutawakel, "A Private APN and NFC HCE Offline Payment Framework for Critical Infrastructure Protection in Emerging Markets," IDEASET-2026, Submission 43.

## 📝 System Overview
This project enables researchers and developers to reproduce and validate published results, ensuring scientific transparency and reliability. The system performs a comprehensive simulation of a **4-layer End-to-End (E2E) model**:

| Layer | Functional Description |
| :--- | :--- |
| **Edge Layer (SDK)** | Simulates NFC interaction and Host Card Emulation (HCE) for local cryptographic token generation. |
| **Network Layer (Transport)** | Models data transmission via Public Internet (S1) or Private APN (S2), accounting for packet loss and latency. |
| **Processing Layer (Atheer Switch)** | Simulates the gateway switch using Redis (idempotency) and PostgreSQL (storage) micro-latencies. |
| **Integration Layer (Core Bank)** | Models the connection to core banking ledgers through secure API adapters. |

## 📊 Reproducible Results
This codebase allows for the regeneration of the following figures and tables presented in the research paper:

*   **Figure 6 (Fig. 6):** Transaction Success Rate vs. Network Load (Mean ± 95% CI).
*   **Figure 7 (Fig. 7):** P95 End-to-End (E2E) Latency under varying traffic loads.
*   **Table V:** Aggregated Performance Summary (Mean ± 95% CI).
*   **Table VI:** Failure Breakdown Analysis at peak load (500 TPS).
*   **Table VII:** Sensitivity Analysis at 500 TPS (armed-session duration and timeout sweep).
*   **Scenario S3:** Private APN with load-dependent degradation — the design-boundary scenario newly documented in v3.0.0.

> **Technical Note:**
> Simulation parameters are defined in `configs/paper.yml` (the source of truth) and loaded dynamically by `atheer_sim.py` at runtime. Any parameter changes should be made in `configs/paper.yml`.

## 📂 Repository Layout
*   `atheer_sim.py`: Main Discrete-Event Simulation (DES) engine built with SimPy.
*   `requirements.txt`: List of required Python dependencies.
*   `tools/build_paper_tables.py`: Helper tool to generate formatted tables from raw simulation data.
*   `configs/paper.yml`: Configuration file containing the specific parameters used in the paper's scenarios.
*   `docs/`: Additional documentation including model assumptions and parameter definitions.

## ⚙️ Requirements
*   **Language:** Python 3.10 or higher.
*   **Core Dependencies:**
    *   `simpy >= 4.1` (Event simulation management)
    *   `numpy`, `pandas` (Data processing and analysis)
    *   `matplotlib` (Scientific data visualization)

## 🚀 Quick Start
To reproduce the research results, follow these steps:

1. **Environment Setup:**
   ```shell
   python -m venv .venv
   source .venv/bin/activate  # For Linux/macOS
   # .venv\Scripts\activate  # For Windows
   pip install -r requirements.txt
   ```

2. **Execute Simulation:**
   ```shell
   python atheer_sim.py
   ```

## 📈 Key Performance Indicators (KPIs)
*   **Switch Overhead:** Validates that the internal processing time within the Atheer Switch remains below **20ms**.
*   **End-to-End (E2E) Latency:** Measures the total time from NFC tap to merchant confirmation.
*   **Success Rate:** The percentage of transactions successfully completed within the defined timeout period.

## 📚 Citation
If you utilize this work in your research, please use the following citation:

```bibtex
@software{al_mutawakel_2026_atheer_sim,
  author       = {Al-Mutawakel, Ahmed Ali Mohammed Hasan},
  title        = {Atheer Simulation Evaluation Artifact},
  year         = 2026,
  version      = {v3.0.0},
  doi          = {10.5281/zenodo.19383900},
  url          = {https://doi.org/10.5281/zenodo.19383900},
  note         = {Concept DOI - always resolves to the latest version}
}
```

## ⚖️ License & Contact
*   **License:** MIT License.
*   **Contact:** Please refer to the author's email provided in the research paper or open an Issue in this repository.
