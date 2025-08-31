* Generative Learning Algorithms
	* Gaussian discriminant analysis (GDA)
	* Naive Bayes

In contrast to **discriminative learning algorithms**, which model/parameterize the conditional relationship $p(y|x)$ (e.g. $y|x:\theta = ExponentialFamily(\eta)\ \ \eta=\theta^Tx$).
# Generative Learning Algorithms
**Generative Learning Algorithms** model the joint distribution $p(x,y)$ by modeling $p(x|y)$ and $p(y)$ such that:
$$p(x,y) = p(x|y)p(y)$$
* x = input
* y = label/class
* p(y) = How likely a given class is before you condition on the actual data (x).

We can apply the Bayes rule to compute $p(y|x)$ as:
$$p(y|x) = \frac{p(x|y)p(y)}{p(x)}$$
## Math

The **product rule of probability** states:
$$p(a|b) = \frac{p(a,b)}{p(b)}$$
> The joint probability of $a$ and $b$ can be written as "probability of one given the other multiplied by the probability of the other".

Put another way:
$$p(x,y) = p(x|y)p(y)$$
and
$$p(x,y) = p(y|x)p(x)$$
Ergo
$$p(x|y)p(y) = p(y|x)p(x)$$
Dividing both sides by $p(x)$ gets us the result for $p(y|x)$.

## Continuing

Consider 2 instantiations of this:

* Continuous inputs $x$ (GDA)
* Discrete inputs $x$ (Spam filtering)

## GDA
Assumes $p(x|y)$ is a multivariate **Gaussian Distribution**.

> A **Gaussian distribution** (also called a **normal distribution**) is a continuous probability distribution that has the famous **bell curve** shape. See **Lecture 3: "Gaussian/Normal Distribution**

