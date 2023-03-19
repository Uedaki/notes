# Robust Monte Carlo Methods For Light Transport Simulation

### Chapter 1: Introduction

The focus is set on unbiased, view dependent, Monte Carlo algorithms.

> A view-independent algorithm is an algorithm that computes an intermediate representation of the solution, from which we can create arbitrary views quickly. Any other algorithm is view-dependant.

> Put it simply, an unbiased estimator computes the correct answer, on average. A biased estimator computes the wrong answer on average. However, if a biased estimator is consistent, then the average error can be made arbitrarily small by increasing the sample size.

The rainbow effect created by a light going through a transmission is called a `dispersion`

## Chapter 2: Monte Carlo Integration

### Probability theory

#### CDF and PDF functions

Cumulative distribution function (cdf) of a real-valued random variable $X$ is defined as:

$$P(x) = P_r \{ X \le x \} = \int_0^x p(X)dX$$

> The cdf is the sum of the probability that an event $X$ is less or equal to $x$

> $dx$ represent the volume integrated for the integral

> $P(x)$ can also be written as $F(x)$

and the corresponding probability density function (also known as pdf) is:

$$ p(x) = \frac{dP}{dx}(x)$$

> $p(x)$ represent the rate of change of the cdf for an event $x$.

> For a function $f(x) = y$, if we define $F$ as the sum of all the values of $f(x)$, therefore $f(x) = y = \frac{dF}{dx}(x)$.

This leads to the following relationship:

$$P_r \{\alpha \le X \le \beta \} = \int_\alpha^\beta p(x)dx = P(\beta) - P(\alpha)$$

> This equation is just an extension of the cdf equation if you think of $P(x) = P_r \{ 0 \le X \le x \}$ where $\alpha = 0$ and $\beta = x$.

The corresponding notions for a multidimensional random vector $(X^1, \dotsc, X^s)$ are the joint cumulative distribution function:

$$P(x^1, \dotsc, x^s) = P_r \{ X^i \le x^i \text{ for all } i = 1, \dotsc, s \}$$

and the joint density function:

$$p(x^1, \dotsc, x^s) = \frac{ \partial^s P }{\partial x^1 \dotsb \partial x^s}(x^1, \dotsc, x^s) = \prod_{i = 1}^s \frac{\partial P}{\partial x^i}dx^i$$

> Jacobien

so that we have the relationship:

$$P_r \{ x \in D \} = \int_D p(x_1, \dotsc, x_s) dx^1 \dotsb dx^s$$

for any Lebesgue measurable subset $D \sub \reals^s$.

> The 3 equations above are just an evolution of the first ones with $x^1, \dotsc, x^s$ instead of $X$

More generally, for a random variable $X$ with values in an arbitrary domain $\Omega$, its probability measure (also known as a probability distribution or distribution) is a measure function $P$ such that:

$$P(D) = P_r \{ X \in D \}$$

for any measurable set $D \sub \Omega$. In particular, a probability measure must satisfy $P(\Omega) = 1$.

The corresponding density function $p$ is defined as the Radon-Nikodym derivative

$$p(x) = \frac{dP}{d\mu}(x)$$

> $d\mu$ replaced $dx$

> $\mu$ is a measure of density corresponding on the volume integrated (i.e. area, solid angle, etc...)

which is simply the function $p$ that satisfies

$$P(D) = \int_D p(x) d\mu(x)$$

The probability that $X \in D$ can be obtained by integrating $p(x)$ over the given region $D$.

#### Expected value and variance

The expected value or expectation of a random variable $Y = f(X)$ is defined as

$$E[Y] = \int_\Omega f(x)p(x)d\mu(x)$$

> The expected value of $Y$ is equal to the sum of all the value of $f(x) \in \Omega$ multiplied by its pdf ($p(x)$) and by the derivative of the measure of density ($d\mu(x)$).

> Rendering paper tend to simplify this equation by removing $p(x)$

while its variance is

$$V[Y] = E \left[(Y - E[Y])^2 \right]$$

It is assume that the expected value and variance of every random variable exist (i..e. the corresponding integral is finite).

> The variance of $Y$ is equal to the expected value of the square root of, $Y$ minus the expected value of $Y$.

Another useful quantity is the standard deviation of a random variable, which is simply the square root of its variance:

$$\sigma[Y] = \sqrt{V[Y]}$$

This is also known as the root mean square (RMS) error.

##### Properties

For any constant $\alpha$:

- $$E[aY] = aE[Y]$$

