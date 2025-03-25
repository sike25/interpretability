## Fair Prediction with Disparate Impact
A study of bias in recidivism prediction instruments

**Chouldechova et al, 2016**

This paper studies recidivism prediction instruments (predictors that a criminal defendant will reoffend), as well as the criterion for evaluating the fairness of these instruments. 

They demonstrate that an application of this criterion, while seeming fair, leads to disparate impact.

Paper: [https://arxiv.org/pdf/1412.6572](https://arxiv.org/pdf/1610.07524)

### 1. Introduction

#### 1.1. Data Description and Setup

The dataset is based Broward County data made publicly available by ProPublica. 
It contains COMPAS recidivism risk decile scores, 2-year recidivism outcomes,
and other demographic and crime-related factors. The authors restrict their subset to
African American (b) and Caucasian (w) instances.

![image](https://github.com/user-attachments/assets/e452ce61-d989-487d-a94b-7ac7cd02ed78)

Note: COMPAS is a recidivism prediction instrument, that is free of predictive bias.

### 2. Assessing Fairness

Let:
$$S = S(x)$$ denote the risk of recidvism, with higher numbers indicating more risk.    
$$X = x$$ denote the variables.   
$$R \in {b,w}$$ denote the racial group of the defendant.
$$Y \in {0,1}$$ be the outcome indicator where 1 means recidivism occured.

A score $$S(x)$$ is **test-fair** if it reflects the same likelihood of recidivism irrespective 
of their group. That is:       
$$P(Y=1|S=s,R=b) = P(Y=1|S=s,R=w)$$

#### 2.1. Implied Constraints on the False Positive and False Negative Rates

**Coarsened Score**

For simplicity, the author introduces $$S_c$$ a coarsened score.

. $$S_c(x)$$ is HR (high-risk) if $$S(x) > S_{HR}$$ and         
. $$S_c(x)$$ is LR (low-risk) if $$S(x) > S_{LR}$$

We can think of $$S_c$$ as a binary classifier and write a confusion matrix like:

| | Sc = Low-Risk | Sc = High-Risk |
|-------|------------|--------------|
| Y = 0 | TN         | FP           |
| Y = 1 | FN         | TP           |

**Test Fairness of Sc**

$$S_c(x)$$ is **test-fair** if the positive predictive value does not depend on $$R$$.     
That is, the quantity: $$PPV(S_c|R=r) \equiv P(Y=1|S_c=HR, R=r)$$ does not depend on $$R$$.

Another variable to consider is the recidivism prevalence within groups, $$p_r \equiv P(Y=1|R=r)$$.

So we can represent the relationship between the ***false positive rate*** $$FPR \equiv P(S_c(x)=HR|Y=0)$$ and the ***false negative rate*** $$FNR \equiv P(S_c(x)=LR|Y=1)$$ as:

$$FPR = \frac{p}{1-p} \frac{1-PPV}{PPV} (1-FNR)$$

From this equation, we can see that when the recidivism prevalence differs across groups, we can not have equal FNR and FPR if the test score $$S_c$$ is to be fair.

This means we can not simulataneously achieve:
1. Equal positive predictive values across groups (test fairness)
2. Equal false positive and negative rates across groups
3. When the base rates of recidivism differ between groups

In the case, the base rates of White defendants in the data is 39%, and for Black defendants, it is 51%. So it differs.   
This implies a policy trade-off, even error rates versus test fairness.

### 3. Assessing Impact

The author now considers how differences in high positive and negative rates result in disparate impact for defendants in situations where high risk defendants are subject to stricter penalties. Penalties here refers to bail, sentencing or parole decisions.

Let the penalty be $t_L \leq T \leq t_H$.

If it is decided by a min-max policy:          
$$T_{MinMax} = t_L$$ if $$S_c=Low-Risk$$  and             
$$T_{MinMax} = t_H$$ if $$S_c=High-Risk$$ 

Let:        
$$T_{r,y}$$ be the penalty for a defendant of group $$R=r$$ and outcome $$Y=y \in {0, 1}$$.   
$$\Delta = \Delta (y_1, y_2) = E(T_{b,y_1} - T_{w,y_2})$$ be the expected difference in penalty between defendants in different groups.

$$\Delta$$ is a measure of disparate impact.

**Propositions**
The expected difference in penalty under the min-max policy is:

$$\Delta \equiv E_{MinMax} (T_{b,y_1} - T_{w,y_2})$$      
$$= (t_H - t_L) (P(S_c = HR | R = b, Y = y_1) - P(S_c = HR | R = w, Y = y_2)$$
= difference in penalty times difference in recidivism prediction

Therefore:
**For Non-recidivators**
$$\Delta = (t_H - t_L) (FPR_b - FPR_w)$$  

**For Recidivators**
$$\Delta = (t_H - t_L) (FNR_w - FNR_b)$$  

When keeping PPV constant amongst groups, the group with the higher base rates of recidivism tend to have higher FPRs and lower FNRs. We can see that this will result in higher penalties for the higher recidivism group.

In the case where $$t_L=0$$ which is applicable to misdemeanors and $$t_L=0$$ is coded as the abscence of incarceration, and $$t_H=1 means some prison time, it is clear that a non-recidivist individual from group with the higher rate of recidivism is $$\frac{FPR_b}{FPR_w}$$ times more like to get some jail time that his counterpart.

The author also points out that even though high FPRs are correlated with heavier criminal histories, there is still FPR discrepancies among Black and White defendants even within the same prior record subgroups. In other words, the influence of race on high FPRs can not be explained alone by Black defendants having worse criminal records. 

![image](https://github.com/user-attachments/assets/b7fb5f1c-7636-4f81-90e1-23a4fdc029b4)

#### 3.1. Connections to Measures of Effect Size

The level of disparate impact $$\Delta$$ is related to the "% non-overlap measure", which is a more classic statistic.

The "% non-overlap measure" quantifies how different two groups are fom each other. It is the percentage of one group which is completely outside the other assuming both distributions are normal. A common example is Cohen's $$d$$. 

Although the COMPAS decile score is far from Gaussian, the author uses "total variation distance" to calculate the % non-overlap. This does not require an assumption of normality, and is the largest possible difference between the probabilities two distributions can assign to the same event.

This value lets the author establish the maximum possible value (sharp bound) for disparate impact $$\Delta$$. 

$$\Delta \leq (t_H - t_L) d_{TV}(f_{b,y}, f_{w,y})$$

Where $$f_{r,y}(s)$$ be the score distribution for race $$r$$ and recidivism outcome $$y$$.     
And $$d_{TV}(...)$$ is the total variation distance.


### 4 Discussion

The point of this paper is to show that using recidivism prediction instruments that are known to be free of predictive bias can still result in disparate impact. 

This is because controlling for the false positive rate across groups means that, by necessity, some other factors can not be controlled across groups. And if these factors come into the calculation of a penalty, the result will not be free from bias.

It is important to note that even with these findings, recidivism predication tools have less bias than human judgement. So this is a call for reiteration and improvement, not a complete rejection of their application.

---

### Reflection      

**What are the strengths?** 
1. Incredible in relevance and simplicity.
2. Purely theoretical and generalizable.
3. Succint and clear.

**What are the weaknesses?**      
None!!!

**What are some significant follow up work from this paper? How do they differ from this paper?**   

compas

