# Lecture Notes — Step 1: The 2D Ising Model

---

## Overview

In this first step we build the 2D Ising model from scratch and use it to observe a phase transition. This is the foundation for everything that follows. By the end you should understand what the model is, why the Metropolis algorithm works, what magnetization tells you, and what a phase transition actually looks like in a simulation.

---

## 1. Why the Ising Model?

The Ising model is probably the most studied model in all of statistical mechanics. That is not because it is realistic — it is a dramatic simplification of any real magnetic material. It is studied because it is the simplest model that has a phase transition, and because that phase transition belongs to a universality class that shows up in contexts far beyond magnetism: liquid-gas transitions, the ordering of binary alloys, the Higgs mechanism in field theory, and, as we will see later in this series, quantum phase transitions.

The Ising model teaches you how to think about collective phenomena. A single spin has two states and nothing interesting happens to it. Two million spins, coupled together, spontaneously organise into a ferromagnet below a critical temperature. That emergent collective behaviour — order arising from disorder — is what we are here to understand.

---

## 2. The Model

We place spins on a two-dimensional square lattice. Each site $i$ carries a spin variable $s_i$ that can take one of two values: $+1$ (spin up) or $-1$ (spin down). That is it. No quantum mechanics, no continuous variables — just a grid of binary degrees of freedom.

The energy of a configuration is given by the Hamiltonian:

$$H = -J \sum_{\langle i,j \rangle} s_i s_j$$

The sum runs over all nearest-neighbour pairs — in 2D, that means right and left, up and down. The coupling constant $J$ controls the strength and type of interaction. When $J > 0$, aligned neighbours (both $+1$ or both $-1$) have lower energy than anti-aligned ones — this is a ferromagnet. When $J < 0$, the opposite is true — an antiferromagnet. We always work with $J = 1$ unless stated otherwise.

The minus sign in front of $J$ is a convention: it ensures that the ground state (lowest energy configuration) is the fully ordered state where all spins point in the same direction.

**Units.** Throughout this series we work in units where $J = k_B = 1$. Temperature therefore has units of $J/k_B$ and is dimensionless. When we write $T = 2.27$ we mean $T = 2.27\, J/k_B$.

---

## 3. Periodic Boundary Conditions

Our lattice is finite — we cannot simulate an infinite system. But we also do not want the simulation to be dominated by edge effects. The standard solution is periodic boundary conditions: the right edge wraps around to the left, and the top wraps to the bottom. The lattice becomes a torus.

In code, this is handled elegantly with the modulo operator. A spin at column $j$ on a lattice of size $L$ has its right neighbour at column $(j+1) \bmod L$. When $j = L-1$, the right neighbour is column 0.

This is not a physical choice — no real magnet is a torus — but it removes surface effects and makes the system translationally invariant, which is what we want for studying bulk properties.

---

## 4. Computing the Energy

The total energy requires summing over all nearest-neighbour pairs. On a square lattice with $N = L^2$ sites, there are $2N$ such pairs (each site has four neighbours but each pair is shared between two sites, so $4N/2 = 2N$).

One way to do this: loop over every site, and for each site only count its right and bottom neighbours. This way each pair is counted exactly once.

A more important observation: we rarely need the total energy. The Metropolis algorithm (next section) only ever needs the *change* in energy when we flip a single spin. That change only involves the four nearest neighbours of the flipped site, not the entire lattice. This is an $O(1)$ calculation rather than $O(N)$, and it is what makes Monte Carlo simulation tractable.

When spin $s_i$ is flipped, the energy change is:

$$\Delta E = 2J\, s_i \sum_{\delta} s_{i+\delta}$$

where the sum is over the four neighbours. The factor of 2 comes from the difference between $-J(-s_i)\sum s$ and $-J(s_i)\sum s$. Notice that $\Delta E$ only depends on the current spin and its four neighbours — you can verify this by writing out the terms explicitly.

---

## 5. The Metropolis Algorithm

We want to sample spin configurations from the Boltzmann distribution:

$$P(\text{config}) \propto e^{-\beta H(\text{config})}$$

where $\beta = 1/T$. At low temperature, high-energy configurations are exponentially suppressed, so the system concentrates near its ground state. At high temperature, all configurations become equally likely.

Direct sampling from the Boltzmann distribution is impossible for a large lattice — there are $2^N$ configurations and we cannot enumerate them. Instead, we use a Markov chain Monte Carlo method: we construct a random walk through configuration space that, in the long run, visits each configuration with the right Boltzmann probability.

The Metropolis algorithm is the standard way to do this. Each step:

