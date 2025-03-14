## Explaining and Harnessing Adversarial Examples

**Goodfellow et al, 2015**

This paper argues that the linearity of neural networks gives them a tendency to misclassify linearly perturbed examples (i.e. makes them vulnerable to adversarial examples). It also draws on this hypothesis to present a quick, easy way to generate adversarial examples. It then shows that training on adversarial examples is a form of regularization.

Paper: https://arxiv.org/pdf/1412.6572

### Introduction

Neural networks are vulnerable to adversarial examples. They correctly classify some inputs but when the inputs are only slightly perturbed the networks misclassify them. Several models trained on separate subsets of a dataset even misclassify the same adversarial example.

Researchers speculate this vulnerability is a weakness of generalization, or extreme non-linearity. This paper posits that the vulnerability is a weakness of linearity. 

Then they use this claim to design a fast, simple method of generating adversarial examples. They also show that adversarial training is important for regularization.


### Related Work

Szegedy et al (2014b) was the most relevant. it found that:
1. Even perturbations imperceptible to the human eye were adversarial for models.
2. The same adversarial examples were misclassified across models with different architectures, trained on different subsets of the same data.
3. Shallow regression models are also vulnerable to adversarial methods.
4. Training on adversarial examples is a form of regularization.
5. Adversarial examples were generated using the expensive L-BFGS method.

### The Linear Explanation of Adversarial Examples

Let us start by explaining aversarial examples in linear classifiers, for example, the softmax regression models in Szegedy 2014.

Real-life data representation has limited precision. For example, an image pixel is 8 bits so can only capture 256 (or $$2^8$$) different values. So 100.2 and 100.4 both equate to 100. Any variation less than $$\frac{1}{255}$$ is discarded.

It is then reasonable to expect that a model should not be ble to distinguish between an input $$x$$ and its adverserial input $$\overline{x} = x + n$$, when all the elements of the perturbation $$n$$ are less than $$e$$ where $$e < \frac{1}{255}$$ (small enough to be discarded).

Why is this not the case? Consider the dot product of an input and a weight vector:

$$w^T \cdot \overline{x} = w^T \cdot x + w^T \cdot n$$

To maximize the effect the pertubation $$w^T \cdot n$$ as on the activation $$w^T \cdot \overline{x}$$ while keeping each element in $$n$$ small, we can set $$n = e \times sign(w)$$.  Note: I validated this empirically by comparing the effect of $$n = e \times sign(w)$$ with the effect of $$n =$$ random vector.

If $$w$$ has dimension $$N$$ and average maginitude of elements $$M$$, then the activation grows by $$eNM$$, which means the perturbation can get quite large for high-dimensional problems. Many infinitesimal changes to the input adds up to one large change in the output.

Extending this linear explanation to neural networks is reasonable and simpler than previous explantions.

### Linear Perturbation of Non-Linear Models

Considering that neural networks like LSTMs, ReLUs, and maxout networks are deliberately designed to act linearly, the authors expect them to be vulnerable to the kind of perturbations described above.

Let us have:     
$$\theta=$$ parameters of the model.   
$$x=$$ input to the model.    
$$y=$$ ground truth.        
$$J(\theta , x, y)$$ is the loss function.

To optimize the pertubation, we use $$n= e \cdot sign(\nabla_x J(\theta , x, y))$$ called the **fast gradient sign method**. The gradient of the loss function (wrt to the input) is calculated efficiently with back-propagation.

**Experimental Results**   

With $$e=0.25$$ on MNIST, on a softmax classifier, error rate of 99.9% was achieved (confidence = 79.3%). On a maxout network, error rate of 89.4% was achieved (confidence = 97.6%).

With $$e=0.1$$ on CIFAR-10, error rate of 87.15% was achieved.

