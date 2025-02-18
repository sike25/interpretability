## Analysis Methods in Neural Language Processing: A Survey (Methods)
**Yonatan Belinkov et al, 2019**

This survey explains the ways natural language models are globally explained or interpreted. 
That is, how does one take a trained language model and figure out what concepts it knows.   

### Introduction

Here, I summarize section 2 (What linguistic information is captured in neural networks) of the survey 
where they go through existing methods in the field.

Language networks are typically trained to take in an input and return an output. 
For example, BERT is trained in phrase completion.      
Example: Input  -> She is going to the      
         Output -> market         

But what other features does BERT learn about the input phrase? Can it recognize that "going" is a verb? 
Does it capture the relationship between "going" and "to"?

When answering questions like this, researchers often look into three dimensions. 
1. What methods are suitable for probing models?   
2. What are these models being probed for? (e.g. probing BERT for the ability to recognize verbs)   
3. What parts of the model are we probing? (e.g. embeddings at what layers?)

### Methods

1. **Diagnostic Classifiers:**

   This is the most common method.

   A model trained for one task (say, MT = main task) has its representations (for example, hidden activations) extracted.
   This representation is fed into a smaller (typically linear) classifier to predict another task (ST).
   How well the classifier does on ST is used as an indication of how well the representation captured ST
   and so indirectly, how well the original model (inadverently) learned ST while it was being supervised on MT.

   It is also called auxiliary prediction tasks or probing tasks. 

   An example is Xing Shi, Inkit Padhi, and Kevin Knight. 2016b. Does String-Based Neural MT Learn Source Syntax?
   They found that local features tend to be stored in lower layers and global ones in higher layers.

   PS. Coming back... are they stored there, or are they just more accessible there? See the slides

2. **Attention Weight Analysis:**

   Here, instead of training a classifier on the hidden state representations.
   Researchers look at the representations directly.

   An example is (Voita et al., 2018) where they counted how often the attention weights of
   antecedents nouns and their anaphoric pronouns coincided.

3. **Direct Correlation Analysis:**
 
   (Qian et al., 2016a) correlated RNN state activations with the depth in a synctatic tree.
   
   (Wu and King, 2016) correlated RNN state activations with Mel-frequency cepstral coefficients (MFCCs)
   which are a small set of features that describe the timbre (or tone color) of sound.
   This is interesting because MFCCs were the features extracted from sound to train classic machine learning models
   before deep learning became more prominent.
   
5. **Indirect Correlation Tasks:**

    Alishahi et al. (2017) examined how a model supervised on speech encoded phonology.
    Given three phonemes, A, B and X, they would check whether the model representations for X is closer to A or B.
   The question being do similar phonemes have similar representations?

    A phoneme is the smallest unit for sound. Example is: /b/ and /p/ in "bat" and "pat".
   /b/ is a phoneme because it changes the word "bat" when it changes.

### Linguistic Phenomena

The kinds of linguistic concepts researchers have investigated the question of whether models learned them include: 

1. Basic Properties:
   sentence length, word position, word prescence, word order.
2. Morphological Properties:
   suffixes, prefixes, root words.
3. Synctatic Information:
   subject-verb agreements, parts of speech.
4. Semantic Information:
   word meanings, context, implication.
5. Speech Information
   speaker identity, accents.

###  Neural network components

The components of language models which are usually evaluated include:
1. Word embeddings
2. Sentence embeddings
3. RNN hidden state or gate activations
4. Attention weights in sequence-to-sequence models
5. Neural network layers
   
Some studies have compared the similarities bewreen convolutiinal filters and word embeddings in joint language-vision models.

### Limitations

That information is captured does not mean it was used in prediction, so it is not a perfect measure of explainability.

Most of the methods look for correlation. But 

The quality of the classifier is too important


---

### Reflection

**What is this paper about?**       

**What are the strengths?** 

**What are the weaknesses?**      

**What are some significant follow up work from this paper? How do they differ from this paper?**    


new info:
attention mechanism
hidden state activation to record activations
anaphora
