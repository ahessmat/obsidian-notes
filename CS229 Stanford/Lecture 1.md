Some software problems are hard to programmatically derive; those same problems are made easier through applying a learning algorithm.

Ng's goals for the course:

1. Convey enthusiasm of the material
2. Student application to original problems
3. Become qualified to start doing research in ML

Ng's prereqs:

* Basic knowledge of Computer Science
	* Understand Big-O notation
	* Data structures like queues, stacks, binary trees
	* Ability to write a simple computer program
	* Utilize MATLAB
* Comprehension of probability and statistics
	* Variance & Random variables
* Linear Algebra
	* Matrices and vectors

# What is Machine Learning?

Defined by Arthur Samuel (1959). Machine Learning: field of study that gives computers the ability to learn without being explicitly programmed.

> Well-posed Learning Problem: a computer program is said to *learn* from experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improvs with experience E.

-Tom Mitchell

# Supervised Learning
We provide the algorithm a dataset that is appropriately labeled. The algorithm, in turn, learns the association between the inputs and the outputs to predict more correct answers.

## Regression Problems
A variable we're trying to predict is a along a **continuous** value (e.g. housing prices vs. square size).
## Classification Problems
Predicting a discrete [^1] value as opposed to a **continuous** [^2] one.
### Support Vector Machines (SVM)
Takes data and maps it to an infinite dimensional space, then performs classification using an infinite number of features.

==**I don't know what Ng means by this right now; we'll come back to that.**==

# Learning Theory
* Seek to prove a theorem to guarantee a learning algorithm will work
* Understand what algorithms can approximate different functions well
* How much training data is needed?
# Unsupervised Learning
Hand unstructured data to an algorithm to make sense of.

[^1]: A finite number of values (i.e. categories/buckets)
[^2]: A potentially infinite number of values

