# Lecture Notes — Step 5: Classical to Quantum — The Heisenberg Model and Exact Diagonalisation

---

## Overview

Until now every spin in our models has been a classical variable — a number ($\pm 1$) or an angle ($\theta$). In this notebook we make the jump to quantum mechanics. Spins become quantum operators with non-commuting components, and the wavefunction of the system is a superposition over all $2^N$ classical spin configurations. This single change opens up qualitatively new physics: quantum fluctuations, entanglement, and the possibility of phases that have no classical counterpart.

We also introduce **exact diagonalisation (ED)** — the method of directly computing the eigenvalues and eigenvectors of the Hamiltonian matrix. ED is the most straightforward quantum many-body method and the natural starting point before tensor networks.

---

## 1. From Classical Spins to Quantum Operators

A classical Ising spin $s_i \in \{+1, -1\}$ is replaced by a quantum spin-$1/2$ operator $\hat{\vec{S}}_i = (\hat{S}_i^x, \hat{S}_i^y, \hat{S}_i^z)$. Each component acts on a two-dimensional local Hilbert space spanned by $|\uparrow\rangle$ and $|\downarrow\rangle$. In matrix form, using the Pauli matrices:

$$\hat{S}^x = \frac{1}{2}\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}, \quad \hat{S}^y = \frac{1}{2}\begin{pmatrix} 0 & -i \\ i & 0 \end{pmatrix}, \quad \hat{S}^z = \frac{1}{2}\begin{pmatrix} 1 & 0 \\ 0 & -1 \end{pmatrix}$$

The defining property of quantum spin operators is the commutation relation:

$$[\hat{S}^x, \hat{S}^y] = i\hat{S}^z$$

and cyclic permutations. This is what makes quantum spins qualitatively different from classical ones: the three components cannot be simultaneously specified. Measuring $S^z$ disturbs $S^x$ and $S^y$. This is quantum mechanics, and it has no classical analogue.

---

## 2. The Heisenberg Hamiltonian

The quantum Heisenberg model replaces the classical spin products with quantum operator products:

$$\hat{H} = -J \sum_{\langle i,j \rangle} \hat{\vec{S}}_i \cdot \hat{\vec{S}}_j = -J \sum_{\langle i,j \rangle} \left(\hat{S}_i^x \hat{S}_j^x + \hat{S}_i^y \hat{S}_j^y + \hat{S}_i^z \hat{S}_j^z\right)$$

This looks like the classical Ising model, but with a crucial difference: the $\hat{S}^x \hat{S}^x$ and $\hat{S}^y \hat{S}^y$ terms cause spin flips. The raising and lowering operators $\hat{S}^\pm_i = \hat{S}^x_i \pm i\hat{S}^y_i$ flip a spin from $|\downarrow\rangle$ to $|\uparrow\rangle$ and vice versa. Writing the interaction as:

$$\hat{\vec{S}}_i \cdot \hat{\vec{S}}_j = \hat{S}^z_i \hat{S}^z_j + \frac{1}{2}(\hat{S}^+_i \hat{S}^-_j + \hat{S}^-_i \hat{S}^+_j)$$

you can see that the last two terms move excitations around the lattice — a spin-up on site $i$ and spin-down on site $j$ can be exchanged by $\hat{S}^-_i \hat{S}^+_j$. The ground state is therefore a quantum superposition — a linear combination of many classical spin configurations — not a single ordered state.

**Symmetry:** The Heisenberg Hamiltonian commutes with the total spin operator $\hat{\vec{S}}_{\text{tot}} = \sum_i \hat{\vec{S}}_i$. It has full $SU(2)$ symmetry — rotational invariance in spin space. This is a larger symmetry group than the Ising model's $\mathbb{Z}_2$.

---

## 3. Quantum Fluctuations

In the classical ferromagnet ($J > 0$), the ground state is exactly $|\uparrow\uparrow\cdots\uparrow\rangle$ or $|\downarrow\downarrow\cdots\downarrow\rangle$ — these are exact eigenstates.

