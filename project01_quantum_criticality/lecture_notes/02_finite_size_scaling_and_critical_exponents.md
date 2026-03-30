# Lecture Notes — Step 2: Finite-Size Scaling and Critical Exponents

---

## Overview

In notebook 01 we observed a phase transition but our estimates of $T_c$ were off, and the transition looked rounded rather than sharp. This notebook explains both of these facts and turns them into a systematic method for extracting exact critical exponents. The central tool is **finite-size scaling (FSS)** — a framework that relates the behaviour of a finite system to the universal properties of the infinite one.

By the end you should understand what critical exponents are, why the Binder cumulant is such a useful observable, and how to perform a scaling collapse that reveals the universal scaling function underlying a phase transition.

---

## 1. The Correlation Length

To understand finite-size effects, we first need the concept of the **correlation length** $\xi$.

At any temperature, nearby spins tend to point in the same direction because of the ferromagnetic coupling. The correlation function $G(r) = \langle s_0\, s_r \rangle - \langle s_0 \rangle \langle s_r \rangle$ measures how correlated two spins separated by distance $r$ are. Away from the critical point, correlations decay exponentially:

$$G(r) \sim e^{-r/\xi}$$

and $\xi$ is the correlation length — the scale beyond which spins no longer know about each other.

As $T \to T_c$ from either side, the correlation length diverges:

$$\xi \sim |T - T_c|^{-\nu}$$

The exponent $\nu$ is one of the fundamental critical exponents. For the 2D Ising model, $\nu = 1$ exactly (from Onsager). This divergence is the origin of everything interesting at a phase transition: when $\xi \to \infty$, the system becomes scale-free and the critical point has a special kind of symmetry (scale invariance) that is not present at any other temperature.

---

## 2. Why Finite Size Rounds the Transition

On a lattice of side $L$, the correlation length cannot exceed $L$. The system cannot develop correlations longer than its own size. This creates a crossover: when $\xi \ll L$ the system behaves like an infinite one, but when $\xi \gtrsim L$ the boundaries are felt everywhere and the system's behaviour changes qualitatively.

Near $T_c$, where $\xi$ would like to diverge, the finite lattice imposes a cutoff. The divergence is replaced by a smooth peak. The temperature at which this cutoff kicks in is roughly where $\xi \sim L$, i.e., where $|T - T_c|^{-\nu} \sim L$, which gives:

$$|T - T_c^{(L)} - T_c| \sim L^{-1/\nu}$$

This is the **shift** of the apparent critical temperature on a lattice of size $L$. As $L \to \infty$, the apparent $T_c$ approaches the true one, but slowly — as $L^{-1/\nu}$.

Similarly, the transition is rounded over a temperature window of width $\sim L^{-1/\nu}$. For the 2D Ising model with $\nu = 1$, this window shrinks as $1/L$. Doubling the system size halves the rounding.

The key takeaway: **finite-size effects are not numerical errors — they are controlled, predictable, and quantifiable.** FSS theory tells us exactly how observables depend on both $T$ and $L$ near the critical point.

---

## 3. Critical Exponents

Why do we care about precisely characterising the singularities at a critical point? Because the exponents are **universal**: two systems with completely different microscopic details — different lattice geometries, different interaction ranges, different physical origins — can share identical critical exponents. This is the concept of universality, and it is what makes critical phenomena a fundamental subject rather than a collection of special cases. Measuring the exponents tells you which universality class a system belongs to, not just the details of one particular model.

The critical point is characterised by a set of power-law singularities. Each one defines a critical exponent:

**Order parameter ($\beta$):**
$$\langle |m| \rangle \sim (T_c - T)^\beta \quad (T < T_c)$$

For the 2D Ising model, $\beta = 1/8$. The magnetization vanishes extremely slowly as $T \to T_c^-$.

**Correlation length ($\nu$):**
$$\xi \sim |T - T_c|^{-\nu}$$

