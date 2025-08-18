Supervised Learning Setup, LMS

Starts at [2:00](https://youtu.be/gqKaVgQxEJ0?list=PLoROMvodv4rNyWOpJg_Yh4NSqI4Z4vOYy&t=120).

* Linear Regression
* Classification
* Generalized Exponential Family
# Notation

* $m$ = # of training examples (i.e. # of rows in table)
* $n$ = # of features, sometimes referred to as $d$
* $x$ = input/feature
	* $x \in \mathbb{R}^d$  for large $d$
		* $d$ = the total number of features (all $x$)
		* Any given feature ($x$) is an element of $d$-dimensional real space (i.e. all possible vectors with $d$ real number components).
	* There might be many features (theoretically infinite) and not all of them may be useful.
		* Because we might not know which features are important, we may find theoretically infinite useful.
			* This does not mean our algorithm at runtime need be infinite (ref [43:35](https://youtu.be/Bl4Feh_Mjvo?list=PLoROMvodv4rNyWOpJg_Yh4NSqI4Z4vOYy&t=2615))
* $y$ = output (aka target variable, labels, or supervisions)
* (x,y) = a single training example
* $(x^{(i)}, y^{(i)})$ = The ith training example; the (i) reflects the index only. Can go up to m.

Given:
![[Pasted image 20250812000131.png]]

Then $x_1^{(1)}$ would be 2104, as it is the first indexed row of our first feature (sq. feet).
# Supervised Learning

Prediction model mapping some input $x$ to an output $y$.
* images -> cat?
* text -> hate speech?
* House data -> price?

Requires a **training set**: 
$$\{(x^{(1)}, y^{(1)}),(x^{(2)}, y^{(2)}),...,(x^{(n)}, y^{(n)})\}$$
Given the above, find a ==good== $h$ (hypothesis) that predicts $x$ -> $y$.

==What do we classify as good?==

Supervised Machine Learning is NOT interested in interpolation; it's not predicting back on just the ${(x,y)}$ pairs that we have in the **training set**. We want to leverage what was learned in the **training set** to **generalize** against *new* inputs $x$.

## How do we represent our hypothesis?
$$h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n$$
* $\theta_i$: "Theta", which will serve as a kind of weigh (aka "parameter") for various **features** ($x$) I've identified.
	* Features may be things like sqr ft. of a home, # of bedrooms, etc.
		* $x_1^{(1)}$: the size of the home in the first training set example
		* $x_2^{(1)}$: the number of bedrooms in the first training set example
		* ...etc.
* The number of $\theta x$ pairs makes this a **high dimension problem**.
	* We can take the data, embed it, and then train linear model on top ([apparently state-of-the-art?](https://youtu.be/gqKaVgQxEJ0?list=PLoROMvodv4rNyWOpJg_Yh4NSqI4Z4vOYy&t=1299) )

### Vector Notation
We can take the parameters $\theta$ and the features $x$ and represent them as vectors like so:

$$
\theta = 
\begin{bmatrix}
\theta_0 \\
\theta_1 \\
\theta_2 \\
\vdots \\
\theta_d
\end{bmatrix},
\quad
x^{(i)} =
\begin{bmatrix}
x^{(i)}_0 \\
x^{(i)}_1 \\
\vdots \\
x^{(i)}_d
\end{bmatrix}
$$
Now we can make a matrix $X$ with one row $x$ for every training example:

$$
X =
\begin{bmatrix}
(x^{(1)}) \\
(x^{(2)}) \\
\vdots \\
(x^{(n)})
\end{bmatrix}
\in \mathbb{R}^{\,n \times (d+1)}
$$

This matrix rests in real-space of $n$ rows against $d + 1$ dimensions (with the +1 accounting for $\theta_0x_0$ which evaluates to 1). This lets us look at our training examples as a matrix.

## How do I find a good line?
We want to minimize the error between our prediction ($h(x)$) and the real price ($y$). For computational/historical reasons, we will *square* of those residuals as:
$$(h_\theta(x) - y)^2$$
From this, we can derive the **cost function** $J(\theta)$ as the sum of "least squares":
$$
J(\theta) = \frac{1}{2} \sum_{i=1}^{n} \left( h_{\theta}(x^{(i)}) - y^{(i)} \right)^2
$$
## Gradient Descent
The gradient descent algorithm can be denoted as:

$$\theta_j := \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)$$
* In the notation above the `:=` value denotes performing the function on the right-side and assigning it to the value on the left-side.
* $\theta_j$ refers to an indexed feature (i.e. feature 0, feature 1, feature 2, ... , feature n)
* $\alpha$ is the learning rate
	* What should this be set at?
	* Ng suggests a value between -1 and 1. In practice, he starts at 0.01 and then increases/decreases.
	* Large values risk over-shooting the minima; small values require more iterations before arriving at minima.
	* If the cost function ($J(\theta)$) is increasing over iterations, that's an indicator that our $\alpha$ is too large
* $\frac{\partial}{\partial \theta_j}$ is a partial derivative[^1] of the cost function ($J(\theta)$)

The above can be described as a search algorithm that starts at some "initial guess" for $\theta$, then repeatedly making changes to $\theta$ to make the cost function ($J(\theta)$) smaller until it it's minimized. 

Here's the intuition between figuring out the partial derivative with one training example (vs. all $m$ training examples):

1. Recall that we already defined what $J(\theta)$ cost function was earlier in our **Linear Regression** overview. Because this is just 1 training example (vs. all) we can drop the $\sum_{i=1}^{n}$ . This becomes a basic substitution:
$$\frac{\partial}{\partial \theta_j} J(\theta) = \frac{\partial}{\partial \theta_j} \frac{1}{2}(h_\theta(x) - y)^2$$
2. From calculus, if you take the derivative of a square, the exponent comes down as a constant; then by the chain rule of derivatives we take the derivative of what's inside the square and multiply it out:
$$2 \frac{1}{2}(h_\theta(x) - y) * \frac{2}{2\theta_j}(h_\theta(x) - y)$$
3. Simplify and substitute definition of $h_\theta(x)$ (defined earlier at start of **Linear Regression**):
$$(h_\theta(x) - y) * \frac{2}{2\theta_j}(\theta_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n - y)$$
4. Observing that the partial derivative for all the $\theta$ values with respect to $\theta_j$ will be zero (0) EXCEPT for the term represented by `j`, then the partial derivative of that term will be just $x_j$ (because no other theta is determined by $\theta_j$ other than that). That makes the whole right side sum simply:
$$(h_\theta(x) - y) * x_j$$
5. Plugging the above back into our **gradient descent** algorithm, we then get:
$$\theta_j := \theta_j - \alpha (h_\theta(x) - y) * x_j$$
6. Multiplying the negative sign across the gradient descent update step and summing across *all* $m$ training examples gets us:
$$\theta_j := \theta_j + \alpha \sum_{i=1}^{n} (y^{(i)} - h_\theta(x^{(i)})) x_j^{(i)}$$
This last result is what's known as **batch gradient descent**.
### Batch vs. Stochastic Minibatch
Summing over all features can be computationally expensive for even a single **learning step** $\alpha$. Modern models may be trained on millions or billions of data points. 