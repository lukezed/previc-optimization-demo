# PREVIC Item Selection: An Affordable Optimization Demo

This repository documents my personal exploration into optimizing the item selection process for the PREVIC assessment. It provides a computationally affordable alternative to the methodology described in:

> **Bohn, M., Prein, J., Engicht, J., Haun, D. B. M., Gagarina, N., & Koch, T. (2025).** *PREVIC: An adaptive parent report measure of expressive vocabulary in children between 3 and 8 years of age.* Behavior Research Methods, 57, 95.

## 🎯 Motivation

The original PREVIC study establishes a robust methodology using **Simulated Annealing** and a complex cost function involving iterative BIRT fit statistics. While rigorous, this approach is computationally expensive and nearly impossible to reproduce on standard personal computers within a reasonable timeframe.

**My goal with this project is to democratize the selection process.** I implemented a lightweight pipeline that runs locally on consumer hardware while maintaining comparable performance to the original cluster-computed solution.

## ⚡ Key Optimizations

I introduced three major shifts to reduce computational costs without breaking the theoretical framework:

1.  **Fit Statistics Strategy**: I replaced the expensive BIRT fit proxies (calculated per iteration in the original paper) with native **Frequentist IRT (1PL) in/out fit parameters** using `TAM`. This allows me to bypass the need for modeling the global item pool in a Bayesian framework during the initial screening phase.
2.  **Algorithmic Approach**: I shifted from stochastic **Simulated Annealing** to a deterministic **Hungarian Algorithm** (Linear Assignment). I reframed the "spacing" objective from a random search process into a linear greedy matching task, solving for the global optimum of my defined cost matrix.
3.  **Cost Function Efficiency**: I utilized native BIRT parameters—specifically **Posterior Width**—directly as a cost penalty, avoiding redundant calculations.

## 💻 Hardware & Runtime

I executed the entire optimization pipeline on my personal laptop:
* **Device**: MacBook Pro (M2 Pro chip, 10-core CPU)
* **Memory**: 16 GB RAM

On this setup, the full workflow completes in **under 24 hours**. This demonstrates that the optimization task, which originally required significant computing resources, is now affordable on a standard high-performance laptop.

**Performance Note**: Preliminary results suggest a minor trade-off in theoretical performance (~10% regarding geometric spacing) compared to the original study. However, this gap could likely be narrowed further by manually fine-tuning the cost function parameters, though I did not pursue such exhaustive optimization in this demo.

## 📂 Project Structure

```text
.
├── data/
│   ├── final_item_list.csv      # The original author's selected items (Benchmark)
│   ├── my_final_item_list.csv   # [OUTPUT] The optimized 90 items from my pipeline
│   └── previc_data.csv          # Raw response data (Sensitive - Not tracked)
├── models/
│   ├── irt1.rds                 # Base 1PL Global Model
│   ├── irt2.rds                 # Base 2PL Global Model
│   ├── irt_ori.rds              # Model fitted on the Original author's subset
│   └── optimization/            # Iterative models for different target lengths
│       ├── irt1_70.rds ... irt1_100.rds
│       └── irt2_70.rds ... irt2_100.rds
├── optimal_demo/
│   ├── optimal_demo.Rmd         # 🟢 MAIN ANALYSIS SCRIPT
│   └── optimal_demo.pdf         # Rendered report
├── original/
│   ├── analysis.Rmd             # Code for reproducing original author's results
│   └── fit_selected_items.rds   # Reference object
└── previc-optimization-demo.Rproj
```

## Disclaimer
This is a Proof-of-Concept (Demo). It is not intended to be a rigorous academic replication or a final production-ready solution. I may revisit this project in the future to provide more specific solutions and detailed validations.
