## Gender Shades: Intersectional Accuracy Disparities in Commercial Gender Classification

**Buolamwini et al, 2018**

This paper characterizes the skin type and gender distributions of two benchmark datasets 
(IJB-A and Adience) and finds lighter skin samples the majority.
It then presents a new dataset with more even distributions. 
It also looks into the performance of three commerical vision classifiers 
and finds that error rates differ significantly by gender and skin type.

Paper: https://proceedings.mlr.press/v81/buolamwini18a/buolamwini18a.pdf

### 1. Introduction

AI is being used in increasingly more crucial work, 
and there is risk for serious harm from incorrect and confident predictions.

Models trained on biased datasets tend to exhibit these biases. 
For example, Word2Vec embeddings shown to have small distances between words like "man" and "computer programmer"
and words like "woman" and "homemaker".

Work on creating fairer algorithms has not been done much in 
computer vision even though the potential for harm in the field is considerable
given its applications in skin disease recognition and law enforcement.

### 2. Related Work

**Automated Facial Analysis**

Face detection, face classification, face recognition, emotion identification.

Applications include smartphone locks and galleries, autism therapy. More controversial applications include: determination of sexuality from dating app pictures, Faception's determination of personality (features like IQ, occupation, propensity to crime) from faces.

Automated facial analysis is used in law enforcement, and previous work has shown that their systems have lower accuracies for women, Black people and young people (18 to 30).

Previous work that explored the impact of ethnicity on gender classification do not include African facial samples or in another case, had broad categories of Caucasian and non-Caucasian.
This paper explores gender classification with African faces included.

**Benchmarks**

Large vision datasets are often collated using face detection algorithms which can themselves have systematic errors. The resulting datasets encode those same biases.

LFW, a major face recognition benchmark, had 77.5% men and 83.5% White samples. And models trained on this dataset have only been evaluated for overall accuracies. It is not obvious how well they would do on subgrouped tests.

In response, the IJB-A dataset was released as the most geographically diverse datasets. It was collated without face detector algorithms. The Adience dataset was released to boost work on gender and age recognition. 

### 3. Intersectional Benchmark

The authors create a new dataset with more balanced skin type and gender representations.

**3.1. Rationale for Phenotypic Labeling**

Racial and ethnic labels vary across geography and with time. So, the authors use skin type labels.

**3.2. Existing Benchmark Selection Rationale**

1. **IJB-A** is a US government benchmark released by the National Institute of Standards and Technology (NIST) in 2015. The authors selected it because it was explicitly created to be geographically diverse. This dataset consists of 500 unique public figures. It was labelled manually using the Fitzpatrick skin types.

2. **Adience** was selected because it is also a recent benchmark. It contained 2,285 unique individuals but only 2,194 could be classified by skin type and gender.

For both these datasets, the authors labelled one skin type per one image.

**3.3. Creation of Pilot Parliaments Benchmark**

Preliminary analysis showed an overrepresentation of light men, and an underrepresentation of dark women and dark folks in general.

The authors create the Pilot Parliaments Benchmark to do better. It contains 1,270 individuals from three African countries (Rwanda, Senegal, South Africa, South Africa) and three European countries (Iceland, Finland, Sweden). 

The individuals are parliamentarians. They are public figures with publically available images. These contries were selected for the gender parity of their national parliaments. And deliberately from two groups of countries with starkly different skin types.

As a result of the official nature of the images, pose, illumination and expressions have little variation. This is important because pose, illumination and expression (PIE) have been shown to impact the accuracy of automated facial analysis.

![image](https://github.com/user-attachments/assets/ee92d1b4-2f81-40d8-8c69-a7196cdf1cd1)


**3.4. Intersectional Labeling Methodology**

1. **Skin Type Labels:** The authors use the Fitzpatrick classification system (Type I to Type VI). Due to its creation for assessing skin cancer risk, this system is skewed to underrepresent darker shades on its limited spectrum. But it will do.
![image](https://github.com/user-attachments/assets/5ca7b14c-9055-49e3-8efb-34defdd4edb1).

2. **Gender Labels:** They use "male" and "female" labels, based on perception of the subjects.

3. **Labeling Process:** Skin type and gender labelling was done manually by the authors and one other annotator, using perception, name of the subject and offical titles (Mr and Mrs).
The classes (I, II and III) are often aggregated as light and (IV, V and VI) as dark to control for one-off errors in labelling.

**3.5. Fitzpatrick Skin Type Comparison:**

![image](https://github.com/user-attachments/assets/76b7054d-5b98-4172-8d13-9903e8fa7f41)

1. Darker females are the least represented in IJB-A (4.4%).
2. Darker males are the least represented in Adience (6.4%).
3. Lighter males are the most represented unique subjects in all datasets.
4. Adience has the most skewed distribution by skin type.
5. IJB-A is has 59.4% unique lighter males, 41.6% and 30.3% in PPB.

### 4. Commercial Gender Classification Audit

The authors evalauted three commercial gender classifiers. Overall, men and people with lighter skin got classified with smaller error rates. The gender results confirms an earlier investigation (Ngan et al, 2015).

The evaluated classifiers were:
1. Microsoft's Cognitive Services Face API
2. IBM's Watson Visual Recognition API
3. Face++'s API (Face++ is a computer vision company hq'ed in China).

None of them reported metrics on how well their classifiers did broken down by skin-type.

**Key Findings on Evaluated Classifiers**

The classifiers were evaluated on the Pilots Parliament benchmark.

![image](https://github.com/user-attachments/assets/e203d438-7553-4aa0-acdf-3fde1f3c094c)

The authors then evaluate the classifiers on just the South African subset of the dataset. This is to control for the differences in image quality and pose variations across countries. The results mirror the accuracies on the larger dataset-- indicating that skin type is a better explanation for error rates than the country the pictures came from.

**Classification Confidence**

Sifnificant variations in confidence scores was found across the intersectional subgroups for IBM. Microsoft and Face++ do not output confidence scores.

![image](https://github.com/user-attachments/assets/975c295c-69c6-4425-9476-d5da378822b9)

---

### Reflection      

**What are the strengths?** 
1. The PPB is a very well-considered and quality dataset (number of samples, gender and racial parity, PIE consistency in images). Seems it will be useful in understanding gender and racial bias in classifiers for white women and Black people.
2. Paper is clear and simply written.
3. I liked the emphasis on the translation of imbalance and bias from datasets to embeddings to models, or from dataset collation algorithms to the collated datasets  to the models trained on them.
4. References to mainstream media as well as research papers, and involved analysis of results and realities often missing in most research papers.

**What are the weaknesses?**      
1. Mostly Caucasian and Black samples are represented in PPB.
2. Manual classification of people with the Fitzpatrick scale can be hard and prone to one-off errors. So they often summarize their results into light and dark, combining three each of the Fitzpatrick classes. This kind of results in a situation where it seems the scale was unnecessary.

**What are some significant follow up work from this paper? How do they differ from this paper?**    
1. Actionable Auditing: Investigating the Impact of Publicly Naming Biased Performance Results of Commercial AI Products (Buolamwini et al, 2019).
   Identifies that publicly naming companies with biased products encourages them to adjust.
3. 
