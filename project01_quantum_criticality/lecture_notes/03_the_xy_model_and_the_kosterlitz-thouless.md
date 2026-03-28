# Lecture Notes — Step 3: The XY Model and the Kosterlitz-Thouless Transition

---

## Overview

In notebooks 01 and 02 we studied a model with a discrete symmetry: flipping all spins gives an equivalent configuration, so the symmetry group is $\mathbb{Z}_2$. The ferromagnetic phase breaks that symmetry spontaneously.

Now we promote the spins to continuous variables. In the XY model, each spin is a unit vector in the plane, characterised by a single angle $\theta_i \in [0, 2\pi)$. The symmetry group is $U(1)$ — continuous rotations in the plane. This single change in the model leads to fundamentally different physics: no conventional ordered phase in 2D, but a completely new kind of phase transition driven by topological defects called vortices.

---

## 1. The XY Model

The Hamiltonian is:

$$H = -J \sum_{\langle i,j \rangle} \cos(\theta_i - \theta_j)$$

Like the Ising model, we have a ferromagnetic coupling $J > 0$ between nearest neighbours. But instead of $s_i \in \{+1, -1\}$, each spin is now an angle $\theta_i \in [0, 2\pi)$, representing a direction in the plane: $\vec{s}_i = (\cos\theta_i, \sin\theta_i)$.

The energy is minimised when all angles are equal — a ferromagnetically aligned state. The energy cost of a misalignment $\Delta\theta = \theta_i - \theta_j$ is $J(1 - \cos\Delta\theta)$, which for small misalignments is $\approx J(\Delta\theta)^2/2$. For small fluctuations, the XY model behaves like an elastic medium for the angle field $\theta(\vec{r})$.

The symmetry is now $U(1)$: rotating all spins by the same angle $\theta_i \to \theta_i + \phi$ leaves the Hamiltonian unchanged. This is a continuous symmetry — there is a continuously connected family of equivalent ground states.

---

## 2. The Mermin-Wagner Theorem

Here is where the XY model surprises you. In 2D, the Ising model has a ferromagnetic phase below $T_c$. You might expect the XY model to also have an ordered phase — perhaps even a richer one, given the continuous symmetry. The opposite is true.

The **Mermin-Wagner theorem** (1966) states:

> A system with a continuous symmetry in $d \leq 2$ dimensions cannot spontaneously break that symmetry at any finite temperature.

For the 2D XY model, this means $\langle \vec{s}_i \rangle = 0$ for all $T > 0$. There is no ferromagnetic order.

The intuitive argument: in the ordered state, the angle field $\theta(\vec{r})$ is nearly uniform. Long-wavelength spin waves (Goldstone modes of the broken symmetry) cost very little energy — their energy goes as $q^2$ where $q$ is the wavevector. In 2D, the density of states at low energy is constant, so there are many low-energy modes. The thermal fluctuations of these long-wavelength modes are logarithmically divergent in system size:

$$\langle (\Delta\theta)^2 \rangle \sim \int_0^\Lambda \frac{d^2q}{q^2} \sim \log(L)$$

No matter how low the temperature, these fluctuations grow without bound as $L \to \infty$, destroying any macroscopic order.

---

## 3. So Is There a Phase Transition?

You might conclude that the 2D XY model has no phase transition — just a single disordered phase for all $T > 0$. This was the belief until Kosterlitz and Thouless (1973) discovered something much more subtle.

The key insight: even though long-range order is destroyed, there is a distinction between two kinds of disorder, characterised by the **decay of correlation functions**.

The spin-spin correlation function is $G(r) = \langle \vec{s}_0 \cdot \vec{s}_r \rangle = \langle \cos(\theta_0 - \theta_r) \rangle$.

- **High temperature (disordered):** $G(r) \sim e^{-r/\xi}$ — exponential decay. No order.
- **Low temperature:** $G(r) \sim r^{-\eta(T)}$ — algebraic (power-law) decay. Also no long-range order, but much slower decay. This is called **quasi-long-range order**.

The exponent $\eta(T)$ varies continuously with temperature in the low-temperature phase. At $T = 0$, $\eta = 0$ and you get true long-range order. As $T$ increases from zero, $\eta$ increases. At $T_{BKT}$, $\eta = 1/4$ exactly.

