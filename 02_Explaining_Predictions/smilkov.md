## SmoothGrad: Removing Nise by Adding Noise
**Smilkov et al, 2017**

Paper: [https://arxiv.org/pdf/1706.03825](https://arxiv.org/pdf/1706.03825)             
Code:  [https://pair-code.github.io/saliency/#smoothgrad](https://arxiv.org/pdf/1706.03825)

### 1. Introduction

Interpretability of deep neural networks matter for trust, debugging and insight. Saliency maps (sensitivity maps, pixel attribution) is a common way of explaining visual models.  

They can be occlusion or gradient-based and they highlight the parts of input images that are important to prediction. In practise, the resulting masks do tend to highlight portions od images that we would reasonably think should be important for a prediction.

![image](https://github.com/user-attachments/assets/e44ebcbd-f9df-4b50-a4d4-ff193480ae97)

Although, the gradient-based ones are often visually noisy and also highlight parts of images that a person would not expect to be relevant to prediction. Whether this phenomenon truly is noise or an expression of the actual working of the model is unclear.

All that said, Smoothgrad is a method for clarifying gradient based saliency maps. It works by adding small amounts of noises to the input image many times, computing the gradient map for each noisy input and finding their average to produce a final saliency map.

Smoothgrad can be combined with other algorithms. They also find that adding noise at training time further improves clarity of saliency maps.

### 2. Gradients as Sensitivity Maps

Consider an image classifier that classifies an input image $$x$$ into classes in the set $$C$$. The classifier computes an activation $$S_c$$ for each class $$c \in C$$. The final class prediction is the class with the highest activation:

$$class(x) = argmax_{c \ in C} S_c(x)$$

One can then build a sensitivity map, $$M_c(x)$$ as the gradient of the predicted probabilities with respect to the input image $$x$$:

$$M_c(x) = \partial S_c(x) / \partial x$$ 

Intuitively, $$M_c(x)$$ is how much a change to $$x$$ would influence $$S_c(x)$$.

#### 2.1. Previous Work on Enhancing Sensitivity Maps

Using gradients as a measure of importance  can create a situation where a very important feature pushed the activation very close to 0 or 1 (saturates it), and the gradient becomes very small even though the feature is critical for the prediction.

This is why methods like Integrated Gradients integrate along a path from a baseline to the full image- to capture the feature's relevance across the entire range (not just at the end which can be prone to saturation). Other methods are: Layerwise Relevance Propagation and DeepLift.

Another method of enhancing saliency maps is by modifying backpropagation. Examples include: deconvolution (which ignores the "gating" behavior of ReLU during backpropagation and passes gradients through whether or not they were activated in the forward pass) and guided backpropagation.

Other methods like Grad-CAM and CAM (see zhou.md in this folder) combine gradient information across layers to create visualizations.


#### 2.2. Smoothing Noisy Gradients




###  Experiments

**Visualiziation Techniques**
1. Absolute value of gradients      
2. Capping outlying values      
3. Multiplying maps with the input images     

**Hyperparamterization**         
The two hyperparameters in this are the sample size and the noise level or standard devation input to our Gaussian random variable.

Noise is optimal at around 10 to 20%. How is SD converted to noise percentages?

Clarity improves with sample size but diminishing returns are observed as the sample size surpasses 50. 

**Comparison to Baseline Methods**      

SmoothGrad is compared qualitatively with three gradient methods: vanilla, Integrated Gradients (Sundararajan et al., 2017), and Guided BackProp (Springenberg et al., 2014).
The authors claim to observe SmoothGrad outperforms the rest in visual coherence. Guided BackProp produces the sharpest images but struggles with uniform backgrounds while SmoothGrad excels with them. 
SmoothGrad also outperforms the rest on discriminativity with BackProp performing the worst.

**Adding Noise during Training**

Apart from averaging several saliency maps from several noisy inputs, 

### Conclusion and Future Work

More theoretical inquiry into the idea that noisy gradients cause noisy maps.

Other ways to smooth noisy maps.

Quantitative assessments for assessing saliency maps. 

### Reflection 

**What is this paper about?**    

Smoothgrad is a method for sharpening gradient based saliency maps. 
It relies on the hypothesis that the noisiness usually observed in gradiency based activation maps are as a result of the fact that gradients change rapidly at small scales. 
It works by adding small amounts of noises to the input image many times, computing the gradient map for each noisy input and finding their average to produce a final saliency map.
Just as the name suggests, this helps to ***smooth*** out the sharp spikes in gradient from one pixel to another. 

**What are the strengths?**

1. Simple and clearly explained methods.     
2. Hypothesizes about the cause of noise (sharp gradient changes) in saliency maps and then creates this method in response.         
3. Experiments carried out on classic benchmark datasets.        
4. Open source code.             

**What are the weaknesses?**      

1. Sparse explanation of prior work done on enhancing saliency maps.      
2. Comparison to baseline models is qualitative.
3. Fails to explain the conversion from Gaussian standard deviation to noise percentages. 

**What are some significant follow up work from this paper? How do they differ from this paper?**    

Rethinking the Principle of Gradient Smooth Methods in Model Explanation         
https://arxiv.org/abs/2410.07711         
Dynamizes the noise hyperparameter

NoiseGrad — Enhancing Explanations by Introducing Stochasticity to Model Weights    
https://arxiv.org/abs/2106.10185            
Adds noise to decision boundaries instead of the input then examines the fusion of this method with SmoothGrad (called FusionGrad)   

Smooth Grad-CAM++: An Enhanced Inference Level Visualization Technique for Deep Convolutional Neural Network Models       
https://arxiv.org/abs/1908.01224                
Combines SmoothGrad and GradCAM++ methods

**New Terms**
Gradient-based vs Occlusion-based methods for creating saliency maps
Integrated graidentnt
Layerwise Relevance Propagation
DeepLift
Deconvolution
Guided Backpropagation
