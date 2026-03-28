# Lecture Notes — Step 8: Entanglement Entropy and the Area Law

---

## Overview

Entanglement is one of the defining features of quantum mechanics with no classical counterpart. Two particles can be correlated in a way that cannot be explained by any classical probability distribution — their joint state is described by a single wavefunction that cannot be factored into a product of individual states.

In many-body physics, entanglement is not just a curiosity — it is a diagnostic tool. The amount of entanglement in the ground state of a quantum system encodes information about its phase. In gapped phases the entanglement is local (the **area law**). At a critical point the entanglement grows logarithmically — a fingerprint that identifies the universality class through the **central charge** of the underlying conformal field theory.

This notebook connects the physics of quantum phase transitions to the mathematics that will make tensor networks possible.

---

## 1. Bipartite Entanglement

Consider a chain of $N$ spins and divide it into two parts: subsystem $A$ (the first $\ell$ sites) and subsystem $B$ (the remaining $N - \ell$ sites). We want to quantify how entangled $A$ and $B$ are in the ground state $|E_0\rangle$.

If the state is a product state — like the ground state of the TFIM at $\Gamma \to \infty$:

$$|E_0\rangle = |\psi_A\rangle \otimes |\psi_B\rangle$$

Subsystems $A$ and $B$ are completely independent. There is zero entanglement.

If the state is entangled — like the ground state near criticality — then $|E_0\rangle$ cannot be written as a product. Measuring a spin in $A$ instantaneously affects the probabilities of outcomes in $B$.

---

## 2. The Reduced Density Matrix

To characterise the state of subsystem $A$ alone, we trace out subsystem $B$:

$$\rho_A = \text{Tr}_B |E_0\rangle\langle E_0|$$

The reduced density matrix $\rho_A$ is a $2^\ell \times 2^\ell$ Hermitian matrix with non-negative eigenvalues summing to 1.

**Key point:** If $\rho_A$ is a pure state (rank 1), then $A$ is not entangled with $B$. If $\rho_A$ has multiple non-zero eigenvalues, the subsystems are entangled. The more spread out the eigenvalue distribution, the more entangled they are.

---

## 3. The Schmidt Decomposition

Any pure state of a bipartite system can be written in the Schmidt form:

$$|E_0\rangle = \sum_{\alpha=1}^{\chi} \lambda_\alpha |\alpha\rangle_A |\alpha\rangle_B$$

where $|\alpha\rangle_A$ and $|\alpha\rangle_B$ are orthonormal sets, $\lambda_\alpha \geq 0$, and $\sum_\alpha \lambda_\alpha^2 = 1$. The $\lambda_\alpha$ are the **Schmidt coefficients** and $\chi$ is the **Schmidt rank**.

The Schmidt decomposition is obtained by reshaping the coefficient tensor $c_{ij}$ into a matrix and performing a singular value decomposition (SVD):

$$C = U \Sigma V^\dagger$$

The singular values $\sigma_\alpha$ are the Schmidt coefficients $\lambda_\alpha$. This SVD structure is the foundation of matrix product states.

---

## 4. Entanglement Entropy

The entanglement entropy is the von Neumann entropy of the reduced density matrix:

$$S_A = -\text{Tr}(\rho_A \log \rho_A) = -\sum_\alpha \lambda_\alpha^2 \log \lambda_\alpha^2$$

Properties:
- $S_A = 0$ for a product state (one $\lambda_\alpha = 1$, all others zero).
- $S_A = \log \chi$ for a maximally entangled state (all $\lambda_\alpha = 1/\sqrt{\chi}$).
- $S_A = S_B$: entanglement is symmetric.
- $S_A \leq \ell \log 2$: bounded by the logarithm of the smaller subsystem dimension.

---

## 5. The Area Law

For ground states of **gapped** local Hamiltonians, the entanglement entropy obeys the **area law**:

$$S_A \sim |\partial A|$$

where $|\partial A|$ is the size of the boundary between $A$ and $B$. In 1D, the boundary consists of at most 2 points regardless of subsystem size, so the area law says $S_A$ saturates to a constant:

$$S_A \to S_{\max} = \text{const} \quad \text{(1D gapped system)}$$

Intuitively: in a gapped phase, correlations decay exponentially over the length scale $\xi$. Only spins near the boundary between $A$ and $B$ are significantly entangled with each other. The entanglement is local.

The area law has profound consequences for numerical methods. If a state satisfies the area law, it can be efficiently represented as a Matrix Product State with a bond dimension $\chi$ that depends only on the entanglement entropy — not on the system size. This is why DMRG (notebook 10) works so well for gapped 1D systems.

---

## 6. Critical Systems: Logarithmic Scaling

At a quantum critical point, the correlation length diverges and the area law is violated. For a 1D critical system described by a conformal field theory, the entanglement entropy scales as:

$$S_A(\ell) = \frac{c}{3} \log \ell + \text{const}$$

where $c$ is the **central charge** of the CFT. For a finite chain of length $L$ with periodic boundary conditions:

$$S_A(\ell) = \frac{c}{3} \log\!\left(\frac{L}{\pi} \sin\!\frac{\pi \ell}{L}\right) + \text{const}$$

This formula was derived by Calabrese and Cardy (2004) and is one of the most important results in modern quantum many-body physics. It connects a local numerical measurement to a universal quantity (the central charge) that classifies the universality class of the critical theory.

---

## 7. The Central Charge

The central charge $c$ is a fundamental property of a CFT. It characterises the number of gapless degrees of freedom:

- A single free boson: $c = 1$.
- A single free Majorana fermion (the field theory of the Ising CFT): $c = 1/2$.

The TFIM at its quantum critical point $\Gamma = J$ is described by the Ising CFT with $c = 1/2$. We therefore expect:

$$S_A(\ell) = \frac{1}{6} \log \ell + \text{const}$$

Fitting this logarithm from ED data is one of the cleanest ways to identify the universality class of a quantum critical point.

---

## 8. Entanglement across the Phase Diagram

By computing $S_A(\ell)$ for several values of $\Gamma/J$, we see three distinct behaviours:

**Ordered phase** ($\Gamma < \Gamma_c$): $S_A$ saturates to a constant — area law. The constant reflects the two-fold degeneracy of the ground state.

**Critical point** ($\Gamma = \Gamma_c$): $S_A \sim \frac{1}{6} \log \ell$ — logarithmic growth, consistent with $c = 1/2$.

**Disordered phase** ($\Gamma > \Gamma_c$): $S_A$ again saturates, but to a smaller constant (the ground state is a non-degenerate product state in the $\Gamma \to \infty$ limit).

This three-way distinction — two area law phases separated by a critical point with logarithmic entanglement — is a general feature of 1D quantum systems and can be used as a diagnostic tool even when the order parameter is not known.

---

## 9. From Entanglement to Tensor Networks

The entanglement entropy tells us how much classical information is needed to describe a quantum state. For a state satisfying the area law, the Schmidt decomposition has only $\chi = e^{S_{\max}}$ significant singular values. Truncating to these $\chi$ values gives a faithful representation of the state as a **Matrix Product State (MPS)**.

This truncation is not an approximation for gapped systems — the discarded singular values are exponentially small. For critical systems, $\chi$ must grow as $\ell^{c/6}$ to maintain fixed accuracy — polynomial rather than exponential.

The entanglement structure thus dictates the optimal tensor network representation. Understanding the area law and its violation at criticality is the theoretical foundation for everything in notebooks 09 and 10.