The transition at $T_{BKT}$ is between quasi-long-range order (algebraic correlations) and disorder (exponential correlations). It is a genuine phase transition, but of a completely different character from the Ising transition. It is an **infinite-order transition** — all derivatives of the free energy are continuous at $T_{BKT}$.

---

## 4. Vortices

To understand what drives the transition, we need topological defects called **vortices**.

A vortex is a configuration of the angle field $\theta(\vec{r})$ that winds by $\pm 2\pi$ as you go around a closed loop enclosing the vortex core. The winding number is a topological invariant — you cannot remove a vortex by smooth deformations of the field.

A single vortex costs a logarithmically divergent energy:

$$E_{\text{vortex}} \sim \pi J \log(L/a)$$

where $L$ is the system size and $a$ is the lattice spacing. A single vortex always costs infinite energy in the thermodynamic limit.

However, a **vortex-antivortex pair** (winding numbers $+1$ and $-1$) costs only a finite energy proportional to $\log(d/a)$, where $d$ is the separation between the pair. At large separation the energy grows, so the pair is bound together by a logarithmic potential — like a 2D Coulomb interaction.

The free energy of a vortex includes both an energy cost and an entropy gain. A single vortex can be placed at roughly $(L/a)^2$ positions, giving an entropy $S \sim 2\log(L/a)$. The free energy is:

$$F_{\text{vortex}} \sim (\pi J - 2T)\log(L/a)$$

This is the crucial formula. For $T < T_{BKT} = \pi J / 2$, the energy term dominates and $F_{\text{vortex}} > 0$: free vortices are suppressed, and only bound pairs exist. For $T > T_{BKT}$, the entropy term dominates and $F_{\text{vortex}} < 0$: free vortices proliferate throughout the system.

---

## 5. The BKT Transition

The **Berezinskii-Kosterlitz-Thouless (BKT) transition** is the unbinding of vortex-antivortex pairs:

- **Below $T_{BKT}$:** Vortices exist only in bound pairs. Spin-spin correlations decay algebraically — quasi-long-range order.
- **Above $T_{BKT}$:** Pairs unbind and free vortices proliferate. Correlations cross over to exponential decay — true disorder.

This is a topological phase transition. There is no local order parameter that distinguishes the two phases. The distinction is entirely in the nature of the topological defects.

The correlation length diverges as $T \to T_{BKT}^+$ not as a power law but exponentially:

$$\xi \sim \exp\!\left(\frac{c}{\sqrt{T - T_{BKT}}}\right)$$

This means the transition is approached much more slowly than a conventional critical point. Finite-size effects are severe and the transition is very hard to locate numerically.

---

## 6. Detecting the BKT Transition in Simulation

**Helicity modulus (spin stiffness):** The natural probe is the helicity modulus $\Upsilon$, which measures the energy cost of twisting the angle field across the system. At $T_{BKT}$, the helicity modulus drops discontinuously to zero, satisfying the universal relation:

$$\Upsilon(T_{BKT}^-) = \frac{2}{\pi} T_{BKT}$$

Measuring this jump is the standard numerical method for locating $T_{BKT}$.

**Vortex density:** You can directly count vortices — plaquettes where the angle winds by $\pm 2\pi$. In the low-temperature phase the vortex density is exponentially small; it rises sharply at $T_{BKT}$.

**Correlation functions:** Fitting the algebraic decay exponent $\eta(T)$ and checking that it reaches $1/4$ at the transition provides another check, though this requires large systems.

---

## 7. Why This Matters

The BKT transition appears in several physical systems:

- **Thin film superconductors:** The BKT transition corresponds to the loss of superconductivity through vortex unbinding, confirmed experimentally by Bishop and Reppy (1978).
- **Liquid helium films:** Superfluidity in a 2D helium film is destroyed by the BKT mechanism.
- **Cold atoms:** 2D Bose gases in optical traps undergo a BKT transition that has been observed directly.

The BKT transition also introduced topological order into condensed matter physics — a concept that later grew into topological insulators, topological superconductors, and the Kitaev chain we study in notebook 11.

---

## 8. What Comes Next

The XY model completes our tour of classical spin models. We have seen three qualitatively different situations: the Ising model ($\mathbb{Z}_2$ symmetry, conventional critical point), the XY model ($U(1)$ symmetry, topological transition), and next, the Ising model in an external field, where we see a first-order transition.

After that, we are ready to make the jump to quantum mechanics.
