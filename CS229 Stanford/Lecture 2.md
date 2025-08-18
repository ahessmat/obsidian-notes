Supervised Learning Setup, LMS

Starts at [2:00](https://youtu.be/gqKaVgQxEJ0?list=PLoROMvodv4rNyWOpJg_Yh4NSqI4Z4vOYy&t=120).

* Linear Regression
* Classification
* Generalized Exponential Family
# Notation

* $m$ = # of training examples (i.e. # of rows in table)
* $n$ = # of features
* $x$ = inputs/features
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
We can take the parameters $\theta$ and the features $x$ and represent them as ve

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





