# Lecture Notes — Step 6: The 1D Transverse-Field Ising Model

---

## Overview

The Transverse-Field Ising Model (TFIM) is the simplest quantum model that has a phase transition. It is exactly solvable, connects directly to the 2D classical Ising model via a quantum-to-classical mapping, and serves as the paradigmatic example of a quantum phase transition. Everything we learn about it generalises to more complex quantum critical systems.

In this notebook we build the TFIM Hamiltonian explicitly from Pauli matrices and tensor products, compute its spectrum using exact diagonalisation, and observe the quantum phase transition in the ground state.

---

## 1. The Hamiltonian

The 1D TFIM on a chain of $N$ sites is:

$$\hat{H} = -J \sum_{i=1}^{N-1} \hat{\sigma}^z_i \hat{\sigma}^z_{i+1} - \Gamma \sum_{i=1}^{N} \hat{\sigma}^x_i$$

There are two competing terms:

- The **Ising coupling** $-J \hat{\sigma}^z_i \hat{\sigma}^z_{i+1}$: exactly the same as the classical Ising model, favouring aligned spins in the $z$-direction.
- The **transverse field** $-\Gamma \hat{\sigma}^x_i$: a magnetic field pointing in the $x$-direction, transverse to the Ising quantisation axis. This term flips spins from $|\uparrow\rangle$ to $|\downarrow\rangle$ (and vice versa).

The competition between these two terms drives the quantum phase transition.

---

## 2. The Two Limits

**Limit 1: $\Gamma = 0$ (pure Ising)**

The Hamiltonian is diagonal in the $z$-basis. The two ground states are $|\uparrow\uparrow\cdots\uparrow\rangle$ and $|\downarrow\downarrow\cdots\downarrow\rangle$, both with energy $-J(N-1)$. The system has a **doubly degenerate ordered phase** with $\langle \hat{\sigma}^z \rangle \neq 0$.

**Limit 2: $J = 0$ (pure transverse field)**

The Hamiltonian is diagonal in the $x$-basis. The ground state is the product state $|+\rangle^{\otimes N} = \left((|\uparrow\rangle + |\downarrow\rangle)/\sqrt{2}\right)^{\otimes N}$ — completely unentangled and unique. The order parameter $\langle \hat{\sigma}^z \rangle = 0$. This is the **disordered phase**.

---

## 3. The Quantum Phase Transition

As $\Gamma / J$ is tuned from 0 to $\infty$, the system undergoes a quantum phase transition at:

$$\Gamma_c = J$$

Below $\Gamma_c$: the Ising coupling dominates, the ground state is approximately $|\uparrow\uparrow\cdots\uparrow\rangle + |\downarrow\downarrow\cdots\downarrow\rangle$ (a cat state), and $\langle\hat{\sigma}^z\rangle \neq 0$ in the ordered phase.

Above $\Gamma_c$: the transverse field dominates, quantum fluctuations destroy the order, and the unique ground state has $\langle\hat{\sigma}^z\rangle = 0$.

At $\Gamma_c = J$ exactly: the system is critical. The energy gap closes, the correlation length diverges, and the system is described by the Ising CFT with central charge $c = 1/2$.

This is a **quantum phase transition**: it occurs at $T = 0$ and is driven by quantum fluctuations, not thermal ones. The transverse field causes spins to tunnel between $\uparrow$ and $\downarrow$, and when this tunnelling rate equals the coupling strength, the system reaches its quantum critical point.

---

## 4. What Makes This Quantum?

A classical phase transition is driven by thermal energy $k_BT$ competing with interaction energy $J$. Set $T = 0$ and the classical system freezes into its ground state — no transition as a function of other parameters.

A quantum phase transition occurs at $T = 0$ as a function of a coupling constant in the Hamiltonian. Quantum mechanics allows the system to exist in a superposition of states, and the energy of that superposition depends on the coupling. When the coupling crosses the critical value, the nature of the ground state changes qualitatively.

| | Classical (Ising, T sweep) | Quantum (TFIM, Γ sweep) |
|---|---|---|
| Control parameter | Temperature $T$ | Transverse field $\Gamma$ |
| Fluctuations | Thermal | Quantum (zero-point) |
| Occurs at | $T = T_c > 0$ | $\Gamma = \Gamma_c$, $T = 0$ |
| Order parameter | $\langle \sigma^z \rangle$ (thermal avg) | $\langle \hat\sigma^z \rangle$ (ground state) |

Despite these differences, the mathematical structure is identical. The FSS tools from notebook 02 apply directly to the TFIM — we use them in notebook 07.

---

## 5. Building the Hamiltonian from Pauli Matrices

For an operator acting on site $i$ of an $N$-site chain, we use the tensor product:

$$\hat{\sigma}^z_i = \underbrace{I \otimes \cdots \otimes I}_{i-1} \otimes \sigma^z \otimes \underbrace{I \otimes \cdots \otimes I}_{N-i}$$

where $I$ is the $2\times 2$ identity matrix. This gives a $2^N \times 2^N$ matrix that acts as $\sigma^z$ on site $i$ and as the identity everywhere else.

The full Hamiltonian is the sum of all such terms, built as a sparse matrix for computational efficiency.

---

## 6. The Energy Spectrum

**In the ordered phase** ($\Gamma < \Gamma_c$): The two lowest energy levels are nearly degenerate, separated by a gap $\Delta_1 \sim e^{-N/\xi}$ — exponentially small in system size. This reflects quantum tunnelling between the two ordered states. In the thermodynamic limit the gap closes and the levels become exactly degenerate — the hallmark of spontaneous symmetry breaking.

**In the disordered phase** ($\Gamma > \Gamma_c$): The ground state is unique and the gap to the first excited state is of order $|\Gamma - \Gamma_c|$ — finite.

**At the critical point** ($\Gamma = \Gamma_c$): The gap closes as $\Delta \sim L^{-z}$ (with $z = 1$ for the TFIM) — a power law vanishing in the thermodynamic limit. The spectrum at criticality has a structure dictated by the conformal field theory.

---

## 7. The Quantum-to-Classical Mapping

There is a profound connection between the 1D quantum TFIM and the 2D classical Ising model. It arises from the path integral representation of the quantum partition function.

Using the Suzuki-Trotter decomposition, the partition function $Z = \text{Tr}\, e^{-\beta \hat{H}}$ can be written as a sum over classical spin configurations on a 2D lattice — one spatial dimension (the chain) plus one imaginary time dimension. At $T = 0$ ($\beta \to \infty$), the imaginary time direction becomes infinite, and the effective 2D classical model is the 2D Ising model.

The consequence: **the critical exponents of the 1D TFIM are the same as those of the 2D classical Ising model**. The exponents $\beta = 1/8$, $\nu = 1$, and $\gamma = 7/4$ that we measured in notebook 02 apply directly to the quantum phase transition.

This quantum-to-classical mapping is one of the deepest ideas in condensed matter theory. It means that classical simulations and quantum simulations can access the same universal physics.

---

## 8. What Comes Next

In notebook 07, we apply the full finite-size scaling machinery to the TFIM quantum phase transition. The control parameter is $\Gamma/J$ instead of $T$, but the Binder cumulant, susceptibility scaling, and data collapse work identically.

After that, in notebook 08, we compute the **entanglement entropy** of the TFIM ground state — a purely quantum quantity that has no classical analogue, and whose logarithmic growth at the critical point is a fingerprint of the underlying CFT.
