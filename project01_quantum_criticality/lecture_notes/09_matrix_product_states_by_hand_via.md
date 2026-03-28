# Lecture Notes — Step 9: Matrix Product States by Hand via SVD Truncation

---

## Overview

In notebook 08 we saw that the entanglement entropy controls how efficiently a quantum state can be represented. In this notebook we implement that idea directly: we take a many-body wavefunction obtained from exact diagonalisation, and decompose it into a **Matrix Product State (MPS)** by performing a sequence of singular value decompositions.

This notebook is the bridge between exact diagonalisation and DMRG. By the end you will understand what an MPS is, why it is an efficient representation for low-entanglement states, and exactly what information is lost when we truncate the bond dimension.

---

## 1. The Problem with the Full Wavefunction

Consider the ground state of a 20-site TFIM chain. The full wavefunction has $2^{20} \approx 10^6$ complex coefficients. For 40 sites this grows to $10^{12}$. The exponential growth of the Hilbert space is the fundamental obstacle to quantum many-body simulation.

But we know from notebook 08 that this enormous wavefunction is not random — it has structure. For a gapped system, entanglement is local: only spins near a cut are significantly correlated across it. Most of those $10^6$ coefficients are nearly zero, or nearly determined by a small number of dominant correlations.

The MPS representation makes this structure explicit and allows us to work directly with the physically relevant degrees of freedom.

---

## 2. What Is a Matrix Product State?

An MPS is a way of writing a many-body wavefunction as a product of matrices:

$$|\psi\rangle = \sum_{s_1, \ldots, s_N} A^{s_1}_1 A^{s_2}_2 \cdots A^{s_N}_N\, |s_1 s_2 \cdots s_N\rangle$$

Here each $A^{s_i}_i$ is a matrix of dimension $\chi_{i-1} \times \chi_i$ (with $\chi_0 = \chi_N = 1$ at the boundaries). The physical index $s_i \in \{0, 1\}$ selects one of two matrices at each site. The coefficient of the basis state $|s_1 s_2 \cdots s_N\rangle$ is the product of matrices $A^{s_1}_1 A^{s_2}_2 \cdots A^{s_N}_N$, which is a scalar because the boundary dimensions are 1.

The maximum matrix dimension — the **bond dimension** $\chi$ — controls the expressiveness of the MPS. An MPS with bond dimension $\chi$ has $O(N \chi^2 d)$ parameters (where $d = 2$ for spin-$1/2$), which is polynomial in $N$ and $\chi$.

---

## 3. Every State Is an MPS (Exactly)

Every quantum state can be written exactly as an MPS — the bond dimension just grows. For a system of $N$ sites with local Hilbert space dimension $d$, the exact MPS representation has bond dimension up to $d^{N/2}$, which is exponential.

The approximation comes when we **truncate**: we keep only the $\chi$ largest singular values at each bond and discard the rest. If the discarded singular values are small (as they are for gapped states), the truncation error is negligible and the MPS with bond dimension $\chi$ is an excellent approximation.

---

## 4. Constructing an MPS by Sequential SVD

Given a full wavefunction $|\psi\rangle$ with coefficient tensor $\psi_{s_1 s_2 \cdots s_N}$, we convert it to MPS form by a sequence of SVDs:

**Step 1:** Reshape the coefficient tensor as a $d \times d^{N-1}$ matrix $\Psi_1$ by grouping the first index against all others. Perform SVD: $\Psi_1 = U_1 \Sigma_1 V_1^\dagger$. The left singular vectors $U_1$ become the tensors $A^{s_1}_1$. The singular values $\Sigma_1$ are the Schmidt coefficients for the cut between site 1 and the rest.

**Step 2:** Form $\Psi_2 = \Sigma_1 V_1^\dagger$, reshape as $(\chi_1 d) \times d^{N-2}$, and SVD again. This gives $A^{s_2}_2$ and moves the Schmidt structure to the next cut.

**Repeat** until all sites have been processed. The result is the set of MPS tensors $\{A^{s_i}_i\}$.

This procedure is exact when we keep all singular values. To get an approximate MPS with bond dimension $\chi$, we truncate each SVD to the $\chi$ largest singular values.

---

## 5. The Truncation Error

At each cut between sites $i$ and $i+1$, the truncation error is:

$$\epsilon_i = \sum_{\alpha > \chi} \lambda_\alpha^2$$

where $\lambda_\alpha$ are the discarded Schmidt coefficients. This is the squared norm of the component of the wavefunction that is lost.

The entanglement entropy directly tells us how quickly this error decays with $\chi$:

- If $S_A = 0$ (product state): $\chi = 1$ is exact.
- For a state obeying the area law: the Schmidt coefficients decay exponentially. $\chi = O(1)$ coefficients capture nearly all the weight.
- At a critical point: the Schmidt coefficients decay as a power law. $\chi$ must grow as $\ell^{c/6}$ to maintain fixed accuracy — polynomial growth, but unbounded.

---

## 6. Canonical Form

An MPS can be brought into **canonical form** where the tensors satisfy orthogonality conditions:

$$\sum_{s_i} (A^{s_i}_i)^\dagger A^{s_i}_i = I \quad \text{(left-canonical)}$$

In canonical form, overlaps and expectation values can be computed efficiently by contracting the network from left to right. The singular values at each cut are directly accessible — they are the diagonal entries of the $\Sigma$ matrices sitting between left- and right-canonical blocks.

The sequential SVD construction naturally produces a left-canonical MPS.

---

## 7. Computing Expectation Values from MPS

Once you have an MPS in canonical form, computing a local observable $\langle \hat{O}_i \rangle$ costs $O(N \chi^2 d^2)$ — linear in $N$ and polynomial in $\chi$. This is the key computational advantage: instead of working with $2^N$-dimensional vectors, we work with $O(N)$ tensors of size $O(\chi^2)$.

The entanglement entropy of a subsystem can be read directly from the singular values at the cut: $S_A = -\sum_\alpha \lambda_\alpha^2 \log \lambda_\alpha^2$.

---

## 8. What Survives Truncation

**In the gapped phases** ($\Gamma \neq \Gamma_c$): The Schmidt spectrum decays rapidly. Even $\chi = 4$ or $\chi = 8$ captures the ground state energy to high precision. The truncation error is tiny.

**At the critical point** ($\Gamma = \Gamma_c$): The Schmidt spectrum decays slowly. Even $\chi = 32$ may not be enough for a long chain. The truncation error decreases slowly with $\chi$, following a power law rather than exponential convergence.

This is not a failure of the method — it is physics. The critical ground state genuinely has more entanglement than the gapped ones, and any finite-$\chi$ representation necessarily misses some of it.

---

## 9. What Comes Next

In notebook 10 we flip the perspective: instead of starting from a full ED wavefunction and truncating it to MPS form, we directly optimise an MPS ansatz to find the ground state — without ever constructing the exponentially large full wavefunction. This is the DMRG algorithm, and it is what makes tensor network methods powerful for systems far beyond ED reach.

The sequential SVD decomposition you performed in this notebook is not just a pedagogical exercise — it is the step that runs inside DMRG every time a site tensor is updated. Understanding it deeply means understanding DMRG.
