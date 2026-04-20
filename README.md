```markdown
# Physics-Informed Neural Networks with Asymmetric Residual Scaling (R-PINN)

![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=for-the-badge&logo=PyTorch&logoColor=white)
![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> **Official PyTorch Implementation** for the paper:  
> *"Physics-Informed Neural Networks with Asymmetric Residual Scaling for State Estimation in Unobservable Power Systems"*

This repository provides the complete source code, datasets, and physical simulation environments to reproduce the highly scalable and robust state estimation framework (R-PINN) across IEEE 33-bus, 69-bus, and 118-bus power systems under a severe **15% observability constraint**.

---

## 🌟 Key Innovations

Standard PINNs suffer from Jacobian ill-conditioning, severe non-convexity, and scale-induced gradient imbalances when applied to large-scale AC power flows. **R-PINN** systematically resolves these pathologies via:

1. **Asymmetric Residual Scaling (ARS):** Embeds statutory physical priors (e.g., $1.0$ p.u. for distribution, $0.95$ p.u. for transmission) into the neural hypothesis space. It transforms global non-convex optimization into localized residual tracking, fundamentally bypassing the curse of dimensionality.
2. **Normalized Mean-Loss Aggregation:** Decouples the observational gradient magnitude from the sensor density, preventing local sensor overfitting and ensuring topology-agnostic generalization.
3. **Zero-Shot N-1 Generalization (No TTA):** Successfully reconstructs blind-zone voltages during unseen topological shifts (branch outages) relying strictly on sparse PMU measurement fluctuations, without any Test-Time Adaptation or topology leakage.
4. **Statistical Physics Immunity:** Acts as a global statistical low-pass filter, maintaining an invariant MAE precision even under extreme $3.0\%$ Gaussian measurement noise.

---

## 📁 Repository Structure

The repository is structured into three main directories representing different topological scales and complexities:

```text
├── IEEE 33/               # Foundational Validation (Radial Distribution)
│   ├── IEEE33.ipynb       # Base case training & ARS ablation studies
│   ├── IEEE33鲁棒测试.ipynb # Measurement noise injection evaluations
│   └── data/              # 50k base datasets & N-1 contingency sets
├── IEEE 69/               # Deep Radial Extrapolation (Deep Feeders)
│   ├── IEEE69.ipynb       # Mean-Loss vs Sum-Loss scale imbalance tests
│   ├── IEEE69拓扑鲁棒.ipynb # Extreme Few-Shot (5k samples) zero-shot inference
│   └── data/              # High-dimensional non-convex datasets
├── IEEE 118/              # Ultimate Stress Test (Meshed Transmission Grid)
│   ├── IEEE118 1.ipynb    # Resolving 431.47 p.u. Vanilla PINN explosion via ARS
│   ├── 118拓扑鲁棒.ipynb    # Redundancy-induced correction & meshed topology N-1
│   ├── pandapower.ipynb   # Raw physical underlying data generation via pandapower
│   └── data/              # Transmission-level steady-state datasets
└── README.md              # Project documentation
```
*(Note: Jupyter notebooks contain comprehensive Chinese/English comments detailing the mathematical mappings and experimental protocols.)*

---

## ⚙️ Environment Setup

We recommend utilizing a CUDA-enabled GPU for deep learning acceleration.

```bash
# Clone the repository
git clone [https://github.com/lemonsheep1123/IEEE33.git](https://github.com/lemonsheep1123/IEEE33.git)  # Update with your actual repo URL
cd IEEE33

# Install dependencies
pip install torch torchvision torchaudio
pip install pandapower numpy pandas scikit-learn matplotlib tqdm
```

---

## 🚀 How to Run

All experiments are encapsulated within standalone Jupyter Notebooks (`.ipynb`) for maximum reproducibility and interactive visualization. 

1. **Data Generation (Optional):** Navigate to the respective folder and execute the `pandapower` generator scripts to dynamically solve the non-linear AC power flows and generate the `.csv` datasets.
2. **Train and Evaluate:** Open core scripts (e.g., `IEEE118 1.ipynb`). All random seeds (`set_seed(42)`) and 15% sensor spatial layouts are strictly locked to guarantee **100% bit-level reproducibility**.
3. **Zero-Shot Inference:** Run the `*拓扑鲁棒.ipynb` (Topology Robustness) notebooks to witness the model reconstruct unobservable dead zones dynamically under N-1 contingencies with frozen weights.

---

## 📈 Highlighted Results

| Grid Topology | Baseline MAE | Vanilla PINN (No ARS) | Proposed R-PINN (15% Obs) |
| :--- | :--- | :--- | :--- |
| **IEEE 33-Bus** | Intact / N-1 | Diverged (0.2700 p.u.) | **0.0030** p.u. |
| **IEEE 69-Bus** | Intact / N-1 | Diverged (0.3647 p.u.) | **0.0005** p.u. |
| **IEEE 118-Bus**| Intact / N-1 | Exploded (431.47 p.u.) | **0.0044** p.u. |

*Even with the training dataset slashed to just **10% (5,000 samples)**, the R-PINN strictly adheres to KCL laws and successfully extrapolates global states.*

---

## 📝 Citation

If you find our framework, datasets, or the ARS concept helpful in your research, please consider citing our paper:

```bibtex
@article{lemonsheep2026rpinn,
  title={Physics-Informed Neural Networks with Asymmetric Residual Scaling for State Estimation in Unobservable Power Systems},
  author={lemon_sheep},
  journal={IEEE Transactions on Power Systems},
  year={2026},
  publisher={IEEE}
}
```
*(BibTeX will be updated upon final publication).*

---
**Contact:** For any questions or discussions regarding the physical formulations or deep learning architectures, please feel free to open an issue or reach out via email.
```