# Lecture Notes — Lecture 3: The 1D Quantum Transverse-Field Ising Model

---

## Overview

The Transverse-Field Ising Model (TFIM) is the simplest quantum model that has a phase transition. It is exactly solvable, connects directly to the 2D classical Ising model via a quantum-to-classical mapping, and serves as the paradigmatic example of a quantum phase transition. Everything we learn about it generalises to more complex quantum critical systems.

In this lecture we make the conceptual leap from classical to quantum spins, build the TFIM Hamiltonian explicitly from Pauli matrices and tensor products, and understand the two competing phases and the quantum-to-classical mapping. The spectrum and finite-size diagnostics are developed in lectures 05–06; the phase transition itself is quantified in lecture 04.

---

## 1. From Classical Spins to Quantum Operators

A classical Ising spin $s_i \in \{+1, -1\}$ is replaced by a quantum spin-$1/2$ operator $\hat{\vec{S}}_i = (\hat{S}_i^x, \hat{S}_i^y, \hat{S}_i^z)$. Each component acts on a two-dimensional local Hilbert space spanned by $|\uparrow\rangle$ and $|\downarrow\rangle$. In matrix form, using the Pauli matrices:

$$\hat{\sigma}^x = \begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad \hat{\sigma}^y = \begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad \hat{\sigma}^z = \begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

The defining property of quantum spin operators is the commutation relation:

$$[\hat{\sigma}^x, \hat{\sigma}^y] = 2i\hat{\sigma}^z$$

and cyclic permutations. This non-commutativity is what makes quantum spins qualitatively different from classical ones: the three components cannot be simultaneously specified. Measuring $\sigma^z$ disturbs $\sigma^x$ and $\sigma^y$. The consequence is that a system can simultaneously fluctuate in the $z$-direction even at $T = 0$ — this is quantum fluctuation.

For an $N$-site chain, the total Hilbert space is the tensor product of $N$ two-dimensional spaces:

$$\mathcal{H} = \mathbb{C}^2 \otimes \mathbb{C}^2 \otimes \cdots \otimes \mathbb{C}^2 \cong \mathbb{C}^{2^N}$$

Its dimension is $2^N$ — exponential in system size. A general state is a superposition over all $2^N$ classical spin configurations, with $2^N$ complex coefficients. This exponential growth is the fundamental challenge of quantum many-body physics.

---

## 2. The Hamiltonian

The 1D TFIM on a chain of $N$ sites is:

$$\hat{H} = -J \sum_{i=1}^{N-1} \hat{\sigma}^z_i \hat{\sigma}^z_{i+1} - \Gamma \sum_{i=1}^{N} \hat{\sigma}^x_i$$

There are two competing terms:

- The **Ising coupling** $-J \hat{\sigma}^z_i \hat{\sigma}^z_{i+1}$: exactly the same as the classical Ising model, favouring aligned spins in the $z$-direction.
- The **transverse field** $-\Gamma \hat{\sigma}^x_i$: a magnetic field pointing in the $x$-direction, transverse to the Ising quantisation axis. This term causes spins to tunnel between $|\uparrow\rangle$ and $|\downarrow\rangle$.

The competition between these two terms drives the quantum phase transition at $\Gamma_c = J$.

---

## 3. The Two Limits

**Limit 1: $\Gamma = 0$ (pure Ising)**

The Hamiltonian is diagonal in the $z$-basis. The two ground states are $|\uparrow\uparrow\cdots\uparrow\rangle$ and $|\downarrow\downarrow\cdots\downarrow\rangle$, both with energy $-J(N-1)$. The system has a **doubly degenerate ordered phase** with $\langle \hat{\sigma}^z \rangle \neq 0$.

**Limit 2: $J = 0$ (pure transverse field)**

The Hamiltonian is diagonal in the $x$-basis. The ground state is the product state $|+\rangle^{\otimes N} = \left((|\uparrow\rangle + |\downarrow\rangle)/\sqrt{2}\right)^{\otimes N}$ — completely unentangled and unique. The order parameter $\langle \hat{\sigma}^z \rangle = 0$. This is the **disordered phase**.

As $\Gamma / J$ is tuned from 0 to $\infty$, the system undergoes a quantum phase transition at $\Gamma_c = J$. At the critical point the energy gap closes, the correlation length diverges, and the system is described by the Ising CFT with central charge $c = 1/2$.

---

## 4. Building the Hamiltonian from Pauli Matrices

For an operator acting on site $i$ of an $N$-site chain, we use the tensor product:

$$\hat{\sigma}^z_i = \underbrace{I \otimes \cdots \otimes I}_{i-1} \otimes \sigma^z \otimes \underbrace{I \otimes \cdots \otimes I}_{N-i}$$

where $I$ is the $2\times 2$ identity matrix. This gives a $2^N \times 2^N$ matrix that acts as $\sigma^z$ on site $i$ and as the identity everywhere else.

The full Hamiltonian is built as a sparse matrix by summing all such terms. For $N$ sites, we have $N-1$ two-body (Ising coupling) terms and $N$ one-body (transverse field) terms, giving at most $O(N \cdot 2^N)$ non-zero entries in a $2^N \times 2^N$ matrix.

In code, this is naturally implemented using `scipy.sparse` and the Kronecker product `kron`. We build each site operator $\hat{\sigma}^\alpha_i$ as a sparse matrix and accumulate the Hamiltonian as a sum.

---

## 5. What Makes This Quantum?

A classical phase transition is driven by thermal energy $k_BT$ competing with interaction energy $J$. Set $T = 0$ and the classical system freezes into its ground state — no transition as a function of other parameters.

A quantum phase transition occurs at $T = 0$ as a function of a coupling constant in the Hamiltonian. Quantum mechanics allows the system to exist in a superposition of states, and the energy of that superposition depends on the coupling. When the coupling crosses the critical value, the nature of the ground state changes qualitatively.

| | Classical (Ising, $T$ sweep) | Quantum (TFIM, $\Gamma$ sweep) |
|---|---|---|
| Control parameter | Temperature $T$ | Transverse field $\Gamma$ |
| Fluctuations | Thermal | Quantum (zero-point) |
| Occurs at | $T = T_c > 0$ | $\Gamma = \Gamma_c$, $T = 0$ |
| Order parameter | $\langle \sigma^z \rangle$ (thermal avg) | $\langle \hat\sigma^z \rangle$ (ground state) |

Despite these differences, the mathematical structure is identical. The FSS tools from lecture 02 apply directly to the TFIM — we use them in lecture 04.

---

## 6. The Quantum-to-Classical Mapping

There is a profound connection between the 1D quantum TFIM and the 2D classical Ising model. It arises from the path integral representation of the quantum partition function.

Using the Suzuki-Trotter decomposition, the partition function $Z = \text{Tr}\, e^{-\beta \hat{H}}$ can be written as a sum over classical spin configurations on a 2D lattice — one spatial dimension (the chain) plus one imaginary time dimension. At $T = 0$ ($\beta \to \infty$), the imaginary time direction becomes infinite, and the effective 2D classical model is the 2D Ising model.

The consequence: **the critical exponents of the 1D TFIM are the same as those of the 2D classical Ising model**. The exponents $\beta = 1/8$, $\nu = 1$, and $\gamma = 7/4$ that we measured in lecture 02 apply directly to the quantum phase transition.

This quantum-to-classical mapping is one of the deepest ideas in condensed matter theory. It means that classical simulations and quantum simulations can access the same universal physics. In $d$ spatial dimensions, a quantum system maps to a classical system in $d + z$ dimensions, where $z$ is the dynamical critical exponent ($z = 1$ for the TFIM).
