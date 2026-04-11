# Lecture Notes — Lecture 10: DMRG II: TeNPy and Applications

---

## Overview

Lecture 09 described the DMRG algorithm. This lecture puts it into practice using **TeNPy** (Tensor Network Python), a well-maintained library that handles the low-level implementation. The focus here is on the workflow: how to define a model, run DMRG, interpret the output, and use the results to explore the TFIM phase diagram at system sizes far beyond ED reach.

---

## 1. TeNPy: Setup and Interface

TeNPy (Tensor Network Python) is a Python library developed primarily by Johannes Hauschild and Frank Pollmann for tensor network algorithms in condensed matter physics. It provides:

- Implementations of DMRG (finite and infinite), TEBD, VUMPS, and other algorithms.
- A model library with many standard Hamiltonians (Heisenberg, TFIM, Hubbard, etc.) and the ability to define custom models.
- Automatic handling of Abelian symmetries (e.g., conserved total $S^z$), which dramatically reduces the effective bond dimension and computational cost.
- Tools for computing observables, correlation functions, and entanglement spectra.

Installation:

```bash
pip install physics-tenpy
```

A minimal DMRG run has four steps: (1) define the model, (2) define the MPS, (3) configure the DMRG engine, (4) run. TeNPy's API makes each step explicit, and the output is a converged MPS object from which any observable can be computed.

---

## 2. Defining Models in TeNPy

TeNPy models are defined by specifying the Hamiltonian in terms of local operators and couplings. For the 1D TFIM:

```python
import tenpy
from tenpy.models.tf_ising import TFIChain

model_params = {
    'J': 1.0,          # Ising coupling
    'g': 1.0,          # transverse field (Gamma/J = g)
    'L': 100,          # chain length
    'bc_MPS': 'finite' # open boundary conditions
}
model = TFIChain(model_params)
```

TeNPy's built-in `TFIChain` model implements $\hat{H} = -J \sum \hat{\sigma}^z_i \hat{\sigma}^z_{i+1} - g \sum \hat{\sigma}^x_i$. Custom models are defined by subclassing `CouplingMPOModel` and specifying the bond terms via `add_coupling`.

A key feature: TeNPy automatically constructs the Hamiltonian as a **Matrix Product Operator (MPO)** — the operator analogue of an MPS. This MPO representation is what allows the effective Hamiltonian $H_{\text{eff}}$ to be computed efficiently during DMRG.

---

## 3. Running DMRG and Reading the Output

```python
from tenpy.algorithms import dmrg
from tenpy.networks.mps import MPS

# Initialise a random MPS
psi = MPS.from_lat_product_state(model.lat, p_state=['up', 'down'])

# DMRG parameters
dmrg_params = {
    'trunc_params': {'chi_max': 100, 'svd_min': 1e-10},
    'mixer': True,         # use mixer for better convergence
    'N_sweeps_check': 10,
    'mixer_params': {'amplitude': 1e-5, 'decay': 1.5, 'disable_after': 50},
}

# Run
engine = dmrg.TwoSiteDMRGEngine(psi, model, dmrg_params)
E0, psi = engine.run()
```

A TeNPy DMRG run produces at each sweep:

- **Energy per site:** Converges from above to the true ground state energy.
- **Maximum discarded weight:** Should decrease as the run proceeds.
- **Entropy at each bond:** The entanglement entropy profile across the chain.

One thing to watch: DMRG can get stuck in local minima in the ordered phase where the two ground states can mix. The `mixer` option adds small noise to the state during early sweeps, which helps escape local minima. Using a small symmetry-breaking field during the first few sweeps — then removing it — also helps steer DMRG into the correct sector.

---

## 4. DMRG vs ED: A Comparison

| Property | Exact Diagonalisation | DMRG |
|---|---|---|
| System sizes | Up to ~40 sites | Up to thousands of sites |
| Cost | $O(4^N)$ memory | $O(N \chi^3)$ time |
| Accuracy | Exact | Controlled by $\chi$ |
| Phase diagram | All parameters at once | One point at a time |
| Excited states | Full spectrum | First few states |
| Dimensions | Any | 1D (or narrow 2D) |

The key message: ED and DMRG are complementary. Use ED for small systems to benchmark. Use DMRG for large systems to access the thermodynamic limit. For the TFIM at $N = 16$, DMRG with $\chi = 32$ should reproduce the ED ground state energy to at least 8 significant figures — this is your first validation step.

---

## 5. Exploring the TFIM Phase Diagram with DMRG

Once DMRG is working and validated against ED, the power comes from scanning over $\Gamma/J$.

**Phase boundary:** Run DMRG at a range of $\Gamma/J$ values for several system sizes $L$. Extract the Binder cumulant $U_4(L, \Gamma)$ and identify the crossing point. For the 1D TFIM, the crossing should converge rapidly to $\Gamma_c = J$ as $L$ increases.

**Spectral gap:** Extract the gap $\Delta = E_1 - E_0$ by running DMRG targeting the first excited state (TeNPy supports this via the `orthogonal_to` parameter). Plot $L \cdot \Delta$ vs $\Gamma/J$ for several $L$ — the crossing at $\Gamma_c$ is the critical point.

**Order parameter:** Compute $\langle \hat{\sigma}^z_i \rangle$ for each site. In the ordered phase with OBC and a small symmetry-breaking field, this profiles the order parameter and reveals finite-size boundary effects. In the disordered phase, it is uniformly zero.

At $N = 200$ or $N = 500$, these quantities approach their thermodynamic-limit values far more closely than any ED calculation. The finite-size corrections are of order $(1/L)^{1/\nu} = 1/L$ for the TFIM, which is already very small at $L = 200$.

---

## 6. Entanglement Entropy from DMRG

The entanglement entropy at each bond is directly available from the MPS singular values:

```python
EE = psi.entanglement_entropy()  # array of length N-1
```

This returns $S_A(\ell)$ for all cuts $\ell = 1, \ldots, N-1$. Comparing this profile across values of $\Gamma/J$ demonstrates the three regimes established in lecture 07:

- **Gapped phases:** $S_A(\ell)$ saturates to a constant for large $\ell$.
- **Critical point:** $S_A(\ell) = \frac{1}{6} \log\left(\frac{2L}{\pi} \sin\frac{\pi \ell}{L}\right) + \text{const}$, from the OBC Calabrese-Cardy formula.

Fitting the critical entropy profile to extract the central charge $c$ is a powerful check: we expect $c = 1/2$ for the Ising CFT. DMRG at $L = 200$ gives a much cleaner fit than ED at $L = 20$.

---

## 7. Pushing to Larger Systems

The real advantage of DMRG over ED becomes clear when you push to $L \sim 200$–$1000$.

**Finite-size scaling with large systems:** With $L$ up to 500, finite-size corrections are $\lesssim 1\%$ for most observables. Critical exponents extracted from a scaling collapse at these sizes are much more reliable than those from ED at $L = 8$–20.

**Bond dimension convergence:** For the gapped TFIM away from criticality, $\chi = 32$ is sufficient. Near the critical point, $\chi = 128$ or $\chi = 256$ may be needed for quantitative accuracy. The discarded weight tells you when to stop increasing $\chi$.

**Computational time:** A DMRG run at $L = 500$, $\chi = 128$ takes seconds to minutes on a laptop. This is what makes DMRG a practical research tool — the computations that once required supercomputers can now be done interactively.

The combination of DMRG at large $L$ and ED at small $L$ gives a complete, benchmarked picture of the quantum phase transition. This workflow — ED validation followed by DMRG extrapolation — is standard in modern condensed matter research.
