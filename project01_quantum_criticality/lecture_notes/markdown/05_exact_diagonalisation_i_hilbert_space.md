# Lecture Notes — Lecture 5: Exact Diagonalisation I: Hilbert Space

---

## Overview

Exact diagonalisation (ED) is the most direct approach to quantum many-body physics: represent the Hamiltonian as an explicit matrix and compute its eigenvalues and eigenvectors numerically. There is no approximation beyond floating-point arithmetic.

In this lecture we build the foundation: how to represent the many-body Hilbert space, how to encode basis states as integers, and how to construct the TFIM Hamiltonian as a sparse matrix that can be diagonalised. The focus is on the *implementation* — what data structures, what algorithms, what limits. Lecture 06 then covers what you can extract from the resulting eigenstates.

---

## 1. The Many-Body Hilbert Space

For a chain of $N$ spin-$1/2$ sites, the Hilbert space is the tensor product of $N$ two-dimensional spaces:

$$\mathcal{H} = \underbrace{\mathbb{C}^2 \otimes \mathbb{C}^2 \otimes \cdots \otimes \mathbb{C}^2}_{N} \cong \mathbb{C}^{2^N}$$

The dimension is $2^N$ — exponential in system size. A general state is:

$$|\psi\rangle = \sum_{s_1, \ldots, s_N \in \{\uparrow,\downarrow\}} c_{s_1 \ldots s_N}\, |s_1 s_2 \cdots s_N\rangle$$

with $2^N$ complex coefficients. This exponential growth is the fundamental challenge of quantum many-body physics. For $N = 20$ the Hilbert space has $\sim 10^6$ dimensions; for $N = 40$ it has $\sim 10^{12}$.

The **computational basis** is the set of all tensor product states $|s_1 s_2 \cdots s_N\rangle$ where each $s_i \in \{\uparrow, \downarrow\}$. Every quantum state can be expressed as a superposition of these $2^N$ basis states, so the computational basis is a complete orthonormal basis for $\mathcal{H}$.

---

## 2. Representing Basis States

The key implementation insight: each computational basis state $|s_1 s_2 \cdots s_N\rangle$ can be encoded as an integer from $0$ to $2^N - 1$ by treating the spin values as bits.

**Encoding:** Map $|\uparrow\rangle \to 1$ and $|\downarrow\rangle \to 0$. Then the basis state $|s_1 s_2 \cdots s_N\rangle$ corresponds to the binary integer with $s_i$ in bit position $i$. For example, on 4 sites:

$$|\uparrow\downarrow\uparrow\uparrow\rangle \to 1011_2 = 11$$

This encoding maps the $2^N$ basis states bijectively to the integers $\{0, 1, \ldots, 2^N - 1\}$, which serve as the row/column indices of the Hamiltonian matrix.

**Bit operations:** Given a basis state encoded as integer $n$:
- Spin at site $i$: `(n >> i) & 1` (extract bit $i$)
- Flip spin at site $i$: `n ^ (1 << i)` (toggle bit $i$)
- Check if spins at $i$ and $j$ are aligned: compare `(n >> i) & 1` with `(n >> j) & 1`

These operations are $O(1)$ and allow us to compute matrix elements of the Hamiltonian without explicitly constructing the spin operators as matrices.

---

## 3. Building the Hamiltonian Matrix

The TFIM Hamiltonian has two types of terms, each contributing matrix elements between basis states.

**Ising coupling** $-J \hat{\sigma}^z_i \hat{\sigma}^z_{i+1}$:

$\hat{\sigma}^z$ is diagonal in the computational basis: $\hat{\sigma}^z |s\rangle = (-1)^{s+1} |s\rangle$ where $s \in \{0,1\}$. The product $\hat{\sigma}^z_i \hat{\sigma}^z_{i+1}$ gives $+1$ if spins $i$ and $i+1$ are aligned and $-1$ if they are anti-aligned. This contributes a **diagonal** matrix element: for basis state $n$, add $-J \cdot (+1)$ if bits $i$ and $i+1$ are equal, or $-J \cdot (-1)$ if they differ.

**Transverse field** $-\Gamma \hat{\sigma}^x_i$:

