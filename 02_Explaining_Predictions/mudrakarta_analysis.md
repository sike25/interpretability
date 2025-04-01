## Did The Model Understand The Question? 
**Mudrakarta et al, 2018**

This paper uses the concept of attribution (or word importance) as calculated via integrated gradients to create adversarial examples for three types of question answering tasks— visual, tabular and passage comprehension.

Paper: https://arxiv.org/pdf/1805.05492                 
Code: https://github.com/pramodkaushik/acl18_results

### 1. Introduction

Neural network models have been used to answer questions about images, passages of text and tabular data. Understanding more closely how they achieve this is useful. 
The standard way of evaluating model performance on a test set is imperfect. Because datasets are often too large for us to manually verify that how well they mirror reality. 
We must understand why models make their predictions.

Here is an illustrative example:

![image](https://github.com/user-attachments/assets/46fa9e41-2d64-4f8f-bf14-97474a970d84)

When the word "symmetrical" was changed to "spherical", the prediction remained "very".

### 2. Related Work

This paper was deeply inspired by Jia and Liang (2017). But there are key differences.

1. Jia and Liang augment the test set with crowd-sourced adversarial examples to improve evaluation technique, but their augmentation is model-agnostic.      
Mudrakarta et al query the model for its particular weaknesses then use these weaknesses to craft targeted adversarial examples. Their goal is not to improve technique but to validate their analysis (or interpretation) technique.
Mudrakarta et al's attacks are also more effective (as one  might expect).

3. Jia and Liang focus only on reading comprehension tasks. Mudrakarta et al also focus on answering questions about visual and tabular data.

4. Mudrakarta et al's method (via Integrated gradients) is specific to deep-learning-based systems. Jia and Liang's method is not.

The Integrated gradient method was introduced by Sundararajan et al (2017). The authors state other methods would have sufficed but they use IG because it is easy and efficient to implement, and they like its theoretical justification.

There have also been adversarial work done on image-based systems as per Goodfellow et al (2015) where imperceptible image pertubation influences prediction (when obviously, it should not). 
This work is similar in spirit to this language task focused paper.

### 3. Integrated Gradients (IG)

Sundararajan et al (2017) introduced the intepretability technique of Integrated Gradients. This paper uses it for attributing what parts of their input question is important for prediction. 

Here is an overview of the method:

![image](https://github.com/user-attachments/assets/00b37894-e24b-40fb-ba5c-e1d15d206a96)

Model:  $$F: R^n \to [0,1]$$
Input:  $$x = (x_1 ... x_n) \in R^n$$
Output: $$F(x) \in [0,1]$$ the probability of a specific response.

The **baseline input** x is a practical zero vector, a blanking vector. For an image input x, x' could be a black picture of the 
same size. Here, x' is a sequence of padding values. The context of the problem (the image, table or passage depending on 
the task) has its baseline input unchanged.

The **attribution** of the model with respect to a particular input is $$A(x,x') = (a_1 ... a_n)$$ where $$a_i$$ is the contribution 
(the influence, the blame assignment) of $$x_i$$ to the prediction $$F(x)$$. Attributions here are represented as integrated gradients.

**The Integrated Gradient:**

$$IG_i (x, x') = (x_i - x_i') \times \int_{\alpha = 0}^{1} \frac{\partial F(x' + \alpha \times (x-x')}{\partial x_i} d \alpha$$

$$x - x'$$ is how much x contributes relative to "nothing", the blanking vector.     
$$x' + \alpha(x - x')$$ makes up a series of gradual inputs from blank to the real image where $$\alpha \in [0,1]$$       
The gradient represents the rate of change or sensitivity of the function F with respect to each input dimension.        
We multiply the integral by $$(x_i - x_i')$$ to weight it.

The theoretical benefits of Integrated Gradients include:
1. Completeness: The sum of all our attribution values $$\sum A = a_1 + ... + a_n$$ is
   exactly equal to the difference between the model's predicted probablility for x and that
   for x '.
2. Influence: Uninfluential variables, if all else is fixed, do not change the
   model prediction. IG gives zero attribution scores to uninfluential variables. Meanwhile,
   influential variables are guaranteed to get attribution.
3. Linearity: Attributions for the linear combination of two models $$F_1, F_2$$ is the
   linear combination of their separate attributions.
4. Symmetry: Symmetric variables get equal attribution.

This work is also an empirical validation of the integrated gradient method whose publication precedes this paper by only a year.


### 4. Visual Question Answering

#### 4.1. Task, Model and Data

Task: This is a classification task. 
Given a question and a context image, the model is expected to choose an answer from 3000
classes of the most frequent answers in the training data.           

Model: The input question is tokenized, embedded and fed into the LSTM from Kazemi (2017) which attends to the context image. This model achieves 66.1% accuracy (SOTA is 66.7%) on the validation set.

Data: The VQA 1.0 dataset contains 614,163 questions on 204,721 images (3 questions per image). The images are from COCO (Lin, 2014). The questions and answers were crowdsourced.

#### 4.2. Observations

Instances where prediction does not change between the input and the baseline were omitted. 
The authors found that very few words where highly attributed and the most attributed were usually words like: "how", "what" which are less important. Important words like nouns were frequently lowly attributed.

#### 4.3. Overstability Test

The authors isolated the words that are most commonly marked with high attributions. Then they varied this isolated vocabulary from 0 to 5305 to see what percentage of accuracy the model loses from having just these words. 

And the answer was: not much.

![image](https://github.com/user-attachments/assets/e54d0c73-0bb9-4b33-9368-f7f9b525f7e6)


Just one word was enough to take the model to half its former accuracy. This was: "color".

Without even any words, the model retained 44.3% of its former accuracy indicating that the context images played a huge part in predication.

The authors posit (and I agree) that the model uses common words like: "what", "how" and "color" to determine **what** kind of question is being asked, and selects one of the appropriate answer.

Thoughts: This hypothesis is testable, I wonder if anybody checked. Also, I think this indicates the model is too small for the problem, or the problem is too general for the model.

#### 4.4. Attacks

The authors exploit the weaknesses from 4.2 and 4.3, that is, the fact that predictions are reliant on a few, less important words.

**Subject Ablation Attack**       
The subject of questions are replaced with the same commonly lowly attributed word. Several commonly lowly attributed words are used 
(“fits”, “childhood”, “copyrights”, “mornings”, “disorder”, “importance”, “topless”, “critter”, “jumper”, “tweet") and the results were averaged.

Of all the questions the model previously answered correctly, 75.6% return the same answer despite the change in subject.

Note: The averaging probably helps account for the fact that some 
swaps may not have necessarily changed the ground truth answer.

**Prefix Attack**     

The authors crafted three prefix pharases out of highly attributed words— “in not a lot of words”, “what is the answer to”, and “in not many words." The accuracy drops from  61.1% to 19%.

Prefix phrases crafted with lowly attributed words like “tell me”,
“answer this” and “answer this for me" drop the accuracy by only 46.9%.

### 5. Question Answering Over Tables

#### 5.1. Task, Model and Data

Data: The WikiTableQuestions dataset (Pasupat, 2015) contains 22,033 questions on 2,108 tables from Wikipedia.

Task: The questions ask for either table cell contents or some kind of table aggregation. The model is expected to translate the question to an SQL command which is then executed on the table. Correctness is not determined by matching the SQL command to a ground truth but by matching the final result of its execution to a ground truth.

Model: They use the state of the art Neural Programmer which achieves 33.5% accuracy on the test set. Its resultant SQL query is structured as four operators and four table columns. Example: “reset
(score), reset (score), min (score), print (name)” to find the person with the lowest score.

#### 5.2. Observations

IG is used to attribute the question with respect to the operators and columns in the SQL command. 

NP pre-processes input questions by appending tokens like "tm" and "cm_token" to signify matches between the question and the table. They are fed into the model along with the question tokens.

The attribution is visualized as an alignemt matrix.

![image](https://github.com/user-attachments/assets/0e61b01e-ff7d-4cd9-a582-5eb9ce91a119)

#### 5.3. Overstability Test

Again, the authors plot accuracy with respect to vocabulary size, isolating the most frequently higer attributed words. And again, just a few words (five - “many”, “number”, “tm token”, “after”, and “total”) are enpugh to achieve 50% of original accuracy.

#### 5.4. Table-Specific Default Programs

About 36.9% of the resulting SQL operator selections are independent of the input question, indicating a strong reliance 
on the context tables.

They also found that words like: “rank”, “gold”, “silver”, “bronze”, “nation”, “year", result in SQL queries that omit the last row. This is because such tables usually have a "total" row and they don't want to include it in for example, selecting the max medals. 

But in situations where similar tables do not have "total" rows, they are prone to produce the wrong answers.

#### 5.5. Attacks

**Question Concatenation Attacks**

The authors attach irrelevant phrases made of high-trigger words as either suffixes or prefixes. Accuracy dropped from 33.5% to 3.3%.
Irrelevant phrases made of low-trigger words only dropped the accuracy from 33.5% to 27.1%.

**Stop Word Deletion Attacks**

Deleting stop-words like "at" from questions like: "what ethnicity is at the top?" dropped accuracy from 33.5% to 28.5%.

**Row Reordering Attacks**

Consider this question: "which nation earned the most gold medals". The model uses operators: "reset", "prev", "first" and "print" to get the answer correctly. 

So it excludes the last row and prints the first row. But this is ony correct because the last row is often the "total" and these tables are usually sorted by gold medals. But of course, the model should be agnostic to the arrangement of the rows.

Perturbing the row order dropped accuracy from 33.5% to 23%.

Training against data with perturbed row tables and deleted stop words should make the model robust against these two attacks.


### 6. Reading Comprehension

#### 6.1. Task, Model and Data

Task: Identifying the part of a context paragraph that answers a question.
Data: The SQuAD dataset (Rajpurkar, 2016) contains 107,700 query-answer pairs. 
Model: The SOTA (Yu, 2018) which has an F1 of 84.6.

#### 6.2. Analyzing Adversarial Examples

Jia and Liang (2017) use ADDSENT to appends phrases (which resemble answers to the question) to the context paragraph. 

ADDSENT attack phrases are created by:
1. generating an answer to the quesion
2. replacing the nouns and adjectives with antonyms
3. replacing named entities with the closest word (in the GloVe word vector space)
4. asking human reviewers whether the new sentence made grammatical sense

The authors of this paper analyze 100 examples of these ADDSENT attacks and find:

1. The adversarial phrase modifies a relevant word that was given a low attribution.
2.  Conversely, an attack will fail when a relevant word with a high attribution is not present in the adversarial phrase.

![image](https://github.com/user-attachments/assets/b23f829b-44d6-4ccd-ad7c-72c4df484233)


#### 6.3. Predicting the Effectiveness of Attacks

The authors analyze 1,000 (question, ADDSENT attack phrase) pairs where the model got the baseline correctly. The ADDSENT attack is able to trick the model with 508 instances, but 492 not.

The authors split the examples into a first group of cases where highly attributed relevant nouns or adjectives are not in the adversarial sentence (and all other cases in the second group). As we might expect, 63% of examples in the first group fail and just 40% in the second.

Using attributions to decide what nouns and adjectives to replace is likely to greatly improve the efficacy of this attack.


---

### Reflection

**What are the strengths?**
1. Work is interesting, detailed and involved.
2. Lots of very illustrative examples.

**What are the weaknesses?**      
1. The models are not that great to start with (except reading comprehension).
