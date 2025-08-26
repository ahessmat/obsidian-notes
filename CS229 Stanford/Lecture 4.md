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
## Examples

Bernoulli random variable (e.g. a weighted coin flip). It represents a trial with exactly **two possible outcomes**:

- **1 (success)** with probability $p$
- **0 (failure)** with probability $1−p$
$$Var(X) = p(1-p)$$
...aka...
$$P(y_j ; \phi) = \phi^{y}(1 - \phi)^{1 - y}$$
Above: probability of y = 1 is $\phi^y$ and when y = 0 its $(1 - \phi)^{1 - y}$

