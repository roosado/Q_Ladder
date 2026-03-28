# Lecture Notes — Step 11: The Kitaev Chain — Topology and Majorana Edge Modes

---

## Overview

The Kitaev chain is the final step in this series, and it brings together everything that came before — phase transitions, energy gaps, exact diagonalisation, and tensor networks — while introducing something genuinely new: **topological order**.

The two phases of the Kitaev chain look the same from the perspective of any local order parameter. They have the same symmetry. The distinction between them is **global** — it is a property of the entire ground state wavefunction that cannot be changed without closing the energy gap. At the boundary between the two phases, **Majorana zero modes** appear: exotic quasiparticles that are their own antiparticles, and that carry quantum information in a way that is protected from local errors.

---

## 1. The Hamiltonian

The Kitaev chain is a model of spinless fermions on a 1D lattice with nearest-neighbour hopping and p-wave superconducting pairing:

$$\hat{H} = -\mu \sum_i \hat{c}^\dagger_i \hat{c}_i - t \sum_i (\hat{c}^\dagger_{i+1} \hat{c}_i + \hat{c}_i^\dagger \hat{c}_{i+1}) + \Delta \sum_i (\hat{c}_{i+1} \hat{c}_i + \hat{c}^\dagger_i \hat{c}^\dagger_{i+1})$$

The three parameters are:
- $\mu$: the chemical potential (controls filling)
- $t$: the hopping amplitude
- $\Delta$: the p-wave pairing amplitude

The pairing term $\Delta \hat{c}_{i+1} \hat{c}_i$ destroys two fermions simultaneously. In a superconductor, pairs of electrons condense and particle number is not conserved — the ground state is a superposition of states with different particle numbers. This is the essential physics of superconductivity encoded in the minimal 1D model.

---

## 2. Majorana Fermions

A **Majorana fermion** is a particle that is its own antiparticle. For a regular (Dirac) fermion, $\hat{c} \neq \hat{c}^\dagger$. For a Majorana fermion $\hat{\gamma}$:

$$\hat{\gamma} = \hat{\gamma}^\dagger$$

We can always decompose a complex fermion into two Majorana fermions:

$$\hat{c}_i = \frac{1}{2}(\hat{\gamma}_{2i-1} + i\,\hat{\gamma}_{2i}), \quad \hat{c}^\dagger_i = \frac{1}{2}(\hat{\gamma}_{2i-1} - i\,\hat{\gamma}_{2i})$$

Each site $i$ contributes two Majorana operators $\hat{\gamma}_{2i-1}$ and $\hat{\gamma}_{2i}$, satisfying $\{\hat{\gamma}_\alpha, \hat{\gamma}_\beta\} = 2\delta_{\alpha\beta}$ and $\hat{\gamma}_\alpha^2 = 1$.

Rewriting the Kitaev Hamiltonian in Majorana language reveals the two phases with striking clarity.

---

## 3. The Two Phases

**The trivial phase** ($|\mu| > 2t$):

The dominant Majorana pairing is on-site: $\hat{\gamma}_{2i-1}$ pairs with $\hat{\gamma}_{2i}$ on the same site. Each complex fermion on site $i$ is intact. There is a bulk gap to all excitations, the ground state is unique, and there are no unpaired Majorana modes at the chain ends.

**The topological phase** ($|\mu| < 2t$, $\Delta \neq 0$):

The dominant pairing is between nearest neighbours: $\hat{\gamma}_{2i}$ on site $i$ pairs with $\hat{\gamma}_{2i+1}$ on site $i+1$. This leaves $\hat{\gamma}_1$ (left end) and $\hat{\gamma}_{2N}$ (right end) **unpaired**. These two Majorana modes do not appear in the Hamiltonian — they have exactly zero energy. They are **Majorana zero modes**.

In the perfectly tuned limit $\mu = 0$, $|\Delta| = t$, the Hamiltonian becomes $\hat{H} = -2t \sum_i i \hat{\gamma}_{2i} \hat{\gamma}_{2i+1}$ — the left and right Majorana modes $\hat{\gamma}_1$ and $\hat{\gamma}_{2N}$ are exactly absent from $\hat{H}$.

---

## 4. The Topological Phase Transition

The transition between the trivial and topological phases occurs at $|\mu| = 2t$. At this line, the bulk gap closes — the excitation spectrum becomes gapless — and then reopens as you cross the transition. This gap closing and reopening is the hallmark of a topological phase transition.

Unlike the Ising or TFIM phase transitions, there is no local order parameter that distinguishes the trivial and topological phases. Both phases:
- Have the same spatial symmetry.
- Have the same local structure (no spin, since this is a spinless model).
- Have a nonzero bulk gap.

The distinction is entirely **non-local**: it is captured by a **topological invariant**, a number that can only change when the gap closes.

---

## 5. The Topological Invariant

For the Kitaev chain, the topological invariant is a $\mathbb{Z}_2$ quantity taking values "trivial" and "topological".

