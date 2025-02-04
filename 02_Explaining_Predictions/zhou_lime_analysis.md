# Zhou: LIME: Learning Deep Features for Discriminative Localization (2015)

## What is this paper about?
Local discrimination in images achieved by global average pooling layers and networks without fully connected layers.

### Related Work
1. Weakly Supervised Object Localization:
This is object localization as an emergent quality of training CNNs to classify images.
a. Bergamo et al: Using masks to identify parts of images that maximize activation. (Sounds like RISE).
b. Cinbis et al: Combining multiple instance learning with CNNs. Multiple instance learning is about having inputs that are bags of instances. Only one instance has to be positive to make the ground truth positive.
c. Oquab et al: Overlapping patches and CNNs
d. Oquab et al 2: Global max pooling to pin point objects on an image (closest work).
Improvement: This paper replaces global max pooling with global average pooling because this captures the full extent of the objects, not just their boundaries and is more generalizable across several kinds of problems.

2. Visualizing CNNs:
This is looking at the feature maps and etc learned by CNNs to understand what is going on.
a. Zeiler et al: Deconvolutional networks to understand the patterns that activate CNN units.
b. Zhou et al: Networks that are taught object classification also learn localization, and can do both in a single forward pass.
Weaknesses: They ignore fully connected layers which also contibrute to classification.
Improvement: This paper uses nets without fully connected layers and retain most of the performance.
c. Mahendran et al, Dosovitskiy et al: Reconstruct input images from feature maps. Also considers fully connected layers.
Weaknesses: Does not address the relevance of locales.

### Class Activation Mapping


What are the strengths?
What are the weaknesses?
What are some significant follow up work from this paper? How do they differ from this paper?
