## SmoothGrad: Removing Nise by Adding Noise
**Smilkov et al, 2017**

### Introduction

Interpretability of deep neural networks matter for trust, debugging and insight. 

Saliency maps (sensitivity maps, pixel attribution) is a common way of explaining visual models. They can be occlusion or gradient-based and they highlight the parts of input images that are important to prediction although the gradient-based ones are often visually noisy and also highlight parts of images that a person would not expect to be relevant to prediction. Whether this phenomenon truly is noise or an expression of the actual working of the model is unclear.

All that said, Smoothgrad is a method for clarifying gradient based saliency maps. 
It works by adding small amounts of noises to the input image many times, computing the gradient map for each noisy input and finding their average to produce a final saliency map.
Smoothgrad can be combined with other algorithms.

They also find that adding noise at training time further improves clarity of saliency maps.

### Gradients as Sensitivity Maps

M = gradient of the predicted probabilities with respect to the input image x

We add gaussian noise.

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
