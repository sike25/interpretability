## Explaining and Harnessing Adversarial Examples

**Goodfellow et al, 2015**

This paper argues that the linearity of neural networks is the main cause of their tendency to misclassify perturbed examples (i.e. vulnerability to adversarial examples) using quantitative results. It also presents a quick, easy way to generate adversarial examples.

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

### The Linear Explanation of Adversarial Examples

Let us start by explaining aversarial examples in linear classifiers, for example, the softmax regression models in Szegedy 2014.

Real-life data representation has limited precision. For example, an image pixel is 8 bits so can only capture 256 (or 2^8) different values. So 100.2 and 100.4 both equate to 100. Any variation less than $$\frac{1}{255}$$ is discarded.

It is then reasonable to expect that a model should not be ble to distinguish between an input $$x$$ and its adverserial input $$\overline{x} = x + n$$, when all the elements of the perturbation $$n$$ are less than $$e$$ where $$e < \frac{1}{255}$$ (small enough to be discarded).

Why is this not the case? Consider the dot product of an input and a weight vector:

$$w^T \cdot \overline{x} = w^T \cdot x + w^T \cdot n$$

To maximize the effect the pertubation $$w^T \cdot n$$ as on the activation $$w^T \cdot \overline{x}$$ while keeping each element in $$n$$ small, we can set $$n = sign(w)$$.  Note: I validated this empirically by comparing the effect of $$n = e x sign(w)$$ with the effect of $$n =$$ random vector.

If $$w$$ has dimension $$N$$ and average maginitude of elements $$M$$, then the activation grows by $$eNM$$, which means the perturbation can get quite large for high-dimensional problems. Many infinitesimal changes to the input adds up to one large change in the output.

Extending this linear explanation to neural networks is reasonable and simpler than previous explantions.

### Linear Perturbation of Non-Linear Models

### Adversarial Training of Linear Models Versus Weight Decay

### Adversarial Training of Deep Networks

### Different Kinds of Model Capacity

### Why Do Adversarial Examples Generalize?

### Alternative Hypothesis

### Summary and Discussion






---

### Reflection      

**What are the strengths?** 

**What are the weaknesses?**      

Literature review is very, very skinny. They simple go into one of their previous papers, and only briefly mention some other two.

**What are some significant follow up work from this paper? How do they differ from this paper?**    