In the quantum antiferromagnet ($J < 0$), the "classical" **Neel state** — the perfectly alternating configuration $|\uparrow\downarrow\uparrow\downarrow\cdots\rangle$ in which every spin is anti-aligned with its neighbours — is *not* an eigenstate of the Heisenberg Hamiltonian. When the Hamiltonian acts on it, the $\hat{S}^+_i \hat{S}^-_j$ terms create configurations like $|\downarrow\uparrow\uparrow\downarrow\cdots\rangle$ — the state is immediately mixed with other configurations. The true ground state is a superposition over many classical configurations.

This means that even at $T = 0$, the system fluctuates — not due to thermal noise, but due to quantum mechanics. These are **quantum fluctuations**, and they are always present regardless of temperature.

Quantum fluctuations can be so strong that they destroy order that would exist classically. Two important examples:

- The **1D quantum Heisenberg antiferromagnet** has no long-range Neel order even at $T = 0$, thanks to quantum fluctuations (a quantum version of Mermin-Wagner). Its ground state is a disordered singlet called a quantum spin liquid.

- In **2D**, there is long-range order, but the sublattice magnetization is reduced from its classical value by about 40% due to quantum fluctuations. This reduction is measurable and is a genuine quantum effect.

---

## 4. The Many-Body Hilbert Space

For a chain of $N$ spin-$1/2$ sites, the Hilbert space is the tensor product of $N$ two-dimensional spaces:

$$\mathcal{H} = \underbrace{\mathbb{C}^2 \otimes \mathbb{C}^2 \otimes \cdots \otimes \mathbb{C}^2}_{N} \cong \mathbb{C}^{2^N}$$

The dimension is $2^N$ — exponential in system size. A general state is:

$$|\psi\rangle = \sum_{s_1, \ldots, s_N \in \{\uparrow,\downarrow\}} c_{s_1 \ldots s_N}\, |s_1 s_2 \cdots s_N\rangle$$

with $2^N$ complex coefficients. This exponential growth is the fundamental challenge of quantum many-body physics. For $N = 30$ the Hilbert space has $\sim 10^9$ dimensions; for $N = 50$ it has $\sim 10^{15}$. No classical computer can store the full wavefunction beyond $\sim 50$ spins.

---

## 5. Building the Hamiltonian Matrix

For small $N$, we can represent $\hat{H}$ as an explicit $2^N \times 2^N$ matrix and diagonalise it numerically. This is **exact diagonalisation (ED)**.

We represent each basis state as a binary integer: $|s_1 s_2 \cdots s_N\rangle \to$ an integer from 0 to $2^N - 1$, where the $i$-th bit encodes spin $i$ ($0 = \downarrow$, $1 = \uparrow$).

For each nearest-neighbour pair $(i, j)$, the interaction $\hat{\vec{S}}_i \cdot \hat{\vec{S}}_j$ contributes two types of matrix element:

**Diagonal terms** from $\hat{S}^z_i \hat{S}^z_j$: if spins $i$ and $j$ are aligned, add $+J/4$; if anti-aligned, add $-J/4$. These keep the basis state unchanged.

**Off-diagonal terms** from $\frac{1}{2}(\hat{S}^+_i \hat{S}^-_j + \hat{S}^-_i \hat{S}^+_j)$: if spins $i$ and $j$ are anti-aligned (one up, one down), this term flips both and contributes $J/2$ connecting that basis state to the one with spins $i$ and $j$ exchanged.

The matrix has at most $O(N \cdot 2^N)$ non-zero entries but $4^N$ matrix elements in total — so only a fraction $O(N/2^N)$ are non-zero for large $N$. We use sparse matrix formats (CSR or CSC) throughout to avoid storing the zeros.

---

## 6. Diagonalisation

Once we have the sparse Hamiltonian matrix, we find its eigenvalues and eigenvectors. There are two regimes:

**Full spectrum** (`numpy.linalg.eigh`): Converts to a dense matrix and finds all $2^N$ eigenvalues and eigenvectors. Practical up to $N \sim 16$–18 (matrix size $\sim 65536 \times 65536$). Memory cost is $O(4^N)$.

**Ground state only** (`scipy.sparse.linalg.eigsh` with Lanczos): Only requires matrix-vector products and can handle matrices of dimension $\sim 10^6$–$10^7$. Practical up to $N \sim 30$–40 when symmetries are used to block-diagonalise $\hat{H}$.

