## Network Dissection: Quantifying Interpretability of Deep Visual Representations
**Bau et al, 2017**

This paper attempts to measure how interpretable or disentangled representations of convolutional neural networks are, automatically, without intermediate human intervention to map network pieces to human concepts.

1. Paper: https://arxiv.org/pdf/1704.05796
2. Code and data: https://netdissect.csail.mit.edu/

### 1. Introduction

Networks with disentangled representations can seem more interpretable, but interpreting models via their disentangled representations raises some questions about whether how we interpret models is actually how they work, and what aspects of training creating more or less entangled representations.

This paper introduces **network dissection** which is a framework for mapping latent representations to human-interpretable concepts. It uses **Broden** (a dense and broad dataset) on several CNNs (**AlexNet, VGG, GoogLeNet, ResNet**) to show that:

The direct mapping of neuron A to concept B can be scrambled (to for example become concept B across neurons A, D and Z) while the networks behaves exactly the same at its primary task.

#### 1.1. Related Work

Understanding CNN behavior can be achieved by:
1. Visualizing image portions that maximize activations.
2. Using backpropagation variants to generate important image features.
3. Isolating network pieces and assessing them on other tasks.                 
  But all these methods require human intervention in mapping the concepts while our authors are interested in mapping interpretable artifacts to human concepts automatically.

More research in understanding how individual pieces of a network include:
1. Showing units in scene detection networks act as object detectors.
2. Automatically generating images likely to cause high activations in certain neurons
3. Placing probes (linear classifiers) between network layers to predict certain concepts from the layer's activations.

### 2. Network Dissection

The network dissection framework can be broken into three steps:
1. Choose a broad set of human-understandable concepts (outdoors, woman, food)
2. Gather data about how strongly the network's internal components react to these concepts (raw activation values).
3. Analyze the data to see how well the internal components' reaction align with the human concepts.

Remember the idea here is to quantify the network's disentanglement (a measure of interpretability) not its primary effectiveness. So it is okay that they check neuron-concept alignment, even though, in reality, some concepts are spread around all of its neurons in weird ways we can't deduce.

This paper collects visuals concepts from the **Broden** dataset (introduced below). Then it measures the alignment of each hidden unit of the CNN with each concept (in form of activation score). The number of distinct visual concepts that are aligned with any of the hidden units is an interpretability score.

#### 2.1. Broden: Broadly and Densely Labeled Dataset

![image](https://github.com/user-attachments/assets/ce58d096-5604-4e3c-a131-e749fcc8e079)

The authors newly assembled Broden for this work from several other diverse, densely-labeled image datasets (ADE, Open-Surfaces, Pascal-Context, Pascal-Part, the Describable Textures Dataset). 

Each image's pixel is labelled with concepts it corresponds to, except for scenes and  textures where the labels are image-wide. The pixels are also labelled with 1 of 11 colors. Other concept labels were merged across datsets and only labels with at least 10 image samples were included.

#### 2.2. Scoring Unit Interpretability

For every individual convolutional unit in the network, they run each of their 1_378 concepts from Broden and ask: does this unit align with this concept? This is a binary segmentation task (i.e. the answer is yes or no). This run is a forward pass. There is no need for training or back-propagation.

# incomplete



### 3. Experiments

The CNN architectures they use include: **AlexNet, GoogLeNet, CGG** and **ResNet** trained on **ImageNet** (objects), **Places205** and **Places365** (places) datasets, for supervised tasks.

For self-supervised tasks, they use severals recent models trained on predicting context, solving puzzles, predicting ego-motion, learning bt moving, predicting video frame order, tracking, detecting object-centric alignement, colorizing images, predicting cross-channel and predicting ambient sound from frames.    
All these models use the AlexNet architecture or a variant of it.

#### 3.1. Human Evaluation of Interpretations

The goal here is to show their method produced interpretations which are "correct".

The authors established a form of ground truth by starting with convolutional units another study [40] had decided were interpretable and had labelled with concepts. This is important because not all units are interpretable, and we would only want to evaluate the method on the interpretable ones.

A group of human raters from Amazon Mechanical Turk were asked to evaluate [40]'s interpretations. Another group was asked to evaluate the interpretations from network dissection. For each interpretable unit, the raters got 15 images (from AlexNet-Places205) with highlighted patches and had to decide (yes/no) whether most of them corresponded with a given phrase.

The proportion of interpretations marked as descriptive by the human raters are reported per layer the unit came from in the following table.

------------------------------------------------------------------------
| | conv1 | conv2 | conv3 | conv4 | conv5 |
|--------------------------|---------|---------|---------|---------|---------|
| Interpretable Units | 57/96 | 126/256 | 247/384 | 258/384 | 195/256 |
| Human Consistency | 82% | 76% | 83% | 82% | 91% |
| Network Dissection | 38% | 56% | 54% | 59% | 71% |

Network dissection underperforms human interpretation, but still performs decently and like human interpretation, even better at higher layers.

#### 3.2. Measurement of Axis-Aligned Interpretability

Is it reasonable to measure interpretability by looking at individual neurons? When we find a neuron that can detect "face", does that mean that neuron learned to detect faces, or it's just a coincidence? 

The authors  randomly change the basis of a representation (i.e. rotate the representation) and effectively mix its activity with others. If the representation loses some interpretative potential, we know it is **not** just a coincidence because if it was, why does a random new basis no longer have coincidences?

They use a random transformation (rotation) Q on the 256 units of AlexNet-Places205 conv5 and observe an 80% decrease in the number of unique detectors. This strongly indicates that networks genuinely learn the interpretations we query via dissection.

Additionally, the authors broke the rotation Q into smaller steps, going from a = 0 (no rotation) to a = 1 (completed rotation) and values in between represent partial rotations.

Measuring the interpretability (using the number of unique detectors)  at each step shows that the loss of interpretability gradually happens as the rotation progresses.

It is important to note that the discriminative power never changed through each partial rotation step. So the authors conclude: good performance neither creates nor requires interpretability.

Interpretability is a different quality that must be measured separately to be understood.


#### 3.3. Disentangled Concepts by Layer

The authors use network dissection to compare the interpretability of the convolutional layers of Places205-AlexNet and ImageNet-AlexNet. Interpretability is measured in number of unique detectots.

Interpretability increases with deeper layers for high level concepts (objects, parts, scenes). It reduces for color.
For material, it stays roughly constant, improving very slightly. For texture,  it improves, reduces at conv4 then picks back up for conv5.

Texture and color dominate the other concepts at the lower levels. At the higher, objects and texture.

#### 3.4. Network Architectures and Supervisions

#### 3.5. Training Conditions vs. Interpretability

#### 3.6  Discrimination vs. Interpretability

#### 3.7 Layer Width vs. Interpretability

### 4. Conclusion

selection bias is incomplete. by evaluating their network on units already deemed interpretable by humans, they failed to find whether the method 

Colors are over represented concepts in Broden with 59,250 samples of them. The next concepts are textures with 1,703 which is much less. scenes have just 38.

some explanations crazily difficult to understand (ex, intro explainability is axis-aligned). the math of network dissection

amt raters might click random buttons to get through the task and make that mula

broden has limited concepts

