# Regularization Methods

## Introduction
The main problem we were facing here is overfitting and methods to avoid that.
The definition of Reguralization is the following: 

Define Regularization as “any modification we make to a learning algorithm that is intended to reduce its generalization error but not its training error.”

We talked about several Reguralization technics:

## Parameter Norm Penalties
The main idea here is to limit the  model complexity by adding a parameter norm penalty, denoted as Ω(θ), to the objective function J:
$ \tilde{J}(\theta;X, y) = J(\theta;X, y) + \alpha\Omega(\theta)$. 

Most importantly, parameter θ represents the weights only and not the biases.

## $L_1$, $L_2$ Regularization

$L_1$ (a.k.a Least Absolute Shrinkage and Selection Operation - LASSO) and $L_2$ (a.k.a Ridge, Tikonohov, Weight Decay) Regularizations update the general cost function $J$ by adding a term known as the regularization term $\alpha$. 
Because of $\alpha$ the values of weights decrease as it assumes that a neural network with smaller weights leads to non complex models. 
Therefore, it will also help in reducing overfitting in an efficient manner.

In $L_2$, we have:

$ \tilde{J}(w;X, y) = \frac{\alpha}{2}w^Tw + J(w;X,y) $

Here, alpha serve as the regularization parameter.  It is known as hyperparameter, whoand it's value is optimized for efficient results. $L_2$ regularization is also known as weight decay as it decrease the weights towards zero but not exactly at 0.

In $L_1$, we have:

$ \tilde{J}(w;X, y) = \alpha \| w\|_1 + J(w;X,y)$

Unlike $L_2$, We can reduce the weight to eactly 0 here.


## Data Augmentation

Data Augmentation solve the problem of processing Limited Data with less diversity in order to get efficient results from the neural network.
Let us think of Data Augmentation as an added noise to our dataset. The idea here is to add new training examples when the data supply is limited.

Following are the most popular Data Augmentation Techniques:

1. Flip
2. Rotation
3. Scale
4. Crop

## Noise Robustness

We can impose a penalty on the norm of the weights by adding a noise with extremely small variance. Similarly, we can also add noise to weights. 
There are some interpretations about it. Among several, one interpretation is that when we add noise to weights, it will be considered as the 
stochastic implementation of Bayesian inference over the weights. In this case, the weights are not known. Probability distribution is used to model the uncertainity.
As learning will become stable and efficient, so it is considered as a type of Regularization. 

For example, consider a linear regression case, in order to reduce mean square error, for each vector x, we can learn to map y(x). 

$J = E_{p(x,y)}{[}(\hat{y}{(x)}{-}y)^2{]} $

If we add Gaussion random noise (ϵ) with zero mean to the weights. In such case, we are still interested to learn to reduce the mean square by proper mapping. 

Including noise in the weights is simply like adding regularization Ω(θ). Due to which, weights are not really affected due to small perturbations in the weights. This helps in stabilising training. 

## Early Stopping

Consider overfitting as decrease in training error and an increase in validation error as soon as we have a model complexity with high representation. 

So, in such scenarion, the best thing to do is to get back to previous point, where we have a least validation error. In order to check for improvement, with each epoch, we have to keep track of validation metrics and keep saving the parameter configuration. When the training
ends, the parameter which was saved at the end is returned.

Similar is the case with Early stopping. With some known or fixed iterations, when there is no improvement in the validation error, then we try to terminate or finish the algorithm.
The capacity or complexity of the model is efficiently reduced with the number of reduced steps to fit the model. 

One of its comparison with weight decay is that in weight decay, we had to work with the coefficient of weight decay manually by tweaking it. Often with 
wrong chosen coefficient of weight decay, we can lead to local minima by suppressing the values of the weights very much.

In Early stopping, we don't need such manual setting of coefficient of weights for tuninig. Early stopping is considered equivalent to L^2 Regularization. 


## Parameter Tying/Share and Dropout
Another way of implementing prior knowledge into the training process. 

### Parameter Tying
Express that certain parameters should be close to each other taken from two different models $A$ and $B$. 

Changing their loss functions with the additive Regularization term: $\Omega(w^{(A)}, w^{(B)}) := \vert\vert w^{(A)}-w^{(B)} \vert\vert_2^2$

### Parameter Sharing
Here we force sets of parameters in one (or multiple models) to be equal. Used for examples Heavy in CNN training, where the feature detectors of one layer get set on the same parameter set. 

The Advantage here is the massive reduction of space (brings the ability to train larger models) and another implement of prior knowledge.

### Dropout
Here we improve generalization and speed up the model training by training the ensemble of all sub-networks of a NN. 

Where a subnet is a subgraph of the original NN connecting (at least some) input neurons to the output. 
When training a subnet its weights are shared with the original net. 


Since there are too many subnets to train we sample a subnet by selecting each non ouput neuron $n$ in layer $l$ with probability $p_l$ 
($p_l$ for the input layer is usually high with $0.8$ and $0.5$ for the other layers).

Training routine is then: Sampling subnet, Training subnet, adjust wheigts in original net and iterate.

Once training is done one can predict new data points by usual forward propagation but with each weight $w$ in layer $l$ multiplied by $p_l$.

In conclusion Dropout provides a cheap Regularization method (implementable in $\mathcal{O}(n)$), 
which forces the model to generalize since it is trained with subnets which have various topologies and a smaller capacity.



## Adverserial Training
Motivation: There a many models capable of reaching (or even exceeding) 
human performance on specific tasks (as Chess or GO). But do they also gather a human understandment of the game?

No they don't. That can be seen very well on object recognition tasks. 
Here often a little noise applied to the input image (barely distinguishable by a human), leads to a huge change in prediction. 
Those examples are called Adversarial examples. 


By training a model on many Adversarial examples one tries to force the model to implement a prediction stable plateau around each of the training data points, 
because prior knowledge tells us that two different object in an image have a larger distance regarding the pixel values than a little noise could inject.



## Questions

Q: Can you give an example where dataset augmentation is not appropriate? <br />
A: It is not appropriate when the transformation would change the correct class. You can see this e.g. on slide 7. If we rotate a six 180°, then it would be impossible to distinguish the digits 6 and 9.

Q: How do I decide how many supervised and unsupervised examples do I take to train a semi-supervised model? <br />
A: This question cannot be answered in general, it depends on the model. You have to try different proportions of supervised and unsupervised examples and take a proportion that yields good results.

Q: Is it better to use the geometric or arithmetic mean when we do inference? <br />
A: We mainly use the geometric mean. It usually improves the performance.

Q: What is the difference between dropout training and bagging. <br />
A: They are similar in many ways. We infer from an ensemble of models. Some differences: In the case of bagging, all models are independent and is trained till convergence. In the case of dropout only a small fraction of the subnetworks are trained.
