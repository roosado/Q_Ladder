# 12 Steps to Density Functional Theory

*From the variational principle to a working self-consistent Kohn-Sham solver*

**Starting point:** Variational principle and Hartree equations for helium
**Endpoint:** A working self-consistent Kohn-Sham DFT solver for a 1D model system

## Notebooks

| # | Title |
|---|-------|
| 00 | Introduction and Prerequisites |
| 01 | The variational principle — bounding the ground state energy numerically |
| 02 | Hartree mean-field theory — why electron-electron interaction is hard |
| 03 | The Hohenberg-Kohn theorems — energy as a functional of density |
| 04 | The Kohn-Sham mapping — interacting to non-interacting electrons |
| 05 | Discretising the Kohn-Sham equations on a 1D grid |
| 06 | The self-consistent field loop — convergence and mixing schemes |
| 07 | Exchange-correlation functionals — LDA and its physical justification |
| 08 | A working 1D DFT solver for a model atom |
| 09 | Convergence tests, basis set effects, and numerical precision |
| 10 | Connection to production codes — what GPAW and Quantum ESPRESSO are actually doing |

## Setup

```bash
pip install -r requirements.txt
jupyter notebook notebooks/
```
