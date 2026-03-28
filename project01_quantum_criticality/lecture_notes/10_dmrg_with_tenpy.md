# Lecture Notes — Step 10: DMRG with TeNPy

---

## Overview

Exact diagonalisation is limited to roughly 40 sites. Matrix product states can represent low-entanglement states efficiently, but we have not yet said how to *find* the ground state as an MPS without first computing it by ED. This notebook introduces the **Density Matrix Renormalisation Group (DMRG)** — the algorithm that directly optimises an MPS ansatz to find the ground state, without ever constructing the full wavefunction.

DMRG was invented by Steven White in 1992 and is the most powerful method available for ground states of 1D quantum systems. We use TeNPy, a well-maintained Python library for tensor network algorithms, which handles the low-level implementation so we can focus on the physics.

---

## 1. The Variational Principle for MPS

The foundation of DMRG is the variational principle: the ground state energy $E_0$ is the minimum of $\langle \psi | \hat{H} | \psi \rangle$ over all normalised states $|\psi\rangle$.

We restrict this minimisation to the set of MPS with fixed bond dimension $\chi$. For a given $\chi$, the MPS with the lowest energy is the best possible approximation to the true ground state within that class. This is **variational MPS optimisation**.

The challenge: an MPS has $O(N \chi^2)$ free parameters. Direct gradient descent is possible but slow. DMRG exploits the MPS structure to make each optimisation step a small eigenvalue problem.

---

## 2. The DMRG Algorithm

The key insight: if you fix all tensors in the MPS except one, the variational problem reduces to an eigenvalue problem for that single tensor.

**Local update:** Fix all tensors $\{A_i\}$ except $A_k$. The energy as a function of $A_k$ alone is a quadratic form — equivalent to finding the ground state of an effective Hamiltonian $H_{\text{eff}}$ acting only on the degrees of freedom associated with site $k$ (and its bonds). This is a small eigenvalue problem of dimension $d\chi^2 \times d\chi^2$.

Solving this eigenvalue problem with Lanczos gives the optimal $A_k$ for the current environment.

**The sweep:** DMRG optimises the MPS by sweeping left to right, then right to left, updating one (or two) tensors at each step. Each sweep reduces the energy. The algorithm terminates when the energy converges between sweeps.

Each local update is guaranteed not to increase the energy. Sweeping ensures every tensor is updated, and the iterative process converges to a local minimum.

---

## 3. The Bond Dimension $\chi$

The bond dimension $\chi$ is the central parameter of DMRG:

- **Accuracy:** Larger $\chi$ means the MPS can represent more entangled states. For a gapped system, $\chi = O(1)$ is often sufficient. At a critical point you need $\chi \to \infty$ for exact results.
- **Computational cost:** Each DMRG sweep costs $O(N \chi^3 d^2)$. Doubling $\chi$ increases the cost by a factor of 8.

The standard workflow: start with a small $\chi$ to get a rough ground state, then increase $\chi$ until observables converge. Convergence is exponential for gapped systems and polynomial for critical ones.

---

## 4. Truncation and the Discarded Weight

At each DMRG update step, after solving for the optimal tensor, we perform an SVD and truncate to bond dimension $\chi$. The **discarded weight**:

$$\epsilon = \sum_{\alpha > \chi} \lambda_\alpha^2$$

is monitored as a convergence diagnostic. If the discarded weight is small (say, $< 10^{-8}$), the truncation is not introducing significant error. If it is large, $\chi$ must be increased.

You can also set a target discarded weight (rather than a fixed $\chi$) and let DMRG adaptively choose the bond dimension at each site — useful for systems where entanglement varies significantly along the chain.

---

## 5. What DMRG Can and Cannot Do

**Where DMRG excels:**
- Ground states of 1D gapped systems: essentially exact for modest $\chi$.
- Quasi-1D systems (ladders, cylinders): tractable if the circumference is not too large.
- Systems with open boundary conditions: DMRG converges faster with OBC than PBC.
- Long chains: chains of $10^3$ or $10^4$ sites are routine for gapped systems.

**Where DMRG struggles:**
- 2D systems: the entanglement grows with the width $W$ as $S \sim W$, requiring $\chi \sim e^W$ — exponential in system width. In practice DMRG is limited to widths of $\sim 10$ for moderate accuracy.
- Critical systems: requires large $\chi$ that grows with system size.
- Excited states and dynamics: more challenging than ground states, though time-evolution algorithms (TEBD, TDVP) extend the framework.

---

## 6. TeNPy

TeNPy (Tensor Network Python) is a Python library developed primarily by Johannes Hauschild and Frank Pollmann for tensor network algorithms in condensed matter physics. It provides:

- Implementations of DMRG (finite and infinite), TEBD, VUMPS, and other algorithms.
- A model library with many standard Hamiltonians (Heisenberg, TFIM, Hubbard, etc.) and the ability to define custom models.
- Automatic handling of Abelian symmetries (e.g., conserved total $S^z$), which dramatically reduces the effective bond dimension and computational cost.
- Tools for computing observables, correlation functions, and entanglement spectra.

In the notebook we use TeNPy to define the TFIM model, run DMRG, extract observables, scan over $\Gamma/J$ to map out the phase diagram, and compare with ED results from notebooks 06 and 07.

---

## 7. Understanding TeNPy Output

A TeNPy DMRG run produces at each sweep:

- **Energy per site:** Converges from above to the true ground state energy.
- **Maximum discarded weight:** Should decrease as the run proceeds.
- **Entropy at each bond:** The entanglement entropy profile across the chain.

One thing to watch: DMRG can get stuck in local minima in the ordered phase where the two ground states can mix. Using a small symmetry-breaking field during the first few sweeps, then removing it, helps steer DMRG into the correct sector.

---

## 8. DMRG vs ED: A Comparison

| Property | Exact Diagonalisation | DMRG |
|---|---|---|
| System sizes | Up to ~40 sites | Up to thousands of sites |
| Cost | $O(4^N)$ memory | $O(N \chi^3)$ time |
| Accuracy | Exact | Controlled by $\chi$ |
| Phase diagram | All parameters at once | One point at a time |
| Excited states | Full spectrum | First few states |
| Dimensions | Any | 1D (or narrow 2D) |

The key message: ED and DMRG are complementary. Use ED for small systems to benchmark. Use DMRG for large systems to access the thermodynamic limit.

---

## 9. What Comes Next

With DMRG we can now study systems of hundreds of sites — far beyond ED reach. The final notebook applies these tools to the **Kitaev chain**, a 1D model of spinless fermions with p-wave superconductivity. The Kitaev chain has a topological phase transition: its two phases are not distinguished by any local order parameter, but by a global topological invariant. At the phase boundary, Majorana zero modes appear at the chain ends — exotic quasiparticles proposed as building blocks of fault-tolerant quantum computers.
