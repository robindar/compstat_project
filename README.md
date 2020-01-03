## Problem setting

This paper studies optimization problems of the form
`min (F theta)` with `F = f + g`.

`f` is a measure of fit typically differentiable with Lipschitz gradient,
and `g` a regularization which might be non-smooth including
for instance sparsity-inducing penalties.

The proximal gradient descent algorithm works well in this setting
and provably converges to the global optimum when both are convex.


## Issue

In modern applications, the gradient of the objective is often intractable.
Performing a gradient step, although required once per iteration for the
proximal gradient algorithm, can be a very difficult problem.

This has been largely studied in the differentiable case, for instance
with stochastic gradient descents, but an equivalent study was not
available for the proximal gradient case before this study.

In this work, an approximation `H (n+1)` is available instead of
the real gradient of the objective `nabla f (theta n)`.


## Possible assumptions cited in the paper

H1 : f has Lipschitz gradient

H2 : the set of minimizers is non-empty

H3 : `nabla f theta = int (H theta x) (pi theta dx)` for some `pi theta`
and an integrable function `H`

H4 : `H (n+1)` is a Monte Carlo approximation of `nabla f (theta n)` for all `n`

H5a : there exists a measurable W such that `H` has W-finite measure
 and the underlying transition kernel is affine bounded on `W^p` for some `p > 1`

H5b : `sup | ((P theta)^n x) - (pi theta) |_(W^l) < C rho^n (W^l x)` for some `C` and `rho < 1`

H6a : `(H theta) + sup_x (P theta x) + (pi theta)` is Lipshitz in theta w.r.t measure W

H6b : `prox (gamma *g)` has finite reach

H6c : steps size varies smoothly (`sum ((gamma (n+1)) - (gamma n)) < inf`)


## Questionning assumptions

H2 is reasonable, since otherwise the problem rarely makes sense because
the feasible set of parameters is compact.

H1 is reasonable in most cases, typical objectives include L2 norms composed
with linear operators, which all give Lipschitz gradients.

H3 : seems reasonable. gradient as an expectation occurs naturally when
the objective is defined as an expectation, such as mean over a training set.

H4 : seems reasonable. to be confirmed. stochastic approximations are the first
option considered when true values are not available (cf deep learning).
Monte Carlo is a classical sampling method to obtain such values.

H5 : I have no clue. Seems rather restrictive, but maybe it's fine in practice.

H6 : No clue again for 6c. 6a and 6b seem to be always verified.


## Perturbated proximal gradient

Same algorithm as PGD, with `H (n+1)` instead of `nabla f theta`.
Nothing new here, value comes from the analysis and convergence bound.

`theta (n+1) = prox ((gamma (n+1)) * g) ((theta n) - (gamma (n+1)) (H (n+1)))`

Two samplings are considered:
- iid Monte Carlo (unbiased)
- MCMC (biased but more reasonable)

Convergence is guaranteed with MC averaging if
`(sum gamma) = inf` and `(sum gamma ** 2 / m) < inf`

Two cases achieving these conditions are cited:
- fixed step size `gamma` and linearly increasing batch size `m`
- decreasing step size `gamma` and fixed `m`

Roughly the same result holds for the MCMC case


## Stochastic proximal gradient

Proofs for both settings (fixed batch size, increasing batch size) are
interesting but probably too long to reproduce in the constrained presentation setting.

I am unable to assert that the proofs are correct at the time. To be checked


## Application : Network structure estimation

The article considers a discrete graphical model. The number of nodes is taken
large enough to make the objective and its gradient intractable.

The normalization constant is generally intractable, and most usable algorithms
use the pseudo-likelihood instead, which is more manageable. Another key to keep
computations tractable is to use high regularization constants with a
sparsity-inducing penalty, guaranteeing that non-sparse-enough solutions
are ignored because suboptimal thanks to the regularization.

This corresponds to the problem setting here : smooth likelihood given by
the exponential factorization on the graphical model, and non-smooth but convex
sparsity-inducing penalty.
Both the likelihood and its gradient are intractable, and direct simulation
is rarely feasible, but with an MCMC estimator, it becomes possible
to estimate the network structure with perturbated proximal gradient.

Both fixed and linearly increasing step sizes are considered, as previously.