- $$V[aY] = a^2 V[Y]$$

For any random variables $Y_1, \dotsc , Y_N$:

- $$E \left[ \sum_{i=1}^N Y_i \right] = \sum_{i=1}^N E[Y_i]$$

The following identity holds only if the variables $Y_i$ are independent:

- $$V \left[\sum_{i=1}^N Y_i \right] = \sum_{i=1}^N V [Y_i]$$

> Notice that from these rules, we can derive a simpler expression for the variance:

> - $$V[Y] = E \left[(Y - E[Y])^2 \right] = E \left[Y^2 \right] - E[Y]^2$$

#### Conditional and marginal densities

Let $X \in \Omega_1$ and $Y \in \Omega_2$ be a pair of random variables, so that:

$$(X, Y) \in \Omega$$

where $\Omega = \Omega_1 \times \Omega_2$.

Let $P$ be the joint probability measure of $(X, Y)$, so that $P(D)$ represents the probability that $(X, Y) \in D$ for any measurable subset $D \sub \Omega$. Then the corresponding joint density function $p(x, y)$ satisfies

$$P(D) = \int_D p(x, y) d\mu_1(x) d\mu_2(y)$$

where $\mu_1$ and $\mu_2$ are measures on $\Omega_1$ and $\Omega_2$ respectively.

The marginal density function of X is now defined as

$$p(x) = \int_{\Omega_2} p(x, y)dy$$

> Reminder that $dy$ is the simplified notation of the measure function notation $d\mu(y)$

while the conditional density function $p(x|y)$ is defined as

$$p(x|y) = \frac{p(x, y)}{p(x)}$$ 

The marginal density $p(y)$ and conditional density $p(x | y)$ are defined in a similar way, leading to the useful identity:

$$p(x, y) = p(y | x) p(x) = p(x | y) p(y)$$

Another important concept is the conditional expectation of a random variable $G = g(X, Y)$, defined as

$$E \left[ G | x \right] \int_{\Omega_2} g(x, y) p(y|x) dy = \frac{\int g(x, y) p(x, y) dy}{\int p(x, y) dy}$$

> $E_Y[G]$ is used for conditional expectation, to emphasizes the fact that Y is the random variable whole density function is being integrated.

There is a very useful expression for the variance of G in terms of its conditional expectation and variance, namely

$$V[G] = E_X V_Y G + V_X E_Y G$$

> $V[G]$ is the mean of the conditional variance, plus the variance of the conditional mean

### Basic Monte Carlo integration

The idea of Monte Carlo integration is to evaluate the integral:

$$I = \int_\Omega f(x) d\mu(x)$$

using random sampling. It is done by independently sampling $N$ points $X^1, \dotsc, X^s$ according to a density function $p$, and then computing the estimate

$$F_N = \frac{1}{N} \sum_{i = 1}^{N} \frac{f(X_i)}{p(X_i)}$$

> It used $F_N$ rather than $I$ to emphasize that the result is a random variable

> Monte Carlo integration converges at a rate of $O(N^{-1/2})$ in any dimension

For further details:

- 2.4.1 convergence rates: proof of the convergence rates

#### Monte Carlo estimator

We define $Q$ (called estimand), as the value of a given integral.

##### Properties

- The quantity $F_N - Q$ is called the `error`, and its expected value is called the `bias`:

$$\beta[F_N] = E[F_N - Q]$$

- an estimator is called `unbiased` if $\beta[F_N] = 0$ for all sample sizes $N$ which means

$$E[F_N] = Q \text{ for all } N \ge 1$$ 

- an estimator is called `consistent` if the error $F_N - Q$ goes to zero with probability one

$$P_r \left\{ \lim_{ N \to \infty} F_N = Q \right\} = 1$$

- another way to say it, it that for an estimator to be consistent, the bias and variance both have to go to zero as $N$ is increased

$$\lim_{N \to \infty} \beta[F_N] = \lim_{N \to \infty} V[F_N] = 0$$

- an unbiased estimator is consistent as long as its variance decrease to zero as $N$ goes to infinity

> To not confuse with the fact of being unbiased which means that the result of the integral does not have bias for all samples $N$.

##### Unbiased estimators

Typically the goal is to minimize the mean squared error ($MSE$), defined by

$$MSE[F] = E[(F - Q)^2]$$

Which can be simplified in (Veach p.44 for how to simplify)

$$MSE[F] = V[F] + \beta[F]^2$$

For unbiased estimators, we can simplified it even further since $\beta[F] = 0$