The **Lanczos algorithm** builds a Krylov subspace $\{|\psi\rangle, \hat{H}|\psi\rangle, \hat{H}^2|\psi\rangle, \ldots\}$ from a random starting vector and finds the extreme eigenvalues within this subspace. After $k$ iterations it gives an excellent approximation to the $k$ lowest eigenvalues. In practice, $k \sim 100$ iterations often suffices to converge the ground state energy to machine precision for matrices of dimension $2^{20}$.

---

## 7. Symmetries and Block Diagonalisation

The Heisenberg Hamiltonian conserves the total $z$-magnetization $\hat{S}^z_\text{tot} = \sum_i \hat{S}^z_i$. This means the Hamiltonian is block-diagonal in sectors of fixed $S^z_\text{tot}$, and we can diagonalise each sector independently.

The sector with $S^z_\text{tot} = 0$ (equal numbers of up and down spins) has dimension $\binom{N}{N/2}$ rather than $2^N$. For $N = 24$ this reduces the matrix dimension from $16\times10^6$ to $2.7\times10^6$ — a factor of $\sim 6$ — and ED becomes much more tractable.

Using all symmetries (translation, parity, spin inversion) can reduce the effective matrix dimension by another factor of $\sim 10$–20, pushing accessible system sizes to $N \sim 36$–40 for the Heisenberg chain.

---

## 8. What You Can Compute from ED

Once you have the ground state $|E_0\rangle$ and a few excited states:

**Ground state energy:** $E_0 = \langle E_0 | \hat{H} | E_0 \rangle$. For the 1D Heisenberg antiferromagnet, the exact answer is $E_0/N = \ln 2 - 1/4 \approx -0.4431$ per site (Bethe ansatz).

**Spin correlations:** $\langle \hat{S}^z_i \hat{S}^z_j \rangle = \langle E_0 | \hat{S}^z_i \hat{S}^z_j | E_0 \rangle$. This reveals the magnetic ordering pattern — alternating sign for the antiferromagnet.

**Energy gap:** $\Delta = E_1 - E_0$. A gapped system has $\Delta > 0$ in the thermodynamic limit; a gapless system has $\Delta \to 0$ as $N \to \infty$. The Heisenberg chain is gapless; the spin-1 Heisenberg chain is gapped (the Haldane gap).

**Entanglement entropy:** Computed from the Schmidt decomposition of $|E_0\rangle$ across a chosen cut. This is the subject of notebook 08.

**Structure factor:** $S(k) = \frac{1}{N}\sum_{i,j} e^{ik(i-j)} \langle \hat{S}^z_i \hat{S}^z_j \rangle$ — the Fourier transform of the spin correlation function, which reveals the ordering wavevector. For the antiferromagnet, it peaks at $k = \pi$.

---

## 9. Limitations of ED

The exponential scaling of the Hilbert space is the fundamental limitation. In practice, ED reaches:

- $N \sim 40$ sites for the 1D Heisenberg chain with symmetry reduction.
- $N \sim 30$ for models without conservation laws.
- Much smaller for 2D systems (the relevant quantity is the total Hilbert space dimension, not the linear size).

Beyond these sizes, we need more sophisticated methods: tensor networks (notebooks 09 and 10) or quantum Monte Carlo. ED remains essential as a benchmark — any new method must agree with ED for the system sizes where both are applicable. When you run DMRG in notebook 10 and it gives an energy within $10^{-8}$ of the ED result, that is your validation that the more expensive method is working correctly.

---

## 10. What Comes Next

In notebook 06 we introduce the 1D Transverse-Field Ising Model (TFIM), which is the simplest quantum model with a phase transition. We will build its Hamiltonian exactly as described here — Pauli matrices, tensor products, sparse matrix representation — and use ED to compute the energy spectrum across the quantum phase transition.

The Heisenberg model serves as a warm-up: it has all the complexity of quantum many-body physics (non-commuting operators, entanglement, quantum fluctuations) without a phase transition to worry about. Once you are comfortable building and diagonalising quantum Hamiltonians, the TFIM is a natural next step.
