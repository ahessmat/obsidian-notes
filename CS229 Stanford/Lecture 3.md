* Classification & Regression
	* Probabilistic view of linear regression
	* Classification
		* "Why not linear regression"?
		* Logistic Regression
		* Method: Newton's Method


# From Lecture 2
 RECALL:
 
 Least Squares

**Given**:  $\{(x^{(i)},y^{(i)})\ for\ i=1...n\}$        in which $x^{(i)}\in \mathbb{R}^{(d+1)}$, $y^{(i)}\in \mathbb{R}$
* Recall this is the training set
* $x^{(i)}$ lives in some realspace of d+1 (recalling the convention there's a bias convention to append 1)
* target value $y^{(i)}$
**Do**:   find $\theta\in \mathbb{R}^{(d+1)}$ such that  $\theta = argmin \sum_{i=1}^{n} \left(y^{(i)} - h_{\theta}(x^{(i)}) \right)^2$  where $h_\theta(x) = O^Tx$
* $\theta$ minimizes the losses of the residuals (squared)
* ==Why did we pick to minimize the sum of squares?==

## New Concept

Assume $y^{(i)} = \theta^Tx^{(i)} + \epsilon^{(i)}$
* $\theta^Tx^{(i)}$ : given some **unknown theta** we could perfectly predict all input values.
	* This would work if all data lay perfectly on a line (noiseless situation).
* $\epsilon^{(i)}$: error/noise term
	* We expect there's some random gyration, measurement error, some unmodeled piece.
	* This is something we don't get to see this; it's just a mental model for how data are linked together.
	* Properties:
		* Different per tuple; each point has random per sample
		* It's unbiased; on avg we don't care about this value as its average will equate to 0.
			* Any given sample may have non-zero value, but taken all the data points as a whole it will.
		* Errors are statistically independent
			* Knowing the error for one tuple doesn't mean anything for another
		* No single area of noise is any more "noisy" than any other (lest it could be accounted for in the model)
# Gaussian/Normal Distribution
![[Pasted image 20250820222713.png]]


