Starting at [11:42](https://youtu.be/KzH1ovd4Ots?list=PLoROMvodv4rNH7qL6-efu_q2_bPuy0adh&t=702), after preamble about admin/logistics of the class.

**Machine Learning** coined by Arthur Samuel, who made a program that could beat him in a game of checkers (authored paper in 1959); the program learned by playing against itself.

**Artificial Intelligence (AI)** is a broader term; Machine Learning is a subset of AI.
* AI is about building algorithms/programs that's able to perform at least at the level of a human
	* ML is an approach to this.
		* It looks at past data (or a simulator) and then in looking at that data forms experiences to improve its performance.
		* Recent advances in ML has been what has elevated AI/ML as a discipline.
		* Deep Learning is a subset of ML

Machine Learning is typically a mix of:
* Supervised Learning
* Unsupervised Learning
* Reinforcement Learning
# Notation

* $m$ = # of training examples (i.e. # of rows in table)
* $n$ = # of features
* $x$ = inputs/features
	* $x \in \mathbb{R}^d$  for large $d$
		* $d$ = the total number of features (all $x$)
		* Any given feature ($x$) is an element of $d$-dimensional real space (i.e. all possible vectors with $d$ real number components).
	* There might be many features (theoretically infinite) and not all of them may be useful.
* $y$ = output (aka target variable, labels, or supervisions)
* (x,y) = a single training example
* $(x^{(i)}, y^{(i)})$ = The ith training example; the (i) reflects the index only. Can go up to m.

Given:
![[Pasted image 20250812000131.png]]

Then $x_1^{(1)}$ would be 2104, as it is the first indexed row of our first feature (sq. feet).

# Supervised: Regression v. Classification

There's typically two types of supervised problems depending on the labels we're working with.

* Regression = when labels are real, continuous (i.e. given context, predict value)
* Classification = when labels are discrete, bucketed (i.e. given context, predict category)

## Broad Applications
* Image Classification
	* x = pixels
	* y = class
	* ImageNet: the dataset that has driven deep learning image recognition
* Object Localization and detection in Computer Vision
	* x = pixels
	* y = bounding box
* Machine translation
	* x = English
	* y = Translation
		* More complicated, since the number of y grows massively.
# Unsupervised: 