$$MSE[F] = V[F] = E[(F - E[F])^2]$$

$\beta$ is non-trivial to compute, which make computing the error difficult for biased estimator. Thus, error estimates are easier to obtain for unbiased estimators. 

Letting $Y_1, \dotsc, Y_N$ be independent samples of an unbiased estimator $Y$, and letting

$$F_N = \frac{1}{N}\sum_{i = 1}^N Y_i$$

as before, then the quantity is

$$\hat{V}[F_N] = \frac{1}{N-1} \left\{ \left( \frac{1}{N} \sum_{i = 1}^N Y_i^2 \right) - \left( \frac{1}{N} \sum_{i = 1}^N Y_i \right)^2 \right\}$$

> Look for the simpler version of the variance

> The hat signal that it is relative to an estimator

The error of an unbiased estimator can be made as small as desired since

$$V[F_N] = V[F_1] / N$$

The goal is to find an estimator whose variance and running time are both small. This is describe by the efficiency of a Monte Carlo estimator:

$$\epsilon[F] = \frac{1}{V[F]T[F]}$$

where $T[F]$ is the time required to evaluate $F$.

### Variance reduction: Analytic integration

The techniques developed to design efficient estimators are often called `variance reduction methods`. These methods are usually grouped into categories base around 4 main ideas:

- `analytically` integrating a function that is similar to the integrand

- `uniformly` placing sample points across the integration domain

- `adaptively` controlling the sample density based on information gathered during sampling

- combining samples from two or more estimators whose values are correlated

#### Use of expected values

One way to reduce variance is to reduce the dimension of the sample space, by integrating analytically with respect to one or more variables of the domain. It consists of replacing an estimator of the form

$$F = f(X, Y) / p(X, Y)$$

with one of the form

$$F' = f'(X) / p(X)$$

where $f'(x)$ and $p(x)$ are defined by

$$f'(x) = \int f(x, y) dy$$

$$p(x) = \int p(x, y) dy$$

> This formulas explain the relationship between $f'(x)$, $p(x)$ and respectively $f(x, y)$, $p(x, y)$.

Thus, to apply this technique, we must be able to integrator both $\int$ and $p$ with respect to y.

> The use of expected values is the preferred variance reduction technique, as long as it is not too expensive to evaluate and sample the analytically integrated quantities.

> Note that if expected values are used for only one part of a larger calculation, then variance can increase.

#### Importance sampling

Importance sampling refers to the principle of choosing a density function $p$ that is similar to the integrand $\int$.

The best choice is to let $p(x) = cf(x)$, where the constant of proportionality, to ensure $p$ integrates to one, is

$$c = \frac{1}{\int_{\Omega}f(y)d\mu(y)}$$

This leads to an estimator with zero variance, since

$$F = \frac{f(X)}{p(X)} = \frac{f(X)}{cf{X}} = \frac{1}{c}$$

for all sample points $X$.

> Not practical since we must already know the value of the desired integral to compute the constant

>
Importance sampling is one of the most useful and powerful techniques of Monte Carlo integration. 

#### Control variates

The idea behind control variates, is to find a function $g$ that can be integrated analytically and is similar to the integrand, and then subtract it. The integral is rewritten as

$$I = \int_{\Omega} g(x) d\mu(x) + \int_{\Omega} f(x) - g(x) d\mu(x)$$

and then sampled with an estimator of the form

$$F = \int_{\Omega} g(x) d\mu(x) + \frac{1}{N} \sum_{i = 1}^{N} \frac{f(X_i - g(X_i))}{p(X_i)}$$

where the value of the first integral is known exactly.

The estimator will have a lower variance than the basic estimator whenever

$$V\left[ \frac{f(X_i) - g(X_i)}{p(X_i)} \right] \le V \left[ \frac{f(X_i)}{p(X_i)} \right]$$

Given a function $g$ that is an approximation to $f$, we must decide whether to use it as a control variate or as a density function for importance sampling.

> It is possible that either one of the choice could be the best

In general, if:

- $f - g$ is nearly a constant function, then $g$ should be used as a control variate

- $f/g$ is nearly a constant, then $g$ should be used for importance sampling

> Control variates have very few application in graphics currently. This technique can produce negative sample values, even for an integrand that is strictly positive. This can lead to large relative errors for integrals whose true value is close to zero.

> On the other hand, the method is straightforward to apply, and can potentially give a modest variance reduction at little cost.

### Variance reduction: Uniform sample placement

