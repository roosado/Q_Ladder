# Lecture Notes — Step 4: The Ising Model with External Field — First-Order vs Continuous Transitions

---

## Overview

So far we have studied the Ising model at $h = 0$, where the $\mathbb{Z}_2$ symmetry ($s_i \to -s_i$) is intact. Adding an external magnetic field $h$ breaks this symmetry explicitly. The result is a much richer phase diagram, and the opportunity to see a qualitatively different kind of transition: the **first-order phase transition**.

Understanding the distinction between first-order and continuous (second-order) transitions is essential before we move to quantum models. It turns out to be central to quantum phase transitions as well, and the tools developed here — the Landau theory picture, hysteresis, and phase coexistence — recur throughout the series.

---

## 1. The Hamiltonian with External Field

We add a coupling to an external field $h$:

$$H = -J \sum_{\langle i,j \rangle} s_i s_j - h \sum_i s_i$$

The field term $-h s_i$ favours spin-up sites when $h > 0$ and spin-down sites when $h < 0$. It breaks the $\mathbb{Z}_2$ symmetry: the configurations with all spins up and all spins down are no longer equivalent.

The magnetization $m = N^{-1}\sum_i s_i$ now measures the response of the system to the applied field. At any temperature and field, there is a well-defined equilibrium value $m(T, h)$.

---

## 2. The Phase Diagram

The phase diagram in the $(T, h)$ plane has three regions:

**Paramagnetic phase** ($T > T_c$ for any $h$, or $h \neq 0$ for any $T$): The magnetization $m$ varies smoothly and continuously as a function of $T$ and $h$. There is no phase transition.

**Ferromagnetic phase** ($T < T_c$, $h = 0$): Two degenerate states coexist — one with $m > 0$ (spin-up dominated) and one with $m < 0$.

**First-order transition line** ($T < T_c$, $h = 0$): As you vary $h$ through zero at fixed $T < T_c$, the equilibrium magnetization jumps discontinuously from a positive value to a negative value. This is a **first-order phase transition**.

The first-order line terminates at the point $(T_c, h = 0)$ — the critical point we studied in notebooks 01 and 02. At this endpoint, the jump in $m$ vanishes continuously and the transition becomes second-order (continuous).

This is the general structure: a line of first-order transitions ending at a critical point. It appears in the liquid-gas phase diagram (the coexistence curve ending at the liquid-gas critical point), in binary alloy ordering, and in many quantum phase diagrams.

---

## 3. First-Order vs Continuous Transitions

The distinction between first-order and continuous transitions is one of the most important in all of condensed matter:

**Continuous (second-order) transition:**
- The order parameter $m$ goes to zero continuously as $T \to T_c$.
- The correlation length diverges: $\xi \to \infty$.
- The transition is characterised by power laws and universal critical exponents.
- The two phases become indistinguishable at the critical point.
- There is no latent heat.

**First-order transition:**
- The order parameter $m$ jumps discontinuously at the transition.
- The correlation length remains finite at the transition point.
- There is no universality — the size of the jump depends on microscopic details.
- The two phases coexist at the transition.
- There is a latent heat — energy is absorbed or released discontinuously.

In the Ising model, the distinction is clear: crossing $h = 0$ at $T < T_c$ is first-order (jump in $m$), while crossing $T_c$ at $h = 0$ is second-order (continuous vanishing of $m$).

---

## 4. Phase Coexistence

At the first-order transition ($T < T_c$, $h = 0$), the system has two coexisting equilibrium phases: one with $m = m_+(T) > 0$ and one with $m = m_-(T) = -m_+(T) < 0$. Both have the same free energy at $h = 0$.

In a real system, regions of spin-up phase and spin-down phase coexist, separated by domain walls. The equilibrium domain structure minimises the total free energy, including the surface tension (energy cost) of the walls.

The mean-field equation of state $m = \tanh(\beta(Jzm + h))$ gives three solutions for $m$ when $T < T_c$ and $|h|$ is small. Two are stable and one is unstable (the metastable branch). The physical equilibrium switches between the two stable branches at $h = 0$ — this is the Maxwell construction.

---

## 5. Hysteresis

A key feature of first-order transitions is **hysteresis**. When you slowly increase $h$ from negative to positive values at $T < T_c$, the system does not immediately jump from $m < 0$ to $m > 0$ when $h$ crosses zero. Instead, it remains in the metastable spin-down phase until the field is large enough to nucleate a droplet of the spin-up phase. This nucleation event is stochastic and depends on temperature and the rate at which the field is varied.

The result is a hysteresis loop: the magnetization traces a different path when $h$ increases versus when it decreases. The area of the loop is the energy dissipated per cycle.

In the simulation you can observe hysteresis directly by sweeping $h$ forward and backward and plotting $m$ vs $h$. The width of the loop shrinks as you approach $T_c$ and vanishes exactly at $T_c$, consistent with the second-order nature of the transition there.

---

## 6. Landau Theory

The Landau theory of phase transitions provides a unified framework for understanding both first-order and continuous transitions in terms of a free energy functional. Close to the transition, we expand the free energy in powers of the order parameter $m$:

$$F(m, T, h) = a(T) m^2 + b(T) m^4 + c(T) m^6 + \ldots - hm$$

**For the continuous transition** at $h = 0$: $b > 0$ and $a(T) = a_0(T - T_c)$ changes sign at $T_c$. For $T > T_c$ the minimum is at $m = 0$; for $T < T_c$ two minima appear at $m = \pm\sqrt{-a/2b} \propto (T_c - T)^{1/2}$ — this is the mean-field value $\beta = 1/2$ (the exact value $\beta = 1/8$ requires going beyond mean field).

**For the first-order transition** (when $b < 0$ and $c > 0$): the free energy has two minima separated by a barrier. The transition is first-order when the two minima have equal free energy.

The Landau theory tells you what kind of transition to expect from the symmetry of the order parameter alone, without solving the model.

---

## 7. Connecting to Quantum Phase Transitions

The distinction between first-order and continuous transitions is not just a classical phenomenon. Many quantum phase transitions are first-order: the ground state changes discontinuously as some parameter is varied. When this happens, there is no quantum critical point, no diverging correlation length, and no universal scaling.

Some quantum phase transitions start continuous at $T = 0$ but become first-order at finite temperature — a phenomenon called a **fluctuation-induced first-order transition**. And some apparently continuous quantum transitions are actually weakly first-order when examined closely enough.

Knowing what to look for — jumps in the order parameter, hysteresis, latent heat, the absence of diverging correlations — is essential for identifying a first-order transition in both classical and quantum simulations.
