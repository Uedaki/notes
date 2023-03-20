# A bit of probability theory

## CDF and PDF

#### Definitions

The cumulative distribution function (cdf) is the probability that a random variable, $X$, will take a value less or equal to $x$.

$$P(x) = P_r \{ X \le x \}$$

!!! note "$P(x)$ can also be written as $F(x)$"

The probability density function (pdf) is the probability that a random variable, $X$, will take a value $x$. In the case of continuous values, the pdf represent the probability per unit length that a random variable, $X$, will be closer to the sample $x$ compared to the other sample.

The cdf can also be expressed as the probability of all the event where $X$ is less or equal to $x$ which can be expressed with the pdf:

$$P(x) = \int_0^x p(X)dX$$

- $p(X)$ is the pdf of the event $X$
- $X$ is a random event
- $dX$ is the rate of change of the random event $X$

Since we can expressed the cdf with the pdf, we use it to express in return the pdf relative to the cdf:

$$ p(x) = \frac{dP}{dx}(x)$$

If we want to compute the probability that a random variable $X$ is in the interval $[\alpha, \beta]$, we can expressed it with the following relationship

$$P_r \{\alpha \le X \le \beta \} = \int_\alpha^\beta p(x)dx = P(\beta) - P(\alpha)$$

#### Multidimensional vector

For multidimensional random vector $(X^1, \dotsc, X^s)$, we can use the joint cumulative distribution function

$$P(x^1, \dotsc, x^s) = P_r \{ X^i \le x^i \text{ for all } i = 1, \dotsc, s \}$$

and the join distribution function

$$p(x^1, \dotsc, x^s) = \frac{ \partial^s P }{\partial x^1 \dotsb \partial x^s}(x^1, \dotsc, x^s)$$

For any Lebesgue measurable subset $D\subseteq\mathbb{R}^s$, we have the following relationship:

$$P_r \{ x \in D \} = \int_D p(x_1, \dotsc, x_s) dx^1 \dotsb dx^s$$

???+ abstract "Lebesgue measure"
    explain what it is

More generally, for a random variable $X \in \Omega$, its probability measure (also known as a probability distribution or distribution) is a measure function $P$ such that for any measurable set $D\subseteq\Omega$, we have

$$P(D) = P_r \{ X \in D \}$$

!!! note "A probability measure must satisfy $P(\Omega) = 1$"

The probability that $X \in D$ can be obtained by integrating $p(x)$ over the given region $D$ using the Radon-Nikodym theorem.

$$P(D) = \int_D p(x) d\mu(x)$$

???+ abstract "Radon-Nikodym theorem"
    The Radon–Nikodym theorem involves a measurable space $(X, \Sigma)$ on which two $\sigma$-finite measures are defined, $\mu$ and $\nu$. It states that, if $\nu \ll \mu$ (that is, if $\nu$ is absolutely continuous with respect to $\mu$), then there exists a $\Sigma$-measurable function $f(X) \in \mathbb{R}^+$ such that for any measurable set $D \subseteq X$
    
    $$v(D) = \int_D f(X) d\mu$$

The can then derived the last equation to get the density function $p$.

$$p(x) = \frac{dP}{d\mu}(x)$$


## Expected value and variance

##### Expected value
The expected value of a random variable with finite number of outcomes is a weighted average of all the possible outcomes. For a continuum of possible outcomes, the expected value is defined by integration.
The expected value or expectation of a random variable $Y = f(X)$ is defined as

$$E[Y] = \int_\Omega f(x)p(x)d\mu(x)$$

!!! note "Rendering paper tend to simplify this equation by removing $p(x)$"
    show the simplification

##### Variance
The variance is a measure of dispersion which means it is a measure of how far a set of numbers is spread out from their average value.

$$V[Y] = E \left[(Y - E[Y])^2 \right]$$

It is assume that the expected value and variance of every random variable exist (i..e. the corresponding integral is finite).

##### Standard deviation
The standard deviation of a random variable (also known as the root mean square error), which is simply the square root of its variance:

$$\sigma[Y] = \sqrt{V[Y]}$$

##### Properties

For any constant $\alpha$:

- $$E[aY] = aE[Y]$$

- $$V[aY] = a^2 V[Y]$$

For any random variables $Y_1, \dotsc , Y_N$:

- $$E \left[ \sum_{i=1}^N Y_i \right] = \sum_{i=1}^N E[Y_i]$$

The following identity holds only if the variables $Y_i$ are independent:

- $$V \left[\sum_{i=1}^N Y_i \right] = \sum_{i=1}^N V [Y_i]$$

!!! note "Simplification of the variance"
    From these rules, we can derive a simpler expression for the variance:
    
    $$V[Y] = E \left[(Y - E[Y])^2 \right] = E \left[Y^2 \right] - E[Y]^2$$

## Conditional and marginal densities

https://machinelearningmastery.com/joint-marginal-and-conditional-probability-for-machine-learning/
https://en.wikipedia.org/wiki/Conditional_probability_distribution
https://study.com/learn/lesson/marginal-vs-conditional-probability-distributions-differences-rules-examples.html