Another important strategy for reducing variance is to ensure that samples are distributed more or less uniformly over the domain.

This technique usually assumed that the domain is the $s$-dimensional unit cube $[0, 1]^s$. The other domain can be handled by defining the appropriate transformation of the form $T: [0, 1]^s \rightarrow \Omega$.

> Note that by choosing different mapping $T$, the transformed samples can be given different density functions

#### Stratified sampling

The idea of stratified sampling is to subdivide the domain $\Omega$ into several non-overlapping regions $\Omega_1, \dotsc, \Omega_m$ such that

$$\bigcup_{i = 1}^n \Omega_i = \Omega$$

> The union of all the regions $\Omega_i$ from 0 to $n$ is equal to the domain $\Omega$ 

Each region $\Omega_i$ is called a stratum. A fixed number of samples $n_i$, is taken within each $\Omega_i$, according to some given density function $p$.

Stratified sampling is a useful, inexpensive variance reduction technique. It is mainly effective for low dimensional integration problems where the integrand is reasonably well-behaved.

#### Latin hypercube sampling

The idea of latin hypercube sampling is to subdivide the domain $[0, 1]^s$ into $N$ sub-intervals along each dimension, and to ensure that one sample lies in each sub-interval.

$$X_i^j = \frac{\pi_j(i) - U_{i, j}}{N}$$

- $X_i^j$ denotes the $j$-th coordinate of the sample $X_i$

- $U_{i, j}$ are independent and uniformly distributed on $[0, 1]$

In two dimensions, the sample pattern corresponds to the occurrences of a single symbol in a Latin square (i.e an $N \times N$ array of $N$ symbols such that no symbol appears twice in the same row or column).

#### Orthogonal array sampling

#### Quasi-Monte Carlo methods

### Variance reduction: Adaptive sample placement

#### Adaptive sampling

do some research

#### Russian roulette and splitting

Their purpose is to decrease the sample density where the integrand is small and increase it where the integrand is large. These technique do not introduce bias.

##### Russian roulette

It is applied to estimators that are the sum of many terms: $F = F_1 + \dotsc + F_N$. This technique solve the issue where the contributions are very small, and yet as expensive to evaluate as the others contributions. The idea of russian roulette is to randomly skip most of the evaluations associated with small contributions, by replacing these $F_i$ with new estimators of the form

$$F_i' = \begin{cases} \frac{1}{q_i}F_i & \quad \text{with probability } q_i \\ 0 & \quad \text{otherwise} \end{cases}$$

The evaluation probability $q_i$ is chosen for each $F_i$ separately, based on some convenient estimate of its contribution.

> The estimator $F_i'$ is unbiased whenever $F_i$.

> This technique increases variance. Russian roulette can still increase efficiency by reducing the average time required to evaluate $F$.

Suppose that each $F_i$ represents the contribution of a particular light source to the radiance reflected from a surface. To reduce the number of visibility tests using Russian roulette, we first compute a tentative contribution $t_i$ for each $F_i$ ignoring blockers. The a fixed threshold $\delta$ is typically chosen, and the probabilities $q_i$ are set to

$$q_i = \min(1, t_i / \delta)$$

> Thus contribution larger than $\delta$ are always evaluated, while smaller contribution are randomly skipped in a way that does not cause bias.

##### Splitting

Russian roulette is closely related to splitting, a technique in which an estimator $F_i$ is replaced by one of the form

$$F_i' = \frac{1}{k} \sum_{j = 1}^k F_{i, j}$$

where the $F_{i, j}$ are independent samples from $F$. 

### Variance reduction: Correlated estimators

#### Antithetic variates

#### Regression methods

## Chapter 3: Radiometry and Light Transport

manifold?

phase space?

trajectory space?

| Term | Description |

|:-:|:-:|

| $\mathcal{M}$ | union of a finite set of surfaces in $\reals^3$ (scene geometry)|

| $\sigma$ | solid angle |

| $\sigma^\perp_x$| projected solid angle measure on surface x |

## Chapter 4: A General Operator Formulation of Light Transport

## Chapter 8: A Path Integral Formulation of Light Transport

## Chapter 9: Multiple Importance Sampling

## Chapter 10: Bidirectional Path Tracing

## Chapter 11: Metropolis Light Transport

## Links

- [Robust Monte Carlo Methods For Light Transport Simulation](https://graphics.stanford.edu/papers/veach_thesis/thesis.pdf), Eric Veach

- [Monte Carlo theory, methods and examples](https://artowen.su.domains/mc/), Art Owen