1. Choose a site $(i, j)$ at random.
2. Compute $\Delta E$ for flipping that spin.
3. If $\Delta E \leq 0$: accept the flip (it lowers the energy, so it is always favoured).
4. If $\Delta E > 0$: accept the flip with probability $e^{-\beta \Delta E}$.
5. If rejected: keep the current configuration.

**Why does this work?** The Metropolis rule satisfies *detailed balance*: the rate of transitions from configuration A to configuration B equals the rate of transitions from B to A (when weighted by their Boltzmann probabilities). This is sufficient to guarantee that the Markov chain converges to the Boltzmann distribution.

The acceptance probability $e^{-\beta \Delta E}$ has a natural physical interpretation. At high temperature ($\beta \to 0$), this factor approaches 1 — nearly all moves are accepted regardless of energy cost, and the system explores configurations freely. At low temperature ($\beta \to \infty$), only downward energy moves are accepted and the system gets trapped near its ground state.

**Equilibration.** When we start from a random configuration, the system is initially far from the Boltzmann distribution. We need to run the algorithm for some number of steps — the equilibration time — before the results are meaningful. The equilibration time grows near the critical temperature, a phenomenon called *critical slowing down* that we will revisit in later notebooks.

---

## 6. Magnetization as an Order Parameter

The magnetization per spin is:

$$m = \frac{1}{N} \sum_i s_i$$

This ranges from $-1$ (all spins down) to $+1$ (all spins up). In a ferromagnet, $m$ is the natural *order parameter* — a quantity that is zero in the disordered phase and non-zero in the ordered phase.

At high temperature, the spins are disordered: roughly half point up and half point down, so $m \approx 0$. At low temperature, a spontaneous ordering develops and $|m|$ approaches 1.

A subtlety: on a finite lattice, the Hamiltonian has a symmetry under $s_i \to -s_i$ (all spins flipped). This means $\langle m \rangle = 0$ exactly, even at low temperature, because the system will eventually flip between the two ordered states (all up and all down). We therefore track $\langle |m| \rangle$ to detect ordering on finite systems.

---

## 7. The Phase Transition

When you run the temperature sweep, you observe a sharp drop in $\langle |m| \rangle$ near $T \approx 2.27$. This is the ferromagnetic phase transition.

Below $T_c$, the system is in the **ordered phase**: thermal fluctuations are weak enough that the ferromagnetic coupling wins, and the spins align into a macroscopic domain.

Above $T_c$, the system is in the **disordered phase**: thermal fluctuations destroy any long-range order and $\langle |m| \rangle \to 0$.

At $T_c$ itself, the system is critical. The correlation length — the distance over which spins are correlated — diverges. Large ordered domains form and dissolve on all length scales simultaneously. This scale-free behaviour is what makes the critical point so special, and so computationally interesting.

The exact value of $T_c$ in 2D was computed by Lars Onsager in 1944:

$$T_c = \frac{2}{\ln(1 + \sqrt{2})} \approx 2.2692$$

This is one of very few exact results in statistical mechanics. We will use it to benchmark our numerics throughout the series.

---

## 8. What You Should Notice in the Simulation

When you look at the lattice snapshots, pay attention to three regimes:

**Well below $T_c$:** The lattice is nearly uniformly one colour — almost all spins aligned. Tiny disordered patches appear and disappear.

**Near $T_c$:** Domains of all sizes coexist. There is structure at every length scale, from single-site fluctuations to domain boundaries spanning the whole lattice. This is what a scale-free system looks like.

**Well above $T_c$:** The lattice looks like random noise. There is no persistent structure at any scale.

The magnetization time series also tells a story. At low temperature, $m$ fluctuates near $+1$ or $-1$ and rarely crosses zero. Near $T_c$, $m$ fluctuates strongly and crosses zero frequently. At high temperature, $m$ fluctuates symmetrically around zero.

---

## 9. Limitations and What Comes Next

The simulation you just ran has one obvious limitation: the transition looks rounded rather than sharp, and the apparent $T_c$ does not quite match Onsager's value. This is not a simulation error — it is a genuine physical effect called *finite-size rounding*. A 25×25 lattice is not thermodynamically large, and the correlation length cannot diverge beyond the system size.

In the next notebook we take this limitation seriously and turn it into a tool. By running the same simulation at several system sizes and comparing the results, we can extract the exact critical temperature and critical exponents without any assumptions about the functional form of the transition. This is the method of *finite-size scaling*, and it is one of the most powerful techniques in computational statistical mechanics.
