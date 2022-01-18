## Summary

[Paper URL](https://arxiv.org/pdf/1812.09808.pdf)




- The issue they handled
  - In practice, obtaining accurate distribution information is a challenging task

- How
  - designing a control policy that is robust against errors in the empirical distribution obtained from data.
  - zero-sum dynamic game problem: the action space of the adversarial player is a Wasserstein ball centered at the empirical distribution



- Assumption: 
  - from theory of stochastic optimal control
    - probability distribution of uncertain variables (e.g., disturbances) is fully know
  
    - restrictive in practice
      - estimating an accurate distribution requires large-scale high-resolution sensor measurements over a long training period or multiple periods.

- Objective:
  - overcome issue of limited distribution information in stochastic control
    - investigate a distributionally robust control approach
  - model the ambiguity set as a statistical ball centered at an empirical distribution with a radius measured by the Wasserstein metric

- Wasserstein ball
  - contains continuous and discretec distributions while statistical balls with the φ-divergence 
  - such as the Kullback-Leibler divergence centered at a discrete empirical distribution is not sufficiently rich to contain relevant continuous distributions

- Wasserstein metric 
  - addresses the closeness between two points in the support, unlike the φ-divergence

- φ-divergence is incapabile
  - in terms of taking into account the distance between two support elements
  - associated ambiguity set may contain irrelevant distributions 

- propose
  - 1
    - computationally tractable value and policy iteration algorithms with explicit estimates of the number of iterations necessary for obtaining an ∈-optimal policy
    - original Bellman equation involves an infinite-dimensional minimax optimization problem, where the inner maximization problem is over probability measures in the Wasserstein ball
    - because of computational issue without sacrificing optimality: Bellman operators by using modern DRO based on Kantorovich duality
  
  - 2
    - when $π^*$ is used, a probabilistic bound holds on the closed-loop performance evaluated under a new set of samples that are selected independently of the training data
    - contraction property of the Bellman operator seamlessly connects a single-stage performance guarantee to its multi-stage counterpart in a manner that is independent of the number of stages
  
  - 3
    - Wasserstein penalty problem and derive an explicit expression of the optimal control policy and the worst-case distribution policy, along with a Riccati-type equation in the linear-quadratic setting

  - 4 
    - resulting control policy converges to the optimal policy of the corresponding linear-quadraticGaussian (LQG) problem as the penalty parameter tends to +∞

- Ambiguity in Stochastic Systems
  - discrete-time stochastic system
    - $x_{t+1} = f(x_{t}, u_{t}, w_{t})$
    - system state $x_t ∈ X ⊆ R^n$
    - control input $u_t ∈ U ⊆ R^m$
    - random disturbance $w_t ∈ W ⊆ R^l$
    -  probability distribution of $w_t$ denoted by $µ_t$
  - probability distribution is not fully known
  - probability distribution is difficult to estimate accurately
  - **One approach: sample average approximation (SAA) method and solve the corresponding optimal control problem with the empirical distribution.** 
    - SAA-control problem can be formulated as:
      - $(SAA-control) \ inf_{π∈Π} \ E^{π}_{w_t∼ν_N} \ [ \sum^{∞}_{t=0} \alpha^tc(x_t,u_t)|x_0=x]$
      - $ν_N$ denotes the empirical distribution constructed from the N-samples
        - $v_N \:=\ 1/N \sum^{N}_{i=1}  δ_{{wˆ}^{(i)}} $
          - with the Dirac delta measure $δ_{{wˆ}^{(i)}} $ concentrated at $wˆ^{(i)}$
          - α ∈ (0, 1) is a discount factor,
          - c : X × U → R is a stage-wise cost function of interest
          - $E^{π}_{w_t∼ν_N}$ denotes the expected value taken with respect to the probability measure induced by the control policy π and the empirical distribution ν

      - As the number of samples, N, tends to infinity, the empirical distribution ν well approximates the true distribution µ; thus, an optimal policy of the SAA-control problem presents a near-optimal performance
  
  - policy minimizes the worst-case total cost that is calculated under a probability distribution contained in a given set D ⊂ P(W), which is called the *ambiguity set* of probability distributions
  - ambiguity set can be designed to adequately characterize errors in the empirical distribution

- Distributionally Robust Policy
  - can be formulated as a two-player zero-sum dynamic game problem, where the action space of the adversarial player is a Wasserstein ball centered at the empirical distribution.
    - $E^{π, γ}$ denotes expectation with respect to the probability measure induced by the strategy pair (π, γ) ∈ Π × Γ
    - $H_t$ be the set of histories up to stage t, whose element is of the form $h_t:= (x_0, u_0, · · · , x_{t−1}, u_{t−1}, x_t)$
    - set of admissible control strategies (for Player 1 of above game) is given by ${Π := \{π := (π_0, π_1, ...) | π_t(U(x_t)|h_t) = 1 ∀h_t ∈ H_t\}}$,
      - $π_t$ = stochastic kernel from $H_t$ to $R^m$
      - $U(x_t) ⊆ U@ is the set of admissible control actions
    - Same for Player 2 as $Γ:=\{γ := (γ_0, γ_1, ...) | γ_t(D|h^e_t) = 1 ∀h^e_t ∈ H^e_t\}$
      - $H^e_t$ is the set of extended histories up to stage t
      - $h^e_t:= (x_0, u_0, µ_0, ··· , x_{t−1}, u_{t−1}, µ_{t−1}, x_t, u_t)$
      - $γ_t$ is stochastic kernel from $H_t$ to P(W)
    - ambiguity set D is the action space of Player II
    - strategy space for Player II is larger than necessary, and this gives an advantage to the adversary (Player II can change the distribution of $w_t$ over time)
    - infinite-horizon discounted cost function:
      - $J_x((π, γ):= \ E^{π, γ}[\sum^{∞}_t=0 \alpha^tC(x_t,u_t)| x0 = x]$
    
  - standard assumption for measurable selection in semicontinuous models
    - **Assumption 1:** Let K := {(x,u) ∈ X × U | u ∈ U(x)}.
        1. The function c is lower semicontinuous on K, and |c(x,u)| ≤ bξ(x) ∀(x,u) ∈ K, for some constant b ≥ 0 and continuous function ξ : X → [1, ∞) such that $ξ'(x,u) := \int_w ξ(f(x,u, w))µ(dw)$ is continuous on K for any µ ∈ D. In addition, there exists a constant β ∈ [1, 1/α) such that ξ'(x,u) ≤ βξ(x) for all (x,u) ∈ K;
        2. For each continuous bounded function χ : X → R, the function $χ'(x,u) := \int_w χ(f(x,u, w))µ(dw)$ is continuous on K for any µ ∈ D;
        3. The set U(x) is compact for every x ∈ X , and the set-valued mapping x → U(x) is upper semicontinuous.
      
    - optimal distributionally robust policies
      - A control policy $π*$ ∈ Π is said to be an optimal distributionally robust policy if it satisfies
        - $sup_{γ∈Γ} J_x(π^*, γ) ≤ sup_{γ'∈Γ} \ J_x(π, γ')∀π ∈ Π$
        - an optimal distributionally robust policy achieves the minimal cost under the most adverse policies that select disturbance distributions in the ambiguity set D
          - can be obtained by solving the following problem:
            - (DR-control) $inf_{π∈Π} \ sup_{γ∈Γ} \ J_x(π, γ)$
        

  - Wasserstein Ambiguity Set
    - Let D be a statistical ball centered at the empirical distribution $ν_N$ with radius $θ > 0$: 
      - $D := {µ ∈ P(W) | W_p(µ, ν_N ) ≤ θ}$
    - Here, the distance between the two probability distributions is measured by the Wasserstein metric of order p ∈ [1, ∞)
      - $W_p(µ, ν_N ) := min_κ∈P(W^2) \{[\int_{W^2}d(w, w')^p κ(dw, dw')]^{1/p}| Π^1κ = µ, Π^2κ = ν_N\}$ this is MongeKantorovich problem
        - d is a metric on W, 
        - $Π^iκ$ denotes the ith marginal of κ for i = 1, 2
    - Wasserstein distance between two probability distributions represents the minimum cost of transporting or redistributing mass from one to another via non-uniform perturbation, and the optimization variable κ can be interpreted as a transport plan
    - minimum of MongeKantorovich problem can be found by solving the following dual problem:
      - $W_p(µ, ν_N )^p = sup_{ϕ,ψ∈Φ} [\int_W {ϕ(w) µ(dw)} + \int_W {ψ(w') ν_N (dw')}]$
        - where $Φ := {(ϕ, ψ) ∈ L^1 (dµ) × L^1 (dν_N ) | ϕ(w) + ψ(w') ≤ d(w, w')^p ∀w, w0 ∈ W}$ (Kantorovich duality principle)
    - The Wasserstein ambiguity set is equivalent to
      - $ D := \{µ ∈ P(W) | \int_W {ϕ(w) µ(dw)} + 1/n (\sum^{N}_{i=1} \ inf_{w∈W} [d(w, w'^{(i)})^p - ϕ(w)] ≤ θ^p ∀ϕ ∈ L^I(dµ))\}$

  - solved DR-control problem with computationally tractable dynamic programming in 3 rd section of paper

  - defect of the SAA-control formulation:
    -  optimal policy may not perform well if a testing dataset of $w_t$ is different from the training datase