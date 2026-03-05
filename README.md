# 🏭 RAG Under Pressure: 25 LLMs as Industrial Decision-Makers

![Kaggle](https://img.shields.io/badge/Kaggle-Benchmark-20BEFF?style=flat&logo=kaggle)
![License](https://img.shields.io/badge/License-MIT-green)
![Models Tested](https://img.shields.io/badge/Models%20Tested-25-blue)
![Simulation](https://img.shields.io/badge/Simulation-365%20Days-orange)
![Best Score](https://img.shields.io/badge/Best%20Score-68.4%2F100-brightgreen)
![RAG Modes](https://img.shields.io/badge/RAG%20Modes-3-purple)

> **Can LLMs make better decisions when given access to documentation?**
>
> 25 large language models were tested as virtual operators of a wastewater treatment plant across a full 365-day simulation — in three RAG modes: BARE (no docs), PASSIVE RAG (auto-injected), and ACTIVE RAG (LLM-initiated search).

🔗 **[View on Kaggle Benchmarks](https://www.kaggle.com/benchmarks/PLACEHOLDER)**

---

## 📋 Overview

This benchmark evaluates LLM decision quality in a real industrial setting using a physics-based simulation engine calibrated with actual laboratory data from **Ceyhan Wastewater Treatment Plant (WWTP), Adana, Turkey**.

Every day of the simulation, each LLM must decide:

| Parameter | Description |
|-----------|-------------|
| **DO** — Dissolved Oxygen | Setpoint for aeration control (mg/L) |
| **WAS** — Waste Activated Sludge | Daily excess biomass removal (m³/day) |
| **RAS** — Return Activated Sludge | Recirculation ratio from settler (%) |

Decisions are scored across 5 categories: Effluent Compliance, Process Stability, Crisis Response, Energy Efficiency, and Documentation Usage.

---

## 🏆 Results Summary

| Rank | Model | Score | vs Baseline | Status |
|------|-------|-------|-------------|--------|
| 1 | DeepSeek R1 | **68.4** | +17.5 | ✅ Complete |
| 2 | Claude Haiku 4.5 | **67.1** | +16.2 | ✅ Complete |
| 3 | Claude Opus 4.6 | **65.4** | +14.5 | ✅ Complete |
| 4 | Gemini 3 Flash Preview | **64.8** | +13.9 | ✅ Complete |
| 5 | Qwen 3 Coder 480B | **64.6** | +13.7 | ✅ Complete |
| … | … | … | … | … |
| 20 | Gemma 3 12B | **24.1** | −26.8 | ✅ Complete |
| — | Gemini 3 Pro Preview | DNF | — | ❌ ERROR |
| — | Claude Opus 4.1 | DNF | — | ❌ ERROR |
| — | Qwen 80B Thinking | DNF | — | ❌ ERROR |
| — | Qwen 235B A22B | DNF | — | ❌ ERROR |
| — | Gemini 3.1 Pro Preview | DNF | — | ❌ ERROR |

**Rule-Based Baseline: 50.9** — a seasonal WAS strategy (200 m³/day winter, 450 m³/day summer) with no crisis logic.

---

## 🔬 Three RAG Modes

| Mode | Document Access | Mechanism |
|------|----------------|-----------|
| **BARE** | None | LLM internal knowledge only |
| **PASSIVE RAG** | Automatic (3 chunks/prompt) | Engine injects documents via TF-IDF |
| **ACTIVE RAG** | On-demand (`SEARCH:` command) | LLM writes its own query and retrieves results |

---

## 🔑 Key Findings

- **RAG helps during crises, not routine operations.** BARE ≈ PASSIVE ≈ ACTIVE in compliance and stability categories. Documentation only made a real difference in crisis response and the Documentation Usage scoring category.

- **Larger models ≠ better decisions.** Gemma 3 1B (56.2 pts) outperformed Gemma 3 12B (24.1 pts) by 32 points. Inverse size-performance correlation observed across the Gemma family.

- **85% of DeepSeek R1's output tokens are invisible.** Chain-of-Thought (CoT) thinking blocks are billed but hidden from the user. Despite this cost, R1's score advantage over Haiku 4.5 is only 1.3 points.

- **"You can search" → 0 searches. "You SHOULD search" → searches began.** A 2-word prompt change triggered a complete behavioral shift in ACTIVE RAG. LLMs skip permitted tasks; they complete mandated tasks.

- **5 models crashed — 5 different failure mechanisms:** API timeout, context limit (200K), thinking token overflow, parse error accumulation, and ERRORED state.

- **97% token reduction via context pruning:** Total cost dropped from $46 (v2.1, 180 days) to $6.14 (v2.8, 365 days).

---

## 📁 Repository Structure

```
wwtp-rag-benchmark/
├── RAG_Under_Pressure_EN.docx    # Full benchmark report (English)
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

---

## ⚙️ Simulation Engine

The physics-based engine models:

- **Biological activity:** Monod kinetics (substrate-limited growth)
- **Temperature effects:** Arrhenius correction
- **Oxygen transfer:** KLa-based DO model
- **Sludge management:** SRT/WAS/RAS coupled dynamics
- **Crisis events:** 18 scenarios including blower failure, toxic discharge, biogas leak

---

## 📊 Scoring System

| Category | Max Points | Description |
|----------|-----------|-------------|
| Effluent Compliance | 35 | Days meeting discharge limits + violation severity |
| Process Stability | 25 | MLSS, SRT, SVI within target ranges |
| Crisis Response | 20 | Correct decisions during 18 crisis events |
| Energy Efficiency | 10 | kWh/m³ optimization |
| Documentation Usage | 10 | Effective RAG utilization |

Discharge limits: TN ≤ 10, NH₄-N ≤ 5, BOD₅ ≤ 25, COD ≤ 125, TSS ≤ 35 mg/L

---

## 💡 The Permission-Mandate Paradox

One of the most significant behavioral findings of this benchmark:

```
Prompt v1: "You can search the documentation if needed."
Result:     0 searches over 7 days — despite 18 violations

Prompt v2: "When limits are exceeded, you MUST search."
Result:     18 searches in the same 7-day window
```

Same model. Same data. Same violations. Only the prompt language changed.

**Implication:** In any RAG or agent system, "you can use the tool" and "you must use the tool" produce fundamentally different behaviors.

---

## 🔗 Links

- 🌐 [Personal Website](https://mehmetisik.dev/)
- 💼 [LinkedIn](https://www.linkedin.com/in/mehmetisik4601/)
- 📊 [Kaggle](https://www.kaggle.com/mehmetisik)
- ✍️ [Medium](https://medium.com/@mmehmetisik)

---

## 👤 Author

**Mehmet Isik**  
Data Scientist & Kaggle Grandmaster  
Plant Manager — Ceyhan WWTP, Adana, Turkey  

---

## 📄 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
