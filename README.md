# The Interpretation of Neural Networks

### Introduction: Why care how?
Does it matter whether we understand what a deep network is doing or not, if the results look good? Isn't accuracy on the holdout set the ultimate goal?

### Explaining predictions.
What part of the input does a model look at? What can we learn from this?

### Explaining models.
How does a neural network decompose data internally? What can we learn from a model's representation?

### Adversaries and interpretability.
Why do some interpretability methods fail to uncover reasonable explanations? Could this be a consequence of how models actually make decisions? 
We will discuss recent findings suggesting that ML models rely on features that are imperceptible to humans. 
Then we will see how training models to be robust to imperceptible input changes can lead to models that rely on more human-aligned features.

### Bias and fairness.
Big data and deep learning may unintentionally amplify bias present in the dataset collection or model formulation process. 
How can we define "fairness" in a quantitative way? How can we audit a system for bias and potential for disparate impact?
How can we create equitable models?

### Interaction and collaboration.
How can interactive methods help us to formulate hypotheses about models and data? 
What can we learn about the structure of a model by posing counterfactual "what if" questions? How can humans collaborate with machine-learned models? 
We will describe some common methods to create interactive AI tools and will create two very small examples for interactions with a text and an image model.

### Complex explanations.
Sometimes representations and decisions in models are hard to communicate with examples and visualizations.
How can we use richer explanations (especially in language) to describe more complex behaviors?

### Other Topics
Kolmogorov-Arnold Networks and Neurosymbolic AI

First seven topics are drawn from 
[the 2020 MIT IBM practicum: Structure and Interpretation of Deep Networks](https://sidn.csail.mit.edu/) and [its accompanying Github](https://github.com/SIDN-IAP).
