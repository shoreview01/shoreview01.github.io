---
title: "Particle Swarm Optimization"
---

## Introduction

Particle Swarm Optimization (PSO) is one of the swarm-based optimization methods which objective is to obtain global optimum of many particles. The objective function and the objective are given as

$$
\begin{align}
    \text{Objective function }& f : \mathcal{X} \to \mathbb{R}\\
    \text{Objective: }& \mathbf{g}^* = \underset{\mathbf{x}}{\arg\min}\; f(\mathbf{x})
\end{align}
$$

and positions of all particles $\mathbf{x}_i$ have to converge to $\mathbf{g^*}$, where $i \in \mathcal{P} = \{1, \cdots, N\}$. The optimization has four steps: algorithm initialization, velocity calculation, position update, and evaluation.

## Algorithm Initialization

Using a random number generator with bound of $[b_{lb}, b_{ub}]$, initialize position $\mathbf{x}_i$ and velocity $\mathbf{v}_i$ for all $i$'s in $\mathcal{P}$. The bounds are arbitrarily decided by user.

$$
\begin{gather}
    \forall i \in \mathcal{P},\; \mathbf{x}^{(0)}_i \sim U(b_{lb},b_{ub})\\
    \forall i \in \mathcal{P},\; \mathbf{v}^{(0)}_i \sim U(b_{lb},b_{ub}).
\end{gather}
$$

## Velocity Calculation

Before calculating the velocity, set hyperparameters $\phi_1$ and $\phi_2$ which determine importances of separate particle and whole swarm by

$$
\begin{gather}
    r_1 \sim U(0, \phi_1)\\
    r_2 \sim U(0, \phi_2),
\end{gather}
$$

where $r_1$ is an importance of information by one particle and $r_2$ is an importance of information from whole swarm. If the stage is in $k$ and we want to update velocity of $(k+1)$-th stage,

$$
\begin{gather}
    \forall i \in \mathcal{P},\; \mathbf{v}^{(k+1)}_i \gets \mathbf{v}^{(k)}_i + r_1(\mathbf{p}_i - \mathbf{x}^{(k)}_i) + r_2(\mathbf{g}-\mathbf{x}^{(k)}_i),\\
    \mathbf{p}_i: \text{Best position that $i$ has found until stage $k$}\notag\\
    \mathbf{g}: \text{Best position whole swarm has found until stage $k$}.
\end{gather}
$$

## Position Update

Update positions of all $i$'s in stage $k+1$ by
$$
\begin{align}
    \mathbf{x}^{(k+1)}_i \gets \mathbf{x}^{(k)}_i + \mathbf{v}^{(k+1)}_i.
\end{align}
$$

## Evaluation

Insert updated positions to the objective function. Then proceed

$$
\begin{gather}
    \text{If } f(\mathbf{x}^{(k+1)}_i) < f(\mathbf{p}_i),\;\; \mathbf{p}_i \gets \mathbf{x}^{(k+1)}_i\\
    \text{If } f(\mathbf{x}^{(k+1)}_i) < f(\mathbf{p}_i) \text{ and } f(\mathbf{p}_i) < f(\mathbf{g}),\;\; \mathbf{g} \gets \mathbf{p}_i.
\end{gather}
$$

Finish the algorithm when all positions converge.
