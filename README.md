# Neural-ODE Surrogate for Langmuir Adsorption (Julia 1.10+)

> **Paper counterpart:**  
> **“A 49-Parameter Neural-ODE Learns and Stays Robust to Noise in Langmuir Adsorption Kinetics”**  
> (pre-print in preparation)

The project is a **fully reproducible Scientific-ML case study** that shows how a *tiny*
Neural Ordinary Differential Equation with just **49 trainable parameters** can

1. **Learn the exact Langmuir adsorption curve** almost perfectly from synthetic data,  
2. **Remain accurate when measurements are noisy** (1 %–10 % Gaussian noise), and  
3. **Give consistent results across ten random seeds**, demonstrating optimiser stability.

<p align="center">
  <img src="assets/trajectory_comparison.png"  alt="Trajectory before training" width="40%">
  <img src="assets/trajectory_comparison_2.png" alt="Trajectory after training"  width="40%">
</p>

---

## 🔑 What the paper (and this repo) actually do

- **Generate data** – 30 points from the analytic Langmuir ODE, plus synthetic noise levels  
  of **1 %**, **5 %** (10 replicates) and **10 %**.
- **Define the surrogate** – a one-hidden-layer Lux network (16 tanh neurons → 49 params)  
  wrapped inside `DiffEqFlux.NeuralODE`.
- **Two-stage optimisation** – coarse **ADAM** followed by precision **BFGS** (via
  `Optimization.jl`).
- **Benchmark results**

  | Scenario                         | Mean RMSE | Notes                                  |
  |----------------------------------|----------:|----------------------------------------|
  | Exact (0 % noise)                | 0.17 %    | After ADAM → BFGS                      |
  | 5 % noise, 10 seeds              | 0.375 ± 0.017 | Tight spread → stable convergence |
  | Noise sweep 1 %, 5 %, 10 %       | 0.363 → 0.405 | Error rises only ~12 %               |

- **Outputs reproduced** – all figures (loss curve, seed histogram, noise-sensitivity bar
  chart), tables, and the LaTeX manuscript itself.

> **Repo URL:** <https://github.com/SAGAR-TAMANG/langmuir-antibiotic-neural-ode-julia>

---

## 📂 Repository layout

```text
.
├─ src/
│  ├─ julia.ipynb          # main notebook: data, training, plots
│  └─ noise_sweep.jl       # CLI script used for 1–10 % noise study
├─ assets/                 # generated PNGs & CSV metrics
├─ paper/
│  └─ manuscript/          # arXiv-ready LaTeX source
├─ Project.toml / Manifest.toml
└─ README.md               # ← you are here
