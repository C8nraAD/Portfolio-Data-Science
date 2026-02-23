# 🩺 InsurTech Premium Optimizer: AI-Driven Underwriting & Simulation Engine

[Live Deployment](https://doradcaskladki.streamlit.app)

## 🎯 System Overview & Business Objective
An interactive InsurTech platform engineered for dynamic health insurance premium estimation and risk profiling. The system transcends basic inference by integrating a deterministic **Recommendation & Simulation Engine**. It executes real-time financial simulations, quantifying potential premium savings (ROI) achievable through targeted health metrics optimization (e.g., BMI reduction, smoking cessation).

## 🛠 Tech Stack & Ecosystem
| Tier | Technology |
| :--- | :--- |
| **Frontend & Visualization** | Streamlit, Plotly Express (Dynamic state-driven charting) |
| **Machine Learning** | PyCaret (AutoML pipeline & artifact serialization) |
| **Data Engineering** | Pandas, Python 3.9+ (Strict Type Hinting) |
| **Architecture Patterns** | Immutability (`@dataclass(frozen=True)`), Functional Programming, SoC |

## 🏗 Core Architectural Mechanics
The codebase intentionally breaks away from Streamlit's default flat-script paradigm, enforcing rigorous software engineering principles:

1. **Immutable State Management:** Application configuration and user telemetry are strictly governed by frozen data structures (`AppConfig`, `UserProfile`, `AppState`). This entirely eliminates unintended side-effects and guarantees data consistency across execution reruns and simulation triggers.
2. **Functional Rule Engine:** The `RecommendationEngine` evaluates user profiles via higher-order functions. Each business rule encapsulates its own activation logic (`applies_when`) and state mutation constraint (`simulate_change`), enabling $O(1)$ scalability for adding new underwriting rules without modifying the core system.
3. **Strict Separation of Concerns (SoC):** Presentation layers (`ui_sidebar`, `ui_dashboard`) are heavily decoupled from domain logic (premium computation) and session state orchestrators.
4. **Deterministic Simulation State:** Any mutation in the baseline user parameters triggers an automatic teardown and reset of active simulation states, preventing logical anomalies in financial projections.



## 📸 System Telemetry & Output
![KPI Dashboard & Risk Profile](assets/1.JPG)
![Financial Simulation & Analytics](assets/2.JPG)

## 🧠 ML Inference & Business Logic Layer
The core underwriting engine is powered by a serialized AutoML pipeline deployed via PyCaret.

* **Inference Pipeline:** Instantiated `UserProfile` objects are vectorized into Pandas DataFrames for batch-like model consumption.
* **Deterministic Post-Processing:** The raw probabilistic output (historical USD baseline) is intercepted and subjected to strict business transformations dynamically: 
$$P_{final} = (P_{ML} \times FX) \times (1 - D) \times MAF$$
*(Where: $FX$ = Currency Exchange Rate, $D$ = Discount Factors, $MAF$ = Market Adjustment Factor).*