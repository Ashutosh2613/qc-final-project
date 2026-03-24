# Grover's Search Algorithm — Qiskit Implementation

> **Quantum Computing Project**  
> 10-qubit search space (2¹⁰ = 1024 states)  
> Marked states: `0110011010` and `1101010001`

---

## Overview

This project implements **Grover's quantum search algorithm** from scratch using Qiskit.  
No built-in `qiskit.algorithms.Grover` is used — all components (oracle, diffuser, iteration loop) are constructed manually with quantum gates.

### Algorithm Summary

Grover's algorithm searches an unstructured database of N items in **O(√N)** steps, compared to the classical O(N).

For N = 1024 states and M = 2 marked elements, the optimal iteration count is:

```
k ≈ (π/4) × √(N/M) ≈ (π/4) × √512 ≈ 17.7 → 18 iterations
```

---

## Project Structure

```
grover_project/
├── grover_algorithm.py      # Standalone Python script
├── grover_algorithm.ipynb   # Jupyter Notebook (recommended)
├── requirements.txt         # Python dependencies
└── README.md
```

---

## Requirements

- Python 3.9 or higher
- Qiskit (open-source)
- Qiskit-Aer (simulator)

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/grover-qiskit.git
cd grover-qiskit
```

### 2. (Recommended) Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

---

## How to Run

### Option A — Python script

```bash
python grover_algorithm.py
```

The script will:
- Run the Grover circuit for **1, 3, 5, 10, and 18 iterations**
- Print the probability of each marked state to the console
- Save a histogram image for each run (`grover_<n>_iterations.png`)

### Option B — Jupyter Notebook (recommended for step-by-step view)

```bash
jupyter notebook grover_algorithm.ipynb
```

Run all cells sequentially. Each section is self-contained and labelled.

---

## Expected Output

When the program runs, a histogram like the one below will appear for each iteration count.  
The two marked states (`0110011010` and `1101010001`) appear in **red** with the highest probability.

At the **optimal 18 iterations**, each marked state should appear with roughly **40–50% probability** each (summing to ~90%+ combined).

### Console output example (18 iterations)

```
============================================================
  Grover's Algorithm  |  10 qubits  |  18 iteration(s)
  Search space size   : 1024
  Marked states       : ['0110011010', '1101010001']
  Shots               : 2048
============================================================

  Marked state probabilities:
    0110011010  →  941/2048  (45.95%)
    1101010001  →  923/2048  (45.07%)
```

---

## Code Architecture

| Component | Function | File location |
|-----------|----------|---------------|
| **Oracle** | Phase-flips marked states using X gates + MCZ | `build_oracle()` |
| **Diffuser** | Applies 2\|s⟩⟨s\| − I via H → U₀ → H | `build_diffuser()` |
| **Grover Iteration** | Composes Oracle + Diffuser | `build_grover_iteration()` |
| **Main runner** | Builds full circuit, runs on Aer, plots results | `run_grover()` + `__main__` |

---

## Oracle Design

For each marked state the oracle:
1. Applies `X` gates to qubits where the target bit is `0`
2. Applies a multi-controlled-Z (MCZ) gate → phase-flips `|11...1⟩`
3. Undoes the `X` gates

This is **fully parameterised** — changing `TARGETS` in the code automatically updates the oracle:

```python
TARGETS = ["0110011010", "1101010001"]   # ← change these freely
```

---

## Dependencies (requirements.txt)

```
qiskit>=1.0.0
qiskit-aer>=0.14.0
matplotlib>=3.7.0
numpy>=1.24.0
jupyter>=1.0.0        # only needed for .ipynb
```

---

## References

- IBM Quantum Learning: https://quantum.cloud.ibm.com/learning/en/modules/computer-science/grovers
- Qiskit Documentation: https://docs.quantum.ibm.com/
- Nielsen & Chuang, *Quantum Computation and Quantum Information*, Ch. 6
