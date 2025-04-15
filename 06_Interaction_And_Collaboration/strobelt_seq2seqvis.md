## SEQ2SEQ-VIS : A Visual Debugging Tool for Sequence-to-Sequence Models

**Hendrik Strobelt et al, 2018**

This paper presents a visual debugging tool for sequence to sequence models.

Paper: [https://arxiv.org/pdf/1804.09299](https://arxiv.org/pdf/1804.09299)     
Code:  [http://seq2seq-vis.io]( http://seq2seq-vis.io)

### 1. INTRODUCTION

Attention-based sequence-to-sequence models (seq2seq) (or encoder-decoder models) are the state of the art in sequence prediction tasks 
like (machine translation, natural language generation, summaries). But the deep net structure of these models makes predictions 
hard to explain.

SEQ2SEQ-VIS is a visual analytics tool that attempts to help with this by meeting three goals:
1. Examine model decisions (errors)
2. Connect decisions with training samples
3. Explore what-if scenarios

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
if she had forced the system to pick "dark" after "gets". This would have given a correct solution.

### 4. GOALS AND TASKS

### 5. DESIGN OF Seq2Seq-Vis

SEQ2SEQ-VIS was designed by experts in machine learning and experts in visualization. 
It facilitates two major modes of analysis: the translation view and the neighborhood view.

#### 5.1. Translation View

#### 5.2. Neighborhood View

#### 5.3. Global Encodings and Comparison Mode

#### 5.4. Interacting With Examples

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


**What are the weaknesses?**      


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


