# Lecture Notes — Lecture 6: Exact Diagonalisation II: Diagnostics

---

## Overview

Lecture 05 explained how to build the Hamiltonian and find its eigenstates. This lecture focuses on what to *do* with those eigenstates — the observables and diagnostics that reveal the physics of the quantum phase transition.

We cover the energy spectrum, ground-state expectation values, correlation functions, the transverse susceptibility, and how to connect ED data to the finite-size scaling framework from lecture 02. By the end, you will be able to map out the full phase diagram of the TFIM from ED data alone.

---

## 1. The Energy Spectrum

The full set of eigenvalues $\{E_n\}$ is a rich source of information about a quantum system.

**In the ordered phase** ($\Gamma < \Gamma_c$): The two lowest energy levels are nearly degenerate, separated by a gap $\Delta_1 \sim e^{-N/\xi}$ — exponentially small in system size. This reflects quantum tunnelling between the two ordered states. In the thermodynamic limit the gap closes and the levels become exactly degenerate — the hallmark of spontaneous symmetry breaking in a finite system.

**In the disordered phase** ($\Gamma > \Gamma_c$): The ground state is unique and the gap to the first excited state is of order $|\Gamma - \Gamma_c|$ — finite and growing away from the critical point.

**At the critical point** ($\Gamma = \Gamma_c$): The gap closes as a power law $\Delta \sim L^{-z}$ with $z = 1$ for the 1D TFIM. The spectrum at criticality has a structure dictated by the conformal field theory: the tower of energy levels above the ground state follows universal ratios predicted by CFT.

Plotting $E_n$ vs $\Gamma/J$ for several values of $n$ — the **level spectroscopy** — makes the transition visible: levels that are split in one phase become degenerate at the transition and then split again in the other phase. This is one of the cleanest finite-size signatures of a quantum phase transition.

---

## 2. Ground-State Expectation Values

Given the ground state $|E_0\rangle$, we compute expectation values of physical operators.

**Order parameter:**

$$m = \frac{1}{N}\sum_i \langle E_0 | \hat{\sigma}^z_i | E_0 \rangle$$

On a finite system with $\mathbb{Z}_2$ symmetry, $m = 0$ exactly — the ground state is a superposition of the two ordered states. To detect order, use $\langle m^2 \rangle$ or the Binder cumulant (see lecture 04).

**Transverse magnetization:**

$$m_x = \frac{1}{N}\sum_i \langle E_0 | \hat{\sigma}^x_i | E_0 \rangle$$

This is non-zero in both phases and can be computed directly as it is diagonal in the appropriate basis.

**Energy per site:**

$$e_0 = E_0 / N$$

For the 1D TFIM this can be compared against the exact analytic result from the Jordan-Wigner transformation. At the critical point $\Gamma = J$, the exact ground state energy per site (with open boundary conditions) is $e_0 = -J/\pi \csc(\pi/2N)$, which approaches $-J/\pi$ as $N \to \infty$. This provides a precise benchmark for the numerics.

---

## 3. Correlation Functions

Correlation functions measure the spatial structure of the ground state.

**Spin-spin correlation function:**

$$C(r) = \langle E_0 | \hat{\sigma}^z_i \hat{\sigma}^z_{i+r} | E_0 \rangle$$

In the ordered phase, $C(r) \to m^2 > 0$ for large $r$ — long-range order. In the disordered phase, $C(r) \sim e^{-r/\xi}$ — exponential decay with correlation length $\xi$. At the critical point, $C(r) \sim r^{-\eta}$ — algebraic decay with exponent $\eta = 1/4$ for the 1D TFIM.

**Structure factor:**

$$S(k) = \frac{1}{N} \sum_{i,j} e^{ik(i-j)} C(j-i)$$

The Fourier transform of the correlation function reveals the ordering wavevector. For the ferromagnetic TFIM, the structure factor peaks at $k = 0$ in the ordered phase.

Computing $C(r)$ from ED is straightforward: for each pair $(i, j)$, the operator $\hat{\sigma}^z_i \hat{\sigma}^z_j$ is diagonal in the computational basis. Its expectation value in the ground state is a sum over basis states weighted by $|c_n|^2$.

---

## 4. The Transverse Susceptibility

The transverse susceptibility measures the response of the ground state $z$-magnetization to a small perturbation. Via second-order perturbation theory:

$$\chi = \frac{1}{N} \sum_{n \neq 0} \frac{|\langle E_n | \hat{M}^z | E_0 \rangle|^2}{E_n - E_0}$$

where $\hat{M}^z = \sum_i \hat{\sigma}^z_i$.

At the quantum critical point, $\chi$ diverges with exponent $\gamma = 7/4$ (inherited from the 2D classical Ising model via the quantum-to-classical mapping). On a finite system it peaks near $\Gamma_c$ with a maximum scaling as:

$$\chi_{\max}(L) \sim L^{\gamma/\nu} = L^{7/4}$$

This finite-size scaling of the susceptibility peak provides an independent estimate of the ratio $\gamma/\nu$ from ED data.

Note that this formula sums over all excited states — in practice, we truncate to the first few hundred excited states. For large systems, this truncation introduces a systematic error that must be monitored.

---

## 5. Finite-Size Scaling with ED Data

ED gives exact results for small systems. The finite-size scaling framework from lecture 02 connects these small-system results to the thermodynamic limit.

**Locating the critical point:**

The Binder cumulant $U_4(L, \Gamma)$ for different $L$ all cross at $\Gamma_c$. The crossing point can be determined to high precision even from systems of $N = 8$–24.

The dimensionless gap ratio $L \cdot \Delta(L, \Gamma)$ also crosses at $\Gamma_c$ — an independent estimate of the critical point.

**Extracting critical exponents:**

From the gap: fit $\Delta(L, \Gamma_c) = A \cdot L^{-z}$ to extract $z$.

From the susceptibility peak: fit $\chi_{\max}(L) = B \cdot L^{\gamma/\nu}$ to extract $\gamma/\nu$.

From the data collapse: with $\tilde{m} = L^{\beta/\nu} \langle |\hat{m}^z| \rangle$ and $x = (\Gamma - \Gamma_c) L^{1/\nu}$, optimise $\Gamma_c$, $\beta/\nu$, and $\nu$ until all system sizes collapse onto a single curve.

For the 1D TFIM, the known exponents are $z = 1$, $\nu = 1$, $\beta = 1/8$, $\gamma = 7/4$ — the same as the 2D classical Ising model.

---

## 6. What ED Reveals about the Phase Diagram

Taken together, the diagnostics above give a complete picture of the TFIM phase diagram:

| Observable | Ordered phase ($\Gamma < \Gamma_c$) | Critical point | Disordered phase ($\Gamma > \Gamma_c$) |
|---|---|---|---|
| Spectral gap $\Delta$ | $\sim e^{-N/\xi}$ | $\sim L^{-1}$ | Finite, $\sim |\Gamma - \Gamma_c|$ |
| Order parameter $m$ | Nonzero (for symmetry-broken state) | 0 | 0 |
| Correlations $C(r)$ | Long-range | Algebraic $r^{-1/4}$ | Exponential |
| Entanglement entropy | Area law (constant) | $\frac{1}{6}\log L$ | Area law (constant) |

The entanglement entropy column anticipates lecture 07: it is the one observable in this table that ED can compute but that has no analogue in classical statistical mechanics. It is the bridge to tensor network methods.

By the time you have worked through this lecture, you have all the tools needed to characterise a quantum phase transition from first principles. The limitation is scale: ED is exact but expensive. Lectures 08–10 show how tensor network methods extend these diagnostics to system sizes far beyond ED reach.
