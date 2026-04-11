# Lecture Notes — Lecture 9: DMRG I: The Algorithm

---

## Overview

Exact diagonalisation is limited to roughly 40 sites. Matrix product states can represent low-entanglement states efficiently, but lecture 08 showed how to *compress* an existing wavefunction into MPS form — not how to *find* the ground state as an MPS in the first place. This lecture introduces the **Density Matrix Renormalisation Group (DMRG)** — the algorithm that directly optimises an MPS ansatz to find the ground state, without ever constructing the full wavefunction.

DMRG was invented by Steven White in 1992 and is the most powerful method available for ground states of 1D quantum systems. This lecture covers the algorithm itself; lecture 10 covers the practical implementation using TeNPy.

---

## 1. The Variational Principle for MPS

The foundation of DMRG is the variational principle: the ground state energy $E_0$ is the minimum of $\langle \psi | \hat{H} | \psi \rangle$ over all normalised states $|\psi\rangle$.

We restrict this minimisation to the set of MPS with fixed bond dimension $\chi$. For a given $\chi$, the MPS with the lowest energy is the best possible approximation to the true ground state within that class. This is **variational MPS optimisation**.

The challenge: an MPS has $O(N \chi^2)$ free parameters. Direct gradient descent is possible but slow. DMRG exploits the MPS structure to make each optimisation step a small eigenvalue problem.

---

## 2. The DMRG Algorithm

The key insight: if you fix all tensors in the MPS except one, the variational problem reduces to an eigenvalue problem for that single tensor.

**Local update:** Fix all tensors $\{A_i\}$ except $A_k$. The energy as a function of $A_k$ alone is a quadratic form — equivalent to finding the ground state of an **effective Hamiltonian** $H_{\text{eff}}$ acting only on the degrees of freedom associated with site $k$ (and its bonds). $H_{\text{eff}}$ is constructed by contracting all fixed MPS tensors $\{A_i\}_{i \neq k}$ with the full Hamiltonian from the left and right, producing a small matrix of dimension $d\chi^2 \times d\chi^2$ that encodes the influence of the rest of the chain on site $k$. Solving this eigenvalue problem with Lanczos costs $O(\chi^3)$ rather than $O(2^{2N})$.

**The sweep:** DMRG optimises the MPS by sweeping left to right, then right to left, updating one (or two) tensors at each step. Each sweep reduces the energy. The algorithm terminates when the energy converges between sweeps.

Each local update is guaranteed not to increase the energy. Sweeping ensures every tensor is updated, and the iterative process converges to a local minimum.

---

## 3. The Bond Dimension $\chi$

The bond dimension $\chi$ is the central parameter of DMRG:

- **Accuracy:** Larger $\chi$ means the MPS can represent more entangled states. For a gapped system, $\chi = O(1)$ is often sufficient. At a critical point you need $\chi \to \infty$ for exact results.
- **Computational cost:** Each DMRG sweep costs $O(N \chi^3 d^2)$. Doubling $\chi$ increases the cost by a factor of 8.

The standard workflow: start with a small $\chi$ to get a rough ground state, then increase $\chi$ until observables converge. Convergence is exponential for gapped systems and polynomial for critical ones.

To put the cost in perspective:

| Method | Memory | Time per ground state | Accessible $N$ |
|---|---|---|---|
| ED (full diagonalisation) | $O(4^N)$ | $O(8^N)$ | $\lesssim 18$ |
| ED (Lanczos, lowest eigenvalue) | $O(2^N)$ | $O(k \cdot 2^N)$ | $\lesssim 40$ (with symmetry) |
| DMRG ($\chi$ fixed) | $O(N \chi^2 d)$ | $O(N \chi^3 d^2)$ per sweep | $\sim 10^3$–$10^4$ (gapped) |

The transition from Lanczos ED to DMRG is not just a quantitative improvement — it is a qualitative one. ED memory grows exponentially no matter what; DMRG memory grows linearly in $N$. For a gapped 1D system, a chain of 1000 sites with $\chi = 100$ is routine.

---

## 4. Truncation and the Discarded Weight

At each DMRG update step, after solving for the optimal tensor, we perform an SVD and truncate to bond dimension $\chi$. The **discarded weight**:

$$\epsilon = \sum_{\alpha > \chi} \lambda_\alpha^2$$

is monitored as a convergence diagnostic. If the discarded weight is small (say, $< 10^{-8}$), the truncation is not introducing significant error. If it is large, $\chi$ must be increased.

You can also set a target discarded weight (rather than a fixed $\chi$) and let DMRG adaptively choose the bond dimension at each site — useful for systems where entanglement varies significantly along the chain.

---

## 5. Convergence and Sweep Strategy

DMRG convergence is assessed by monitoring the energy across sweeps. A well-converged run shows:

- **Energy stabilisation:** The energy change per sweep drops below a target threshold (e.g. $10^{-10}$).
- **Declining discarded weight:** The maximum discarded weight over all sites decreases as $\chi$ increases.
- **Consistent observables:** Physical quantities like the magnetization profile and entanglement entropy no longer change between successive sweeps.

**Warm-up:** DMRG is typically initialised with a random MPS at small $\chi$, then progressively increased. Starting from a random state risks getting stuck in a local minimum, especially in the ordered phase of the TFIM where two ground states compete. A common strategy is to add a small symmetry-breaking field ($\delta \hat{\sigma}^z_{N/2}$) during the first few sweeps to steer DMRG into one sector, then remove it and continue.

**Two-site vs single-site DMRG:** In single-site DMRG, one tensor is updated at a time. In two-site DMRG, pairs of adjacent tensors are updated simultaneously, which allows the bond dimension to grow dynamically during the sweep. Two-site DMRG is more robust against getting stuck but more expensive per sweep. Most production codes use two-site DMRG with dynamic bond dimension expansion followed by single-site sweeps for refinement.

**Boundary conditions:** DMRG converges faster with open boundary conditions (OBC) than with periodic boundary conditions (PBC). With PBC, the effective Hamiltonian $H_{\text{eff}}$ involves long-range connections that are expensive to contract. For most purposes, OBC DMRG with a finite-size correction is preferred over PBC DMRG.

---

## 6. What DMRG Can and Cannot Do

**Where DMRG excels:**
- Ground states of 1D gapped systems: essentially exact for modest $\chi$.
- Quasi-1D systems (ladders, cylinders): tractable if the circumference is not too large.
- Systems with open boundary conditions: DMRG converges faster with OBC than PBC.
- Long chains: chains of $10^3$ or $10^4$ sites are routine for gapped systems.

**Where DMRG struggles:**
- 2D systems: the entanglement grows with the width $W$ as $S \sim W$, requiring $\chi \sim e^W$ — exponential in system width. In practice DMRG is limited to widths of $\sim 10$ for moderate accuracy.
- Critical systems: requires large $\chi$ that grows with system size.
- Excited states and dynamics: more challenging than ground states, though time-evolution algorithms (TEBD, TDVP) extend the framework.
- Fermionic systems in 2D or higher: though Jordan-Wigner transformation allows exact mapping to spin models in 1D.

The lecture 10 notebook puts these capabilities into practice using TeNPy, applying DMRG to the TFIM and comparing with the ED results from lectures 05–06.