For the 2D Ising model, $\nu = 1$. This means at $T = 2.0$ (which is $0.27$ below $T_c$) the correlation length is already of order $1/0.27 \approx 4$ lattice spacings. A 25×25 lattice starts feeling finite-size effects well before reaching $T_c$.

**Susceptibility ($\gamma$):**
$$\chi \sim |T - T_c|^{-\gamma}$$

For the 2D Ising model, $\gamma = 7/4$. The susceptibility diverges much more strongly than the correlation length — it grows as the square of $\xi$ (this is related by the scaling relation $\gamma = (2 - \eta)\nu$).

These exponents are not independent. They are connected by the **scaling relations** (derived from the scaling hypothesis):

$$\alpha + 2\beta + \gamma = 2, \quad \gamma = \nu(2 - \eta), \quad \ldots$$

So there are really only two independent exponents. In 2D, once you know $\nu$ and $\eta$, everything else follows.

---

## 4. The Finite-Size Scaling Hypothesis

FSS theory states that near the critical point, any singular observable $O$ takes the form:

$$O(T, L) = L^{x_O} \cdot f_O\!\left(\frac{T - T_c}{T_c} \cdot L^{1/\nu}\right)$$

where $x_O$ is the scaling dimension of $O$ and $f_O$ is a **universal** scaling function — the same for all $L$, and, remarkably, the same for all models in the same universality class.

For the magnetization: $x_m = -\beta/\nu$, so $\langle |m| \rangle = L^{-\beta/\nu} f_m((T - T_c) L^{1/\nu})$.

For the susceptibility: $x_\chi = \gamma/\nu$, so $\chi = L^{\gamma/\nu} f_\chi((T - T_c) L^{1/\nu})$.

This is a powerful prediction. If you plot $L^{\beta/\nu} \langle |m| \rangle$ against $(T - T_c) L^{1/\nu}$ for several values of $L$, all the curves should **collapse onto a single line** — the universal function $f_m$. The collapse only works if you use the correct exponents and the correct $T_c$. This gives us a precise, visual method for extracting critical exponents.

---

## 5. The Binder Cumulant

Locating $T_c$ precisely is the first challenge. Reading it off the magnetization curve is difficult because the curves for different $L$ do not all cross at the same point — the shift $\sim L^{-1/\nu}$ means larger systems have their apparent $T_c$ closer to the true value.

The **Binder cumulant** solves this problem. It is defined as:

$$U_4(L, T) = 1 - \frac{\langle m^4 \rangle}{3\,\langle m^2 \rangle^2}$$

Its scaling form is $U_4(L, T) = g\!\left((T - T_c) L^{1/\nu}\right)$. Notice that there is no prefactor of $L$ — the Binder cumulant is **dimensionless**. This means its curves for different $L$ are all mapped by the same function $g$, and they therefore all cross at the same point: $T = T_c$.

To understand the limiting values:

- **Ordered phase** ($T \ll T_c$): The magnetization is nearly $\pm 1$ and has very small fluctuations. $\langle m^4 \rangle \approx \langle m^2 \rangle^2 \approx 1$, so $U_4 \to 1 - 1/3 = 2/3$.

- **Disordered phase** ($T \gg T_c$): The magnetization is Gaussian distributed with zero mean. For a Gaussian variable, $\langle m^4 \rangle = 3\langle m^2 \rangle^2$, so $U_4 \to 1 - 3/3 = 0$.

So the Binder cumulant starts at $2/3$ in the ordered phase and drops to $0$ in the disordered phase, and curves for different $L$ all cross at the same temperature — which is $T_c$. This crossing gives us the critical temperature without any fitting.

In practice, the crossing is not perfectly sharp because of subleading corrections, but using the crossing of the two largest system sizes gives an excellent estimate.

---

## 6. The Susceptibility

The magnetic susceptibility is related to magnetization fluctuations through the **fluctuation-dissipation theorem**:

$$\chi = \frac{N}{T}\left(\langle m^2 \rangle - \langle |m| \rangle^2\right)$$

This makes physical sense: if spins fluctuate a lot (large variance of $m$), the system is easily polarised by an external field (large $\chi$).

