## SEQ2SEQ-VIS : A Visual Debugging Tool for Sequence-to-Sequence Models

**Hendrik Strobelt et al, 2018**

This paper presents a visual debugging tool for sequence to sequence models.

Paper: [https://arxiv.org/pdf/1804.09299](https://arxiv.org/pdf/1804.09299)     
Code:  [https://github.com/HendrikStrobelt/Seq2Seq-Vis](https://github.com/HendrikStrobelt/Seq2Seq-Vis)

### 1. INTRODUCTION

Attention-based sequence-to-sequence models (seq2seq) (or encoder-decoder models) are the state of the art in sequence prediction tasks 
like (machine translation, natural language generation, summaries). But the deep net structure of these models makes predictions 
hard to explain.

For example, when using a rule-based translation system, non-trivial mistranslations can be
investigated by examining what rule misfired. Neural networks do not have explicit rules to check. 

This work aims to keep the great performance of deep nets but aims to provide
the ability to interactively explore issues using SEQ2SEQ-VIS — a visual debugging tool.


### 2. SEQUENCE-TO-SEQUENCE MODELS AND ATTENTION

The assumption is that a sequence to sequence model for machine translation works in five stages:

1. **Encoder (S1):**   

This takes in a sequence of words in the source language and processes it (via RNN, LSTM, CNN or 
Transformer) into a sequence of vectors that capture the meaning 
of each word and their purpose within the sentence. 
This paper's method supports all encoding methods.

2. **Decoder (S2):**

This has a similar mechanism to the encoder except that it does it for the target language. 
using the previous output tokens of the model.

3. **Attention (S3):**

This is the stage where the decoder outputs and the encoder outputs interact. The attention mechanism seeks 
to find at what portions the current decoder sequence and the encoder sequence "match up". This is done by taking dot products.
Vectors that match up should result in higher values.

For example, in a machine translation task (English to French), the vector for "Hello" should get a high dot product with the vector for "Bonjour."

4. **Prediction (S4):**

This stage involves computing probabilities for all the words in the target language that they might be 
the next token in the prediction. It uses the context vector reseulting from the attention mechanism 
to predict this probability distribution.

5. **Search (S5):**

Piecing together a translation is not as simple as picking the most likely token at each time step. 
This does not result in good predictions. 
Instead, we preserve the K most likely tokens at each time step
and when all branches have terminated (by predicting the stop token),
beam search (a variant of tree search) is used to find the translation with the highest score.

The visual analytics system is built around these five stages.

### 3. MOTIVATING CASE STUDY: DEBUGGING TRANSLATION

The authors present a representative case study to explain why their tool exists and 
how it might be used.

A user finds an instance mistranslated by a German-to-English model.

The source sentence: Die langsten Reisen fangen an, wenn es auf den Straßen dunkel wird.   
The target sentence: The longest journeys begin, when it gets dark in the streets.       
The mistranslation:  The longest journey begins, when it gets to the streets.

The model failed to translate **dunkel** into **dark**.

**Encoder (S1) Error?**

SEQ2SEQ-VIS lets the user see encoder states from the training data
that are similar (in vector-ish distance) to 
the mistranslated **dunkel**. 

![image](https://github.com/user-attachments/assets/3ea43d42-afee-4759-ba06-21898e363c21)

We can see that the closest instances match similar uses of the word. Therefore, 
the error is unlikely to have come from the ecncoder.

**Decoder (S2) Error?**

SEQ2SEQ-VIS also lets the user see similar decoder states at time step $$t$$  
(The longest journey begins, when it gets) and time step $$t+1$$ 
(The longest journey begins, when it gets to). 

![image](https://github.com/user-attachments/assets/3720e924-e74f-439e-a25f-d7830e6fc9c8)

These decoder state at $$t+1$$ is close to ones that support **dark, darker** 
or **darkness** as a prediction. Therefore, 
the error is unlikely to have come from the decoder.

**Attention (S3) Error?**

SEQ2SEQ-VIS lets the user see that the connection from "get" to **dunkel** 
is very strong (wide). This means that the model is strongly focusing on 
"dunkel" when generating the next word in the English translation.

![image](https://github.com/user-attachments/assets/d61c6425-fa62-46a6-a60d-5cf9733c7a9d)


**Prediction (S4) Error?**

SEQ2SEQ-VIS can also let us see the probabilities the model assigns to each word
to predict the next one in the sequence. 
And we can see (in Figure 5 above) that the model assigns a higher probability to "to" than "dunkel", but 
both probabilities are very close and so we would not consider the mistranslation a prediction error
as such local mistakes should be fixed by beam search.

**Search (S5) Error**

We now know the problem is with the beam search. 
SEQ2SEQ-VIS lets the user examine the entire beam search tree. 
We see then that "dark" never makes it into our tree. 
So, we can imagine a reasonable global solution would be to
increase the value of K (the K-most likely next words, the width of the search tree).

![image](https://github.com/user-attachments/assets/2dbb4738-8089-4f8d-b52e-45a1b51eb65f)

SEQ2SEQ-VIS also lets the user evaluate what would have happened 
if she had forced the system to pick "dark" after "gets". 
And we see this would have given a correct solution.

### 4. GOALS AND TASKS

SEQ2SEQ-VIS aims to meet three domain goals for sequence-to-sequence models:    
  
1. **G1 - Examine Model Decisions (errors)**      
SEQ2SEQ-VIS helps users examine the model's decision chain (encoding, decoding, attention, and beam search)
by providing the ability to visualize parts of each stage. 

2. **G2 - Connect Decisions with Training Data Samples**    
Because, the training data is the model's worldview, examining past
training data samples similar to the inference 
instance we are investigating is often revealing.

3. **G3 - Test Alternative Decisions**
SEQ2SEQ-VIS also has the goal of allowing users to explore what-if scenarios by
intervening in model decisions at different steps to see what possible outcomes could have resulted
from a different decision.  
This is important in understanding what decisions would have been "correct" and therefore, in 
brainstorming model-level fixes.

Using these goals, the authors comple five tasks for SEQ2SEQ-VIS:

1. T1 - Create visualizations of each of our five decision stages [G1]
2. T2 - Visualize the progression of latent vector sequences over time [G2]
3. T3 - Explore vectors produced by decoder, encoder stages) and look at their nearest neighbor vectors pulled from the training set [G2]
4. T4 - Generate reasonable alternative decisions for different stages of the model to compare possible corrections [G1, G3]
5. T5 - Create an interface with a cohesive front-end for many sequence-to-sequence problems (like translation, summary, and generation) [G1, G2, G3]


### 5. DESIGN OF Seq2Seq-Vis

SEQ2SEQ-VIS was designed by experts in machine learning and experts in visualization. 
It facilitates two major modes of analysis: the translation view (the upper part) and the neighborhood view (the lower part).

![image](https://github.com/user-attachments/assets/bb0bd258-5dc8-4e73-80ca-1a5290f7f274)

We have each of the five stages of the model decision visualized. On top is the attention visualization with the source sequence in blue and the target sequence in yellow. Attention values are represented as edges between them- the larger the value, the thicker the edge [S3]. To prevent visual clutter, very skinnier edges are omitted.

The top K most likely words at each time step are listed under the chosen words of the target sequence [S4]. Below that is the Beamsearch Tree [S5]. 

Below the tree, the neighborhood view begins. 

This view is concerned primarily with allows us to compute sequence vectors (from the encoder and decoder, or even context vectors from the attention stage) and then search for similar vectors (precomputed from training data samples). These vectors are visualized in the state trajectories and the neighbor list. 

The vectors have their dimensions reduced by t-SNE (t-Distributed Stochastic Neighbor Embedding), non-metric MDS (Multidimensional Scaling) or a custom projection, so they and their distances from each other can be represented on our 2D screen. When we hover an input vector, its neighborhood vectors are highlighted. 

Furthermore, if three states from a decoder sequence have one common neighbor, that neighbor has its radius to $$r(x) = \sqrt{2x} = 2.5$$ so more connected neighbors are bigger. Each input vector also has its own little cut out in the trajectory pictograms where we can more cleary examine its neighbors and how each vector changes in time. 

Clicking on a vector pulls up its neighbor list on the right, with the neighbor vectors written out in their original training sentences. 

#### 5.3. Global Encodings and Comparison Mode

SEQ2SEQ-VIS has a unified look and feel, with a consistent color scheme (encoder - blue, decoder - yellow, pivot - green, compare - violet). Clickable visual elements have round corners. Hovering highlights elements in red. 

We can trigger comparison mode which overlays elements and highlights differences in violet. Triggers are disabled in the comparison view.

#### 5.4. Interacting With Examples

**Model-Focused Interactions**

This helps model architects produce similar examples to a current sequence to test small, reasonable variations for various model stages. The user can click on a word to substitute it with a similar one. 
The program searches for neighbors for the selected word and displays them in a word cloud. Clicking on one of the option triggers an automatic replacement and a new translation in comparison mode.

![image](https://github.com/user-attachments/assets/74115557-196a-4c86-b7b3-5e049a73f1a7)

Another model-focused interaction lets the user change predicted sequences (by modifying attention values) by repeatedly clicking an encoder word to give it more weight. 







#### 5.5. Design Iterations

### 6. IMPLEMENTATION

SEQ2SEQ-VIS integrates a visualization client 
with a running sequence to sequence model through a REST API.

**Model Backend**

The sequence to sequence models here were built with the OpenNMT toolkit. 
The authors plan to distribute Seq2Seq-Vis as the default visualization mode
for OpenNMT.

The model is modified to expose some internal components 
like latent vectors (Internal state vectors), 
search beams (alternative translation candidates) and attention values.

Interactive capabilites like the ability to decode
sentences as they are being constructed, 
and for users to modify attention values.

**Nearest Neighbor Immplementation**

The program also extracts and stores
hidden states from training data. These states are stores as HDF5 
(Hierarchical Data Format version 5) files.
The program uses the Faiss (Facebook AI Similarity Search) library 
for quick similarity searches in high-dimensional space. 

**Visualization Components**

The visualization is built in Typescript and uses the D3.js library. 
Dimensionality reduction are done with the SciKit Learn 
library and include TSNE (t-Distributed Stochastic Neighbor Embedding) 
and MDS (Multidimensional Scaling) methods.

Flask connects the frontend with the back.

### 7. USE CASES

### 8. RELATED WORK

### 9. CONCLUSIONS AND FUTURE WORK


---

### Reflection      

**What are the strengths?** 
1. The SEQ2SEQ-VIS tool is beautiful, involved engineering.

**What are the weaknesses?**      
1. The SEQ2SEQ-VIS tool does not seem applicable to sequence-to-sequence models that do not follow the five stages as laid out.
2. The SEQ2SEQ-VIS requires non-trivial modifications to the model.


**What are some significant follow up work from this paper? How do they differ from this paper?**   

**New Terms**:
1. beam search 
2. nlp pathway   
3. rest api
4. HDF5 file
5. Faiss Libraty
6. OpenNMT
7. D3 library
8. TSNE and MDS projections


