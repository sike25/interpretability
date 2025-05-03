## Learning Deep Features for Discriminative Localization (CAM)
**Zhou et al, 2015**

Visualization of the importance of input image portions for classifications, achieved by a network with only convolutional layers and topped with a global average pooling layer.

Paper: https://arxiv.org/abs/1512.04150

### Related Work

1. **Weakly Supervised Object Localization:**

> This is an object detection task where the training image data has no bounding boxes.
> 
> a. Bergamo et al: Using masks to identify parts of images that maximize activation. (Sounds like RISE).
>     
> b. Cinbis et al: Combining multiple instance learning with CNNs. Multiple instance learning is about having inputs that are bags of instances. Only one instance has to be positive to make the ground truth positive.
> 
> c. Oquab et al: Overlapping patches and CNNs.
> 
> d. Oquab et al 2: Global max pooling to pin point objects on an image (closest work).
> 
> Improvement: This paper replaces global max pooling with global average pooling because this captures the full extent of the objects, not just their boundaries and is more generalizable across several kinds of problems.   

2. **Visualizing CNNs:**
   
> This is looking at the feature maps and etc learned by CNNs to understand what is going on.
>
> a. Zeiler et al: Deconvolutional networks to understand the patterns that activate CNN units.
> 
> b. Zhou et al: Networks that are taught scene recognition also learn object identificarion, and can do both in a single forward pass.
>
> Weaknesses: They ignore fully connected layers which also contibrute to classification.
> 
> Improvement: This paper uses nets without fully connected layers and retain most of the performance.
> 
> c. Mahendran et al, Dosovitskiy et al: Reconstruct input images from feature maps. Also considers fully connected layers.
>
> Weaknesses: Does not address the relevance of locales.    

### Class Activation Mapping

A CAM is a visual explanation of the relevance of input image portions for a classification. 
To find the CAMs, they use a network without fully connected layers (which are bad for object detection emergent quality produced by conv layers). Below is a description of the CAM structure.

![Zhou, Learning Deep Features for Discriminative Localization (2015)-4](https://github.com/user-attachments/assets/c6504176-a36c-4cc1-848a-15703a81608a)

Intuitively, from this description, we can see how GAP would be better than GMP, as GAP captures its values from the feature map values for all the pixels, and GMP would pick just the most important pixel. It makes one wonder why Oquab et al ever decided on GMP in the first place. 

### Weakly Supervised Object Localization

Here, the paper evaluates CAM using the popular ILSVRC 2014 benchmark dataset and the popular CNNs: AlexNet, VG-Gnet and GoogLeNet but they remove teh feed-forward layers and add a GAP layer at the end. 
They perfrorm four kinds of evaluations:

1. Classification:
> Compare the original AlexNet, VGGnet and GoogleLeNet to understand the effects of removing the feed forward layers.
> 
> There are small gains in error (1 to 2%) from losing the feed-forward layers. AlexNet shows more considerable suffering with its top-1 error going from 42.6 to 51.1. To compensate, they added in a few more convolutional layers and the error dropped to 44.9.
   
2. Localization:
> Compare with the original GoogLeNet, and NIN (network in network). They also compare CAM with back-propagation,
> 
> They generate bounding boxes from the CAM heatmaps by segmenting regions where the CAM values are 20% of the maximum CAM value. Then they take the largest connected bounding box.
> 
> The GAP networks outperform all the classical ones with the GoogLeNet-GAP achieving the lowest error rate. This is remarkable considering that they did not train their GAP models for localization, just classification, but just drawing bounding boxes from the 
 calculated CAMs would rival localization-trained models. 
   
3. Global Average Pooling v. Global Max Pooling:
> The GoogLeNet-GAP outperforms the GoogLeNet GMP.

4. Weakly v. Fully Supervised Methods:
> The fully supervised methods outperform the weak. They note they still have work to be done here.
>

### Deep Features for Generic Localization

Even though CNNs are trained on specific tasks like classifying the ImageNet database, we have found that that their deeper layers learn universal patterns which are transferable to other talks. This is also true for the GAP-CNNs in this paper, with the added benefit that these higher levels also learn useful discrimination. 

To get the final softmax weights, they collect the output of the GAP layer from GoogleNet-GAP, a vector of averaged feature maps, and feed it as inputs to an SVM. Then they test this against AlexNet's fc7 features across a handful of vision tasks and eight datasets. Fine-grained recognition features the classification of 200 bird species and their GoogleNet-GAP features performs comparably with existing approaches. 

It also performs well on pattern discovery tasks whic include discovering informative objects in scenes, recognizing concepts (like, view out of window) in images, text detection in images, finding the answer to a visual question (where are the cows) within images where it performs with an accuracy of 55.89%. 

### Visualizing Class-Specific Units

This paper goes on to visualize the class-specific units of a CNN using the ILSVRC and the Places datasets. Class-specific units are parts of the CNNs (group of neurons, kernels) that recognize textures and materials at low-level layers and objects and scenes at higher-level layers. We can see that the units detecting flowers and the units detecting grass are important to garden, infering that CNNs, in a way, learn a bag of words to make predictions-- where each word is a discriminative class specific unit.

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