At $T_c$, $\chi$ diverges as $|T - T_c|^{-\gamma}$. On a finite lattice, this divergence is cut off at $\xi \sim L$, and the peak value scales as:

$$\chi_{\max}(L) \sim L^{\gamma/\nu}$$

For the 2D Ising model, $\gamma/\nu = 7/4 = 1.75$. Doubling the system size increases the peak susceptibility by a factor $2^{7/4} \approx 3.36$. You can measure $\gamma/\nu$ directly from a log-log plot of $\chi_{\max}$ against $L$ — the slope is $\gamma/\nu$.

---

## 7. Performing the Scaling Collapse

The scaling collapse is where FSS theory really shows its power. Here is the procedure:

1. Run simulations at several system sizes $L$ over a range of temperatures near $T_c$.
2. For each $(T, L)$, compute $\langle |m| \rangle$.
3. Define rescaled variables: $y = L^{\beta/\nu} \langle |m| \rangle$ and $x = (T - T_c)/T_c \cdot L^{1/\nu}$.
4. Plot $y$ against $x$ for all $(T, L)$ pairs.
5. If the exponents and $T_c$ are correct, all points collapse onto a single curve.

A good collapse is a powerful visual statement: it means you have found the correct scaling description of the transition. Any systematic deviation from collapse means either your exponents are wrong or you are outside the scaling regime (too far from $T_c$ or too small $L$).

The notebook shows the collapse with the exact 2D Ising exponents and with the 3D Ising exponents (which are $\beta \approx 0.326$, $\nu \approx 0.630$). The contrast is stark: correct exponents give a tight collapse, wrong exponents give scattered curves that disagree by a system-size-dependent amount.

---

## 8. Extracting $\beta/\nu$ Directly

At exactly $T = T_c$, the scaling form gives:

$$\langle |m| \rangle \sim L^{-\beta/\nu}$$

This means if you plot $\langle |m| \rangle$ evaluated at $T_c$ against $L$ on a log-log plot, the slope gives $-\beta/\nu$ directly. For the 2D Ising model, $\beta/\nu = 1/8 = 0.125$.

In practice you use the temperature grid point closest to $T_c$. The result is somewhat sensitive to how close your grid point is to $T_c$, which is why using the Binder cumulant crossing first to pin down $T_c$ is important.

---

## 9. Universality

One of the deepest insights of critical phenomena is **universality**: the critical exponents depend only on gross features of the model — the spatial dimension $d$ and the symmetry of the order parameter — not on the microscopic details.

Every model with a scalar order parameter ($\mathbb{Z}_2$ symmetry, i.e., $m \to -m$) in two dimensions has exactly the same exponents: $\beta = 1/8$, $\nu = 1$, $\gamma = 7/4$. This includes the Ising model, the lattice gas, the binary alloy model, and many others.

In three dimensions, the same symmetry class (3D Ising universality class) has $\beta \approx 0.326$, $\nu \approx 0.630$, $\gamma \approx 1.237$ — the exponents change, but they are again universal across all models with the same symmetry in the same dimension.

This universality is why the Ising model matters so far beyond its original context as a model of ferromagnetism. Understanding the 2D Ising transition means understanding the universality class itself, which appears in many physical systems.

---

## 10. What Comes Next

We have now completely characterised the classical 2D Ising transition. In notebook 03 we add an external magnetic field, which breaks the $\mathbb{Z}_2$ symmetry and introduces a qualitatively different kind of transition — a **first-order** phase transition. Understanding the difference between first-order and continuous transitions is essential before we move into quantum models.

After that, notebooks 04 and 05 introduce the XY model and the Heisenberg model — models with continuous symmetry groups instead of the discrete $\mathbb{Z}_2$ of the Ising model. These support fundamentally different kinds of order (or, in the XY case in 2D, no order at all — but something more subtle). Once we have built intuition for these classical models, notebook 06 makes the jump to quantum mechanics, where the phase transition is driven by quantum rather than thermal fluctuations.
