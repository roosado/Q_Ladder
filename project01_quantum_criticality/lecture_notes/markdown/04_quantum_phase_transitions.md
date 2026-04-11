# Lecture Notes — Lecture 4: Quantum Phase Transitions

---

## Overview

In lecture 02 we applied finite-size scaling to the classical Ising transition. Now we apply the exact same tools to the quantum phase transition of the 1D TFIM. The control parameter has changed — it is $\Gamma/J$ instead of temperature — but the FSS framework is identical. The quantum-to-classical mapping guarantees this: the 1D TFIM and the 2D classical Ising model share their universality class and their critical exponents.

This lecture also introduces a new observable with no classical counterpart: the **spectral gap**. Its closing at the quantum critical point is one of the clearest signatures of a quantum phase transition.

---

## 1. The Quantum Phase Transition in the TFIM

The quantum phase transition occurs at $\Gamma_c = J$. The two limits established in lecture 03 make clear what changes at this point:

- Below $\Gamma_c$: the Ising coupling dominates. The ground state is approximately $|\uparrow\uparrow\cdots\uparrow\rangle + |\downarrow\downarrow\cdots\downarrow\rangle$ (a Schrödinger cat state), and the symmetry-broken order parameter $\langle\hat{\sigma}^z\rangle \neq 0$.
- Above $\Gamma_c$: the transverse field dominates. Quantum fluctuations destroy the order, the ground state is unique, and $\langle\hat{\sigma}^z\rangle = 0$.
- At $\Gamma_c = J$ exactly: the system is critical. The correlation length diverges, and the system is described by the Ising CFT with central charge $c = 1/2$.

The role of temperature $T$ in the classical problem is played by $\Gamma/J$ in the quantum problem. The role of magnetization $\langle \sigma^z \rangle$ (the classical order parameter) is played by $\langle \hat{\sigma}^z \rangle$ (the quantum expectation value in the ground state). Everything else translates directly.

---

## 2. The Order Parameter

In the classical Ising model, $\langle |m| \rangle$ was computed by sampling configurations. In the quantum TFIM, we compute the ground state $|E_0\rangle$ by exact diagonalisation and take the expectation value:

$$m = \frac{1}{N}\left|\sum_i \langle E_0 | \hat{\sigma}^z_i | E_0 \rangle\right|$$

On a finite system with periodic boundary conditions, the $\mathbb{Z}_2$ symmetry gives $\langle \hat{\sigma}^z \rangle = 0$ exactly in the ground state, even in the ordered phase — just as the classical system had $\langle m \rangle = 0$ on finite lattices. To detect the order we use the Binder cumulant, which handles this automatically.

---

## 3. The Energy Gap

A quantity with no direct classical analogue is the **spectral gap**:

$$\Delta(L, \Gamma) = E_1(L, \Gamma) - E_0(L, \Gamma)$$

In the ordered phase ($\Gamma < \Gamma_c$), the gap is exponentially small in $L$. This reflects quantum tunnelling between the two ordered states: on a finite system, the Hamiltonian couples $|\uparrow\uparrow\cdots\uparrow\rangle$ and $|\downarrow\downarrow\cdots\downarrow\rangle$ through a sequence of single-spin flips. The amplitude for this process is $(\Gamma/J)^N \sim e^{-N/\xi}$, giving a gap that vanishes exponentially in the thermodynamic limit — the hallmark of spontaneous symmetry breaking.

In the disordered phase ($\Gamma > \Gamma_c$), the ground state is unique and the gap is of order $|\Gamma - \Gamma_c|$ — finite.

At the critical point ($\Gamma = \Gamma_c$), the gap closes as a power law:

$$\Delta(L, \Gamma_c) \sim L^{-z}$$

where $z = 1$ for the 1D TFIM. This power-law closing is one of the most important signatures of a quantum phase transition.

The gap also encodes the **relaxation time**: $\tau \sim \Delta^{-1} \sim \xi^z$. Near the QCP, both the spatial correlation length $\xi$ and the temporal correlation length $\xi_\tau = \xi^z$ diverge. This makes explicit the quantum-to-classical mapping: in $d$ spatial dimensions, the effective classical problem has $d + z$ dimensions.

---

## 4. The Binder Cumulant for Quantum Systems

The Binder cumulant is defined exactly as before:

$$U_4(L, \Gamma) = 1 - \frac{\langle m^4 \rangle}{3\langle m^2 \rangle^2}$$

but now $\langle m^k \rangle = \langle E_0 | (N^{-1}\sum_i \hat{\sigma}^z_i)^k | E_0 \rangle$ is a ground state expectation value.

The crossing property holds just as it does for classical systems: curves for different system sizes $L$ all cross at $\Gamma_c$, giving us the critical point without any fitting.

---

## 5. Finite-Size Scaling for the Gap

The gap obeys its own FSS form:

$$\Delta(L, \Gamma) = L^{-z} \cdot g\!\left((\Gamma - \Gamma_c) L^{1/\nu}\right)$$

At $\Gamma_c$, $\Delta \sim L^{-z}$. A log-log plot of $\Delta$ vs $L$ at the critical point gives slope $-z = -1$.

The dimensionless ratio $L \cdot \Delta(L, \Gamma)$ is size-independent at $\Gamma_c$, so curves for different system sizes cross at the critical point — another Binder-cumulant-like observable with no classical analogue.

---

## 6. Scaling Collapse for the Quantum Transition

Define:

$$\tilde{m} = L^{\beta/\nu} \langle |\hat{m}^z| \rangle, \quad x = (\Gamma - \Gamma_c) \cdot L^{1/\nu}$$

With $\beta = 1/8$ and $\nu = 1$ (from the quantum-to-classical mapping), all system sizes should collapse onto a single curve. The fact that this collapse works — and uses the same exponents as the 2D classical Ising model — is a direct verification of the mapping.

---

## 7. Comparison: Classical vs Quantum Critical Behaviour

| Observable | Classical Ising (lecture 02) | Quantum TFIM (lecture 04) |
|---|---|---|
| Control parameter | Temperature $T$ | Transverse field $\Gamma/J$ |
| Order parameter | $\langle\|m\|\rangle$ (thermal) | $\langle\|\hat m^z\|\rangle$ (ground state) |
| Gap closes? | No — thermal gap always finite | Yes — spectral gap $\Delta \sim L^{-z}$ |
| Binder cumulant | Crosses at $T_c$ | Crosses at $\Gamma_c$ |
| Critical exponents | $\beta=1/8$, $\nu=1$, $\gamma=7/4$ | Same (universality class) |

The diverging correlation length $\xi \sim |\Gamma - \Gamma_c|^{-\nu}$ measured via FSS in lecture 02 and the vanishing spectral gap $\Delta \sim L^{-z}$ that appears only in the quantum problem are two faces of the same phenomenon. Both diverge because the system cannot pick a preferred state — it is poised exactly between two competing phases.
