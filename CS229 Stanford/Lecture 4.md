* Exponential Family Models
	* Definition & Motivation
	* Examples
	* Softmax (Multiclass Classification)


# Exponential Family

* Not the state of the art

**IDEA:** "If $p$ has a special form => some questions/inference/learning for free"

> "What we mean by 'come for free' is what you already know automatically applies to them. When we start working with more complicated models, this will form a kind of subroutine that we'll use again and again." -Re

==What does that mean?==

Form looks like:    $P{(y;\eta)} = b(y)\ exp\{\eta^T T(y) - a(\eta)\}$

The above suggests that our probability distribution ($P(y;\eta)$) can be written in the above form. **Not every probability distribution can.** The above is a formal way of saying that while there are many probability distribution functions, this *particular* one can be written this way.

* $P(y;\eta)$ = Probability distribution 
* $y$ = Data, labels
	* Scalar function
* $\eta$ = Natural parameters
* $b(y)$ = base measure
	* Scalar function
* $T(y)$ = sufficient statistics
	* Think of as capturing everything relevant to your data
	* Same dimension as $\eta$.
* $a(\eta)$ = partition function
	* Often called the "log partition function"
	* Does not depend on $y$
	* Picked to normalize the probability
	* Scalar function
## Example 1

Bernoulli random variable (e.g. a weighted coin flip). It represents a trial with exactly **two possible outcomes**:

- **1 (success)** with probability $p$
- **0 (failure)** with probability $1−p$
$$Var(X) = p(1-p)$$
...aka...
$$P(y_j ; \phi) = \phi^{y}(1 - \phi)^{1 - y}$$
Above: probability of y = 1 is $\phi^y$ and when y = 0 its $(1 - \phi)^{1 - y}$

Taking the above, we can show it in the form $P{(y;\eta)} = b(y)\ exp\{\eta^T T(y) - a(\eta)\}$:

$$\begin{aligned}
p(y;\phi) &= \phi^{y}(1-\phi)^{1-y} \\
&= \exp\!\big(y \log \phi + (1-y)\log(1-\phi)\big) \\
&= \exp\!\left(\left(\log \frac{\phi}{1-\phi}\right) y + \log(1-\phi)\right)
\end{aligned}$$
Where:
* $\eta = \log \frac{\phi}{1-\phi}$ 
* $T(y) = y$
* $a(\eta) = \log(1-\phi)$

Why does this matter?

> Given a probability distribution in one form (e.g. $P(y_j ; \phi) = \phi^{y}(1 - \phi)^{1 - y}$), I'm going to translate it into a same functional form such that it satisfies the **Exponential Family** conditions (i.e. $b(y)\ exp\{\eta^T T(y) - a(\eta)\}$). In that case, the $\eta$ becomes the "natural parameters". You're typically not given a function in terms of natural parameters...if you can do this mapping however, then a bunch of stuff becomes easy because now you can do gradient descent on these parameters and its going to be concave.

-Re

## Example 2

**Gaussian w/ fixed variance**

$$\begin{aligned}
p(y; \mu) &= \frac{1}{\sqrt{2\pi}} \exp\left( -\frac{1}{2} (y - \mu)^2 \right) \\
&= \frac{1}{\sqrt{2\pi}} \exp\left( -\frac{1}{2} y^2 \right) \cdot \exp\left( \mu y - \frac{1}{2} \mu^2 \right)
\end{aligned}
$$
Recalling our exponential family form as:
$$P{(y;\eta)} = b(y)\ exp\{\eta^T T(y) - a(\eta)\}$$
This would make:
* $\eta$ = $\mu$
* $T(y) = y$
* $a(\eta) = \frac{\mu^2}{2} = \frac{\eta^2}{2}$
* $b(y) = \frac{1}{\sqrt{2\pi}} \exp\left( -\frac{1}{2} y^2 \right)$

### Math rundown
To convert it from the first form ($p(y; \mu) = \frac{1}{\sqrt{2\pi}} \exp( -\frac{1}{2} (y - \mu)^2$) to the second ($\frac{1}{\sqrt{2\pi}} \exp\left( -\frac{1}{2} y^2 \right) \cdot \exp( \mu y - \frac{1}{2} \mu^2$), we had to do a couple steps:

1. We expanded the square in the exponent
$$(y - \mu)^2 = y^2 - 2y\mu + \mu^2$$
2. We distributed the $-\frac{1}{2}$:
$$-\frac{1}{2}(y^2 - 2y\mu + \mu^2) = -\frac{1}{2}y^2 + y\mu - \frac{1}{2}\mu^2$$
3. For transparency, we rearrange terms before putting the simplified exponent back into the exponential (we didn't have to, but it helps for visual):
$$-\frac{1}{2}y^2 + y\mu - \frac{1}{2}\mu^2 = -\frac{1}{2}y^2 + \mu y - \frac{1}{2}\mu^2$$
$$p(y; \mu) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}y^2 + \mu y - \frac{1}{2}\mu^2\right)$$
4. We split the exponential using $e^{a+b} = e^a * e^b$. This property works because when you multiply exponentials with the same base, you add the exponents.
$$p(y; \mu) = \frac{1}{\sqrt{2\pi}} \exp\left(-\frac{1}{2}y^2\right) \cdot \exp\left(\mu y - \frac{1}{2}\mu^2\right)$$
## Why do we care about this form?

1. Inference is easy
	1. $E[y:n] = \frac{\partial}{\partial\eta}*a(\eta)$
		1. ==What does this mean?==
	2. $var[y:n] = \frac{\partial^2}{\partial^2\eta}*a(\eta)$
		1. The variance is also the second derivative of $a(\eta)$
	3. The above is interesting because once you take your distribution - however wild it may be - by putting them into this form, you have a mechanical way to do inference (important), because that means we can do **learning**
2. Learning is "well defined" being concave to $\eta$
	1. MLE with regard to $\eta$ is concave.
# Generalized Linear Models

These are all design assumptions we can make in our models.

1. $y | x; \theta ~ Exponential Family (\eta)$
	1. The distribution of our labels - given $x$ for some parameters $\theta$ - follows Exponential Family
		1. Re asserts this is an important family; many different data types fall into this family
			1. Binary (y = 1,0) = Bernoulli
			2. Real values = Gaussian
			3. Counts (e.g. how many packets arrive at a server) = Poisson (sp?)
			4. Real positive line = Gamma expansion
			5. Dirichlet = Dirichlet
2. $\eta = \theta^Tx$ are of the following form: $\theta \in \mathbb{R}^{(d+1)}, x \in \mathbb{R}^{(d+1)}$
	1. Make the assumption that - subject to noise - our model varies linearly with some underlying feature
		1. If your model is large with huge number of features everything becomes linear in that space
		2. If you take your data and put it up into a huge dimensional space, odds are it will be linearly separable - a line through it
3. Inference is easy
	1. Output $E[y|x;\theta]$ i.e. $h_\theta(x) = E[y|x;\theta]$
		1. Inference is just returning the probability that $y$ has a particular value