# Quantum Simulation Series

A progressive, open-source Jupyter notebook curriculum for quantum technology students,
modelled on Lorena Barba's *12 Steps to Navier-Stokes*.

Developed by students of the Quantum Technology Masters programme at Uppsala University.

## Phase 1 Projects

| # | Series | Domain | Tools |
|---|--------|--------|-------|
| 01 | [12 Steps to Quantum Criticality](project01_quantum_criticality/) | Statistical mechanics, many-body, QI | NumPy, SciPy, TeNPy |
| 02 | [12 Steps to Superconducting Qubits](project02_superconducting_qubits/) | Circuit QED, open systems | NumPy, QuTiP |
| 03 | [12 Steps to Density Functional Theory](project03_dft/) | Electronic structure | NumPy, SciPy, GPAW |

## Design Philosophy

- Each notebook is self-contained and fully executable top to bottom.
- Physics and numerics develop in parallel.
- Every step begins from something the student already understands.
- The endpoint of each series is connected to real physics and active research.

## Getting Started

Each project has its own `requirements.txt`. Install dependencies for the project you want
to run:

```bash
pip install -r project01_quantum_criticality/requirements.txt
```

Then open the notebooks in order, starting from `00_introduction.ipynb`.

## Contributing

All materials are developed in the open. Contributions from students at other institutions
are welcome. See each project's README for local setup instructions.
