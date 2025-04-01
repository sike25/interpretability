## Did The Model Understand The Question? 
**Mudrakarta et al, 2018**

This paper uses the concept of attribution (or word importance) as calculated via integrated gradients to create adversarial examples for three types of question answering tasks— visual, tabular and passage comprehension.

Paper: https://arxiv.org/pdf/1805.05492

### 1. Introduction

Neural network models have been used to answer questions about images, passages of text and tabular data. Understanding more closely how they achieve this is useful. 
The standard way of evaluating model performance on a test set is imperfect. Because datasets are often too large for us to manually verify that how well they mirror reality. 
We must understand why models make their predictions.

Here is an illustrative example:

![image](https://github.com/user-attachments/assets/46fa9e41-2d64-4f8f-bf14-97474a970d84)

When the word "symmetrical" was changed to "spherical", the prediction remained "very".

#### 1.1. Specific Contributions

fill after rest of summary

### 2. Related Work

This paper was deeply inspired by Jia and Liang (2017). But there are key differences.

1. Jia and Liang augment the test set with crowd-sourced adversarial examples to improve evaluation technique, but their augmentation is model-agnostic.      
Mudrakarta et al query the model for its particular weaknesses then use these weaknesses to craft targeted adversarial examples. Their goal is not to improve technique but to validate their analysis (or interpretation) technique.
Mudrakarta et al's attacks are also more effective (as one  might expect).

3. Jia and Liang focus only on reading comprehension tasks. Mudrakarta et al also focus on answering questions about visual and tabular data.

4. Mudrakarta et al's method (via Integrated gradients) is specific to deep-learning-based systems. Jia and Liang's method is not.

The Integrated gradient method was introduced by Sundararajan et al (2017). The authors state other methods would have sufficed but they use IG because it is easy and efficient to implement, and they like its theoretical justification.

There have also been adversarial work done on image-based systems as per Goodfellow et al (2015) where imperceptible image pertubation influences prediction (when obviously, it should not). 
This work is similar in spirit to this language task focused paper.

### 3. Integrated Gradients (IG)

Sundararajan et al (2017) introduced the intepretability technique of Integrated Gradients. This paper uses it for attributing what parts of their input question is important for prediction. 

Here is an overview of the method:

![image](https://github.com/user-attachments/assets/00b37894-e24b-40fb-ba5c-e1d15d206a96)

Model:  $$F: R^n \to [0,1]$$
Input:  $$x = (x_1 ... x_n) \in R^n$$
Output: $$F(x) \in [0,1]$$ the probability of a specific response.

The **baseline input** x is a practical zero vector, a blanking vector. For an image input x, x' could be a black picture of the 
same size. Here, x' is a sequence of padding values. The context of the problem (the image, table or passage depending on 
the task) has its baseline input unchanged.

The **attribution** of the model with respect to a particular input is $$A(x,x') = (a_1 ... a_n)$$ where $$a_i$$ is the contribution 
(the influence, the blame assignment) of $$x_i$$ to the prediction $$F(x)$$.

Attribution here is: **The Integrated Gradient:**

$$IG_i (x, x') = (x_i - x_i') \times \int_{\alpha = 0}^{1} \frac{\partial F(x' + \alpha \times (x-x')}{\partial x_i} d \alpha$$

$$x - x'$$ is how much x contributes relative to "nothing", the blanking vector.     
$$x' + \alpha(x - x')$$ makes up a series of gradual inputs from blank to the real image where $$\alpha \in [0,1]$$       
The gradient represents the rate of change or sensitivity of the function
F with respect to each input dimension.
We multiply the integral by $$(x_i - x_i')$$ to weight it.

The theoretical benefits of Integrated Gradients include:
1. Completeness: The sum of all our attribution values $$\sum A = a_1 + ... + a_n$$ is
   exactly equal to the difference between the model's predicted probablility for x and that
   for x '.
2. Influence: Uninfluential variables, if all else is fixed, do not change the
   model prediction. IG gives zero attribution scores to uninfluential variables. Meanwhile,
   influential variables are guaranteed to get attribution.
3. Linearity: Attributions for the linear combination of two models $$F_1, F_2$$ is the
   linear combination of their separate attributions.
4. Symmetry: Symmetric variables get equal attribution.

This work is also an empirical validation of the integrated gradient method whose publication precedes this paper by only a year.


### 4. Visual Question Answering

### Question Answering Over Tables

### Reading Comprehension



### Reflection

**What are the strengths?**
1. Novel and interesting concept   

**What are the weaknesses?**      
1. 
**What are some significant follow up work from this paper? How do they differ from this paper?**    