One way to compute it: the **Pfaffian of the Hamiltonian** in Majorana form at zero momentum. In the topological phase, the Pfaffian is negative; in the trivial phase, it is positive. This sign cannot change without passing through zero — which requires the gap to close.

Another formulation: the **winding number** of the momentum-space Hamiltonian $h(\mathbf{k})$ as $\mathbf{k}$ traverses the Brillouin zone. In the topological phase the Hamiltonian winds around the origin once; in the trivial phase it does not. The winding number is an integer-valued topological property.

---

## 6. Bulk-Boundary Correspondence

The most important property of topological phases is the **bulk-boundary correspondence**: the topological invariant of the bulk determines the number of protected zero-energy modes at the boundary.

- Topological invariant $= 0$ (trivial): no boundary modes.
- Topological invariant $= 1$ (topological): one Majorana zero mode at each end.

The "protection" is physical: to gap out a Majorana zero mode, you would need a term in the Hamiltonian that couples $\hat{\gamma}_1$ and $\hat{\gamma}_{2N}$. Any such coupling must involve a string of operators spanning the entire chain — it is exponentially suppressed in the chain length. For a macroscopically long chain, the zero modes are effectively degenerate to any local perturbation.

---

## 7. The Non-Local Qubit

The two Majorana zero modes $\hat{\gamma}_1$ and $\hat{\gamma}_{2N}$ can be combined into a single complex fermion:

$$\hat{f} = \frac{1}{2}(\hat{\gamma}_1 + i\,\hat{\gamma}_{2N})$$

This fermion is either occupied or empty, giving a two-fold ground state degeneracy. The two states $|0\rangle$ and $|1\rangle$ form a **qubit**.

The crucial feature: this qubit is **non-local**. Its two basis states differ in the occupation of a mode split between the left and right ends of the chain, separated by a macroscopic distance. Any local perturbation on either end affects only one of the two Majorana operators and cannot rotate the qubit state. The qubit is protected from local noise by its non-locality.

This is the basis for **topological quantum computing**: encode quantum information in pairs of Majorana zero modes, and the information is automatically protected from local decoherence. Quantum gates are implemented by braiding the Majorana modes — physically moving them around each other — which implements unitary transformations depending only on the topology of the braiding path, not on the details of how the braid was performed.

---

## 8. Computing the Phase Diagram

In the notebook, we map out the Kitaev chain phase diagram in the $(\mu/t, \Delta/t)$ plane using both ED and TeNPy DMRG.

**From the spectrum:** In the topological phase, the two lowest eigenvalues of $\hat{H}$ with open boundary conditions are nearly degenerate (split only by the exponentially small overlap of the two Majorana modes). In the trivial phase, there is a unique ground state with a finite gap.

**From the topological invariant:** The Pfaffian or winding number can be computed from the Hamiltonian in momentum space (for a periodic chain). We verify it matches the prediction from the edge mode degeneracy.

**From the edge mode:** The Majorana zero mode is localised at the chain ends. We visualise this by computing the local density of states using the eigenvectors of $\hat{H}$. In the topological phase, the lowest eigenstate has weight concentrated at the ends; in the trivial phase it is distributed throughout the bulk.

---

## 9. Connection to Experiment and Active Research

The Kitaev chain is directly relevant to experimental efforts to build topological quantum computers. Candidate physical systems include:

- **Semiconductor nanowires** (InAs, InSb) with strong spin-orbit coupling, proximity-coupled to an s-wave superconductor and placed in a magnetic field. The effective low-energy physics is described by the Kitaev chain.
- **Magnetic atomic chains** on superconducting surfaces.
- **Chains of Josephson junctions.**

Signatures of Majorana zero modes — specifically, a robust zero-bias conductance peak in tunnelling spectroscopy — have been observed experimentally, though definitively establishing the topological origin of these signatures remains an active and contested research area.

---

## 10. Where the Series Has Brought You

| Notebook | Physics | Method |
|---|---|---|
| 01 | 2D Ising model, phase transition | Metropolis Monte Carlo |
| 02 | Critical exponents, universality | Finite-size scaling |
| 03 | XY model, topological defects | Monte Carlo, vortex counting |
| 04 | First-order transitions, Landau theory | Monte Carlo with field |
| 05 | Quantum spins, many-body Hilbert space | Exact diagonalisation |
| 06 | Quantum phase transition (TFIM) | ED, Hamiltonian construction |
| 07 | Quantum FSS, spectral gap | ED, scaling collapse |
| 08 | Entanglement entropy, area law | ED, Schmidt decomposition |
| 09 | Matrix product states, SVD truncation | ED + MPS decomposition |
| 10 | DMRG, large-scale simulation | TeNPy |
| 11 | Topological phase, Majorana modes | ED + TeNPy |

The series has taken you from thermal fluctuations in a classical model all the way to topological quantum phases and Majorana fermions — a journey that mirrors the development of condensed matter physics over the past three decades. The computational and conceptual tools you have built — Monte Carlo, finite-size scaling, exact diagonalisation, tensor networks — are the same ones used in active research today.
