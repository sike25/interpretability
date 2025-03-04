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

![image](https://github.com/user-attachments/assets/01fd195c-bb21-4467-9fb3-5850ac09202a)

It is important to note that the discriminative power never changed through each partial rotation step. So the authors conclude: good performance neither creates nor requires interpretability.

Interpretability is a different quality that must be measured separately to be understood.


#### 3.3. Disentangled Concepts by Layer

The authors use network dissection to compare the interpretability of the convolutional layers of Places205-AlexNet and ImageNet-AlexNet. Interpretability is measured in number of unique detectots.

Interpretability increases with deeper layers for high level concepts (objects, parts, scenes). It reduces for color.
For material, it stays roughly constant, improving very slightly. For texture,  it improves, reduces at conv4 then picks back up for conv5.

Texture and color dominate the other concepts at the lower levels. At the higher, objects and texture.

#### 3.4. Network Architectures and Supervisions

Here, the authors measure the impact of architecture and supervision on interpretability.

**Architecture**   
The authors used the last layers of each CNN where semantic detectors are usually more prominent. They found deeper, more complex architectures tend to have better interpretability, with the ranking ResNet > VGG > GoogLeNet > AlexNet. The depth ranking is exactly the same.

**Dataset**      
The Places dataset was more interpretable than ImageNet. The authors hypothesize that this is perhaps because scene detectors have layers which learn object detection and object detection is pretty interpretable.

**Supervision**    
The architecture is controlled (AlexNet). Self-taught supervision turns out to be much weaker at learning interpretable concepts than annotated supervision.
But training on Places365 results in the largest interpretation. The colorozation task is trained on grayscale images and learns virtually zero color detectors but learns texture detectors. The authors  hypothesize that the interpretable concepts learned are those needed to succeed in the primary task.

#### 3.5. Training Conditions vs. Interpretability

The authors use the Places205-AlexNet as the baseline model to measure how training conditions affect interpretability. Then they make some variants using the same AlexNet architecture:

1. Repeat1, Repeat2, Repeat3 - random initialization of weights. 
The models acheive similar interpretability (in terms of unique detectors and total detectors).

2. NoDropout - dropouts are removed from fully connected layers.
More texture detectors imerge but fewer object detectors.

3. BatchNorm - batch normalization is applied at each convolutional layer.
Batch normalization significantly decreases interpretability. The authors hypothesize that batch normalization promotes random rotations. They also make the point that hyperparameterization should consider interpretability as well as discriminative power.

Additionally, they look at interpretability across training iterations and note that it begins to appear after 10,000 interations. They also note that concept detectors never transition (i.e. texture detectors in one layer becoming object detectors as training goes on).

#### 3.6  Discrimination vs. Interpretability

The authors take CNN representations from higher layers and evaluate how well they transfer to a new task (action40 action recognition) by training linear SVMs on them and measuring the classification accuracy.

The more interpretable layers (layers with more unique object detectors) performed better on the transfer task. The auhors hypothesize that performance on the secondary task does not just depend on the number of detectors but also on their relevance to the tranfer task.

#### 3.7 Layer Width vs. Interpretability

We have found that network depth improves interpretability (section 3.4), and it is also known that depth improves discriminative power. 

Now, we look at the width (number of convolutional units at a layer). This is interesting, because it is widely understood while increasing width is very expensive, it has only marginal improvements on classification accuracy. Although, more recent research has started to show that wider networks can be made to rival deeper ones in performance.

The authors removed the fully connected layers of AlexNet and tripled the number of units at conv5 (from 256 to 768 units) then they add a global average pooling layer after conv5 fo connect it to the final class prediction. They call it AlexNet-GAP-Wide.

They trained AlexNet-GAP-Wide on Places365 and not only observed similar classification accuracy with classic AlexNet, but also got many more unique detectors and total detectors at conv5.

![image](https://github.com/user-attachments/assets/693b196a-7d29-4b89-8ec8-e2eb88eeef5f)


Although, increasing the units at conv5 to 1024 then 2048 does not improve interepretability further, indicating a ceiling on AlexNet's capacity to disentagle concepts, or a limit on the number of disentangled concepts that are useful in solving the promary task of scene classification.

### 4. Conclusion

1. Network dissection is an automatic framework for quantifying the interpretability of a neural network.
2. Interpretabiliy is not an axis-independent phenomenom.
3. Training conditions and netowrk architecture affect interpretability.

---

### Reflection      

**What are the strengths?** 

The work and outlook is thorough and visionary.

The authors consistently talk about implications next to results. And the idea that evaluating interpretability should be as standard as interpreting performance is a powerful, forward-looking idea.

**What are the weaknesses?**      

1. Broden has a limited number of concepts. It is not as robust as human interpretation, and in fact, network dissection considerably underperforms human interpretation.

2. Some explanations are creatively worded. Sections 2.2 and 3.1 took me several hours to understand.

3. When they choose to evaluate ND on only units already deemed interpretable by humans, they missed the opportunity find whether the method finds meaningful interpretation in neurons that aren't interpretable. If it does, should that not count against the framework?

4. Colors are over represented concepts in Broden with 59,250 samples of them. The next concepts are textures with 1,703 samples which is much less. Scenes have just 38.

5. There is a good chance that human raters from Amazon Mechanical Turk click random buttons to finish the task quickly and collect their pay.

6. They argued that depth of networks improved interpretability then ranked the architectures by interpretability without mentioning their relative depths. Although, anybody can look it up, it would be more complete if they had also ranked the depth.


**What are some significant follow up work from this paper? How do they differ from this paper?**    

The authors mentioned working on future research to capture the benefits of batch normalization without losing interpretability, but they never published anything of the sort. And nobody else did either.

DISCOVER: Making Vision Networks Interpretable via Competition and Dissection (2023)      
Introduces architectural changes that encourages neurons to activate sparsely (with ~ 4% of neurons activating for a given input) and consequently, specialize more claearly. This makes network dissection easier.


