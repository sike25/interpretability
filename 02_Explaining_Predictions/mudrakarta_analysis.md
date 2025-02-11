## Did The Model Understand The Question? 
**Mudrakarta et al, 2018**

This paper uses the concept of attribution (or word importance) as calculated via integrated gradients to create adversarial examples for three types of question answering tasks-- visual, tabular and passage comprehension.

### Introduction

Neural network models have been used to answer questions about images, passages of text and tabular data. Understanding more closely how they achieve this is useful. 
The standard way of evaluating model performance on a test set is imperfect. Because datasets are often too large for us to manually verify that how well they mirror reality. 
We must understand why models make their predictions.

Here is an illustrative example:

#### Specific Contributions

fill after rest of summary

### Related Work

This paper was deeply inspired by Jia and Liang (2017). But there are key differences.

1. Jia and Liang augment the test set with crowd-sourced adversarial examples to improve evaluation technique, but their augmentation is model-agnostic.      
Mudrakarta et al query the model for its particular weaknesses then use these weaknesses to craft targeted adversarial examples. Their goal is not to improve technique but to validate their analysis (or interpretation) technique.
Mudrakarta et al's attacks are also more effective (as one  might expect).

3. Jia and Liang focus only on reading comprehension tasks. Mudrakarta et al also focus on answering questions about visual and tabular data.

4. Mudrakarta et al's method (via Integrated gradients) is specific to deep-learning-based systems. Jia and Liang's method is not.

The Integrated gradient method was introduced by Sundararajan et al (2017). The authors state other methods would have sufficed but they use IG because it is easy and efficient to implement, and they like its theoretical justification.

There have also been adversarial work done on image-based systems as per Goodfellow et al (2015) where imperceptible image pertubation influences prediction (when obviously, it should not). 
This work is similar in spirit to this language task focused paper.

### Integrated Gradients (IG)

Sundararajan et al (2017) introduced the intepretability technique of Integrated Gradients. This paper uses it for attributing what parts of their input question is important for prediction. 

Here is an overview of the method:

The benefits of Integrated Gradients include:


And this is how the authors used it:

This work is also an empirical validation of the integrated gradient method which precedes it by only a year.


### Visual Question Answering

### Question Answering Over Tables

### Reading Comprehension



### Reflection

**What is this paper about?**       
Visualization of the importance of input image portions for classifications, achieved by a network with only convolutional layers and topped with a global average pooling layer.

**What are the strengths?**
1. Thorough literature review    
2. Thorough evaluation of technique   
3. Good visualizations, and tables    
4. Breaking new ground   

**What are the weaknesses?**      
1. The feed-forward layers on models have to be removed before this technique can be applied.   
2. Their description of CAM was difficult to follow.    
3. Fully supervised methods still outperform their weakly supervised one.    

**What are some significant follow up work from this paper? How do they differ from this paper?**    

Grad-CAM: Why did you say that?, 2016    
Introduces gradient-weighted CAMs for more transparency

Eigen-CAM: Class Activation Map using Principal Components, 2020    
Uses PCA to more simply and more intuitively calculate CAMs

Relevance-cam: Your model already knows where to look, 2021   
Works on using intermediate convolutional layers along with the final one
