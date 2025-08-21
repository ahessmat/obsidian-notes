* Classification & Regression
	* Probabilistic view of linear regression
	* Classification
		* "Why not linear regression"?
		* Logistic Regression
		* Method: Newton's Method


Least Squares

**Given**:  $\{(x^{(i)},y^{(i)})\ for\ i=1...n\}$        in which $x^{(i)}\in \mathbb{R}^{(d+1)}$, $y^{(i)}\in \mathbb{R}$
* Recall this is the training set
* $x^{(i)}$ lives in some realspace of d+1 (recalling the convention there's a bias convention to append 1)
* target value $y^{(i)}$
**Do**:   find $\theta\in \mathbb{R}^{(d+1)}$ such that  $\theta = argmin \sum_{i=1}^{n} \left(y^{(i)} - h_{\theta}(x^{(i)}) \right)^2$  where $h_\theta(x) = O^Tx$
* $\theta$ minimizes the losses of the residuals (squared)
* ==Why did we pick to minimize the sum of squares?==