![image](https://github.com/user-attachments/assets/835c5c7c-1ae3-47c4-9a39-6671cd619060)

Rotating the input in the direction of the loss gradient also worked well to create adversarial examples. (don't really get this).

### Adversarial Training of Linear Models Versus Weight Decay

The authors compare adversarial taining to L1 regularization (weight decay or lasso regularization) on a logistic regression model. The goal is to build intuition for why adversarial training would regularize neural networks.

Setting up:    
Labels: $$y \in$$ {-1,1}          
Linear prediction: $$z = w^T \cdot x + b$$    
Probability of class 1: $$P(y=1) = \sigma (z) = \sigma (w^T \cdot x + b)$$     
Loss function (softplus): $$l(d) = log(1 + exp(d))$$
Regularization penalty: p

Weight decay seeks to minimize the expected loss: $$E_{\forall(x,y)} l(-y(z)) + \lambda p$$ where $$p$$ is the sum of the weights.

The adversarial version of the expected loss is $$E_{\forall(x,y)} l(y(p - z))$$ where $$p = e \cdot w^T \cdot sign(w)$$.

In the adversarial version, the penalty is subtracted from the model's activation during training, rather than added to training loss. This means that when the model gets really confident, z gets very large and the penalty's effect shrinks. So while weight decay fails to deactivate in the case of more robust predictions, adversarial regularization incentivizes the model to "overcome" the penalty.

The pessimism of lasso regularizations becomes worse with deeper networks and multiclass classification. On maxout neural networks even a $$\lambda$$ of 0.0025 was too pessimistic (stalling the network at a 5% training error). Smaller coefficients let the networks train but failed to regularize it,

Meanwhile using adversarial regularization with e = 0.25 on maxout networks and MNIST gave good results.

### Adversarial Training of Deep Networks

**Apeal To Universal Approximation**

The criticism that deep networks are inherently vulnerable to perturbation is misguided. Theoretically, the universal approximation theorem (Hornik et al, 1989) guarantees that neural networks can estimate any function. 

This includes functions that are resistant to adversarial examples. Even if that means that to get a function that resists adversarial examples, we must encode this particular objective in our training of the neural network.

**Adversarial Training as Regularization**

We should think of adversarial training as different from data augmentation because augmentation techniques are about transforming inputs in ways that could be naturally occuring (like slight rotation of images). Adversarial examples are unlikely to occur naturally, and are carefully crafted to exploit the way the model does math.

Szegedy (2014b) validates adversarial training as a regularization technique, although it is not shown to outperform drop-out as the state-of-the-art regularization. This is partly because, the authors argue, the cost of generating adversarial examples via L-BFGS limited experimentation.

**New Experiments**

The authors of this paper make Szegedy's second attempt at validation. They implement an adversarial loss function based on the fast gradient method described in **Linear Perturbation of Non-Linear Models**:

$$\overline{J}(\theta, x, y) = \alpha J(,\theta x, y) + (1-\alpha)J(\theta, x + sign(\nabla_x J(\theta, x, y))
$$ with $$\alpha = 0.5$$ in all experiments.

When applied to a maxout net which was also regularized with dropout, the error rate dropped from 0.94% to 0.84%. Increasing the model size (from 240 to 1600 units per layer) and modifying early stopping criterion further dropped the error rate to 0.782%. Although this does not outperform drop-outs.

More specifiaclly, the error rate on the purely adversarial test set dropped from 89.4% to 17.9%. 

### Different Kinds of Model Capacity

### Why Do Adversarial Examples Generalize?

### Alternative Hypothesis


### Summary and Discussion



---

### Reflection      

**What are the strengths?** 

This is a very fundamental and theoretical paper, which I enjoyed reading. In some ways, they are validating neural networks which, ten years later, feels classic.

**What are the weaknesses?**      

1. Literature review is very, very skinny. They simply go into one of their previous papers, and only briefly mention some other two.

2. The experimental results from linearly perturbing non-linear models are impressive and cover >1  architecture and datasets, but seem cherry-picked as no data tables are presented or explanations on the choice of e. Just sentence summaries of the best results.

3. The general claim here is broader than what their evidence supports. 

This paper only demonstrates that FGSM adversarial examples work on neural networks. It does not unequivocally show that they exploit the linearity of the model, although given the linearity of the FGSM method, it is a reasonable assumption. They could have tried the FGSM method on a less linear network (like the sigmoid network) as a comparison.

Finally, it certainly does not show that most/all other adversarial examples work as a result of neural network linearity.


**What are some significant follow up work from this paper? How do they differ from this paper?**    

Projected gradient descent