$\hat{\sigma}^x$ flips a spin: $\hat{\sigma}^x |\uparrow\rangle = |\downarrow\rangle$ and $\hat{\sigma}^x |\downarrow\rangle = |\uparrow\rangle$. Acting on basis state $n$, the term $-\Gamma \hat{\sigma}^x_i$ gives a matrix element of $-\Gamma$ connecting $n$ to the state $n' = n \oplus (1 \ll i)$ (with bit $i$ flipped). This contributes an **off-diagonal** matrix element.

The result: the Hamiltonian matrix has at most $N$ off-diagonal entries per row (one from each transverse field term), plus one diagonal entry per row. The total number of non-zero entries is $O(N \cdot 2^N)$ out of $4^N$ possible entries — the matrix is very sparse.

**Sparse matrix storage:** We use coordinate (COO) or compressed sparse row (CSR) format. Build a list of `(row, col, value)` triples, then convert to a sparse matrix. The memory cost is $O(N \cdot 2^N)$ rather than $O(4^N)$.

---

## 4. Diagonalisation

Once we have the sparse Hamiltonian matrix, we find its eigenvalues and eigenvectors.

**Full spectrum** (`numpy.linalg.eigh`): Converts to a dense matrix and finds all $2^N$ eigenvalues and eigenvectors. Practical up to $N \sim 16$–18. Memory cost is $O(4^N)$ — at $N = 18$ this is already 64 GB for double precision.

**Ground state only** (`scipy.sparse.linalg.eigsh` with Lanczos): Only requires matrix-vector products — never forms the full dense matrix. The Lanczos algorithm builds a Krylov subspace $\{|\psi\rangle, \hat{H}|\psi\rangle, \hat{H}^2|\psi\rangle, \ldots\}$ from a random starting vector and finds the extreme eigenvalues within this subspace. After $k$ iterations it gives an excellent approximation to the $k$ lowest eigenvalues. Memory cost $O(2^N)$; practical up to $N \sim 30$–40.

In practice, for this series we use Lanczos to find the lowest 2–4 eigenvalues and their eigenvectors. This gives us:
- The ground state energy $E_0$ and eigenvector $|E_0\rangle$
- The first excited state $E_1$ and eigenvector $|E_1\rangle$ (needed for the spectral gap)

---

## 5. Symmetries and Block Diagonalisation

The TFIM has a $\mathbb{Z}_2$ symmetry: the Hamiltonian commutes with the **parity operator**

$$\hat{P} = \prod_{i=1}^N \hat{\sigma}^x_i$$

which flips all spins simultaneously. Since $[\hat{H}, \hat{P}] = 0$, we can simultaneously diagonalise $\hat{H}$ and $\hat{P}$. Every eigenstate has a definite parity $P = +1$ or $P = -1$, and the Hamiltonian is block-diagonal in the two parity sectors.

Each parity sector has dimension $2^{N-1}$, reducing the matrix dimension by a factor of 2. More importantly:
- In the ordered phase, the two nearly-degenerate ground states are the even and odd superpositions of $|\uparrow\uparrow\cdots\uparrow\rangle$ and $|\downarrow\downarrow\cdots\downarrow\rangle$.
- In the disordered phase, the true ground state has even parity.

Working in fixed parity sectors avoids numerical issues from near-degeneracy and is more computationally efficient.

For other models (e.g. Heisenberg chain), additional symmetries apply: conservation of total $S^z$, translation invariance, and spatial reflection. Using all symmetries can reduce the effective matrix dimension by a factor of $\sim 10$–20, pushing accessible system sizes from $N \sim 20$ (without symmetries) to $N \sim 36$–40.

---

## 6. Limitations of Exact Diagonalisation

The exponential scaling of the Hilbert space is the fundamental limitation. In practice, ED reaches:

- $N \sim 40$ sites for 1D spin-$1/2$ models with full symmetry exploitation.
- $N \sim 20$–24 without symmetry reduction.
- Much smaller for 2D systems, or models with larger local Hilbert spaces.

Memory is usually the binding constraint: storing one eigenvector of a $2^{30}$-dimensional Hilbert space as double-precision complex numbers requires $16 \times 2^{30} \approx 16$ GB.

ED is indispensable as a **benchmark method**: any numerical approach that claims to solve a quantum many-body problem must agree with ED for the system sizes where both are applicable. When you run DMRG in lecture 09 and it gives an energy within $10^{-8}$ of the ED result, that is your validation that the tensor network method is working correctly.

The path beyond ED is through tensor networks, which exploit the structure of low-entanglement states to represent wavefunctions efficiently. The bridge between ED and tensor networks is the subject of lectures 07–08.
