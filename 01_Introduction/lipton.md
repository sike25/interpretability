## The Mythos of Model Interpretability

**Zachary C. Lipton, 2017**

This paper 

Paper: https://arxiv.org/pdf/1606.03490

### 1. Introduction

As deep neural networks are used more and more often in critical cases like healthcare and markets, the human inability to understand them has arised as a problem.
Interpretability is cited as a solution yet it seems the definition varies to the point of discordancy. The author accepts the word as referring to several concepts. Then focuses on supervised learning to ask both **what** is interpretability and **why** we need it. 

### 2. Desiderata of Interpretability Research

One of the aims here is to clearly give definitions for interpretability and one of the ways to do that is to consider the purposes or desiderata. 
Need for interpretability come up when there is a mismatch between what the model does and what we need from it. 
One source of conflict is the fact that the goal error minimization in supervised learning does not always capture more complex, real-life goals. Another source is a mismatch between offline training data and real use-cases which are often dynamic.

**2.1. Trust**

Some describe interpretability as a prequisite to trust. What is trust? Is it confidence that the model will perform well? For a sufficiently accurate model, we can expect it to perform well and we don't need interpretability? But even a good model can be biased (for example, have racial bias in predicting crime rates) and so it is also important to consider for what examples for which it is accurate. Another aspect to trust is how we feel letting the model take charge? Does it make errors for the same examples a human might? If so, maybe we can trust it.

**2.2. Causality**

Interpretability is attractive for understanding what features lead to a decision. This kind of knowledge is often exploited to study possible causal relationships like with thalidomide and birth defects.

**2.3. Transferability**

Humans have far richer generalization skills, but models are at times deployed in situations that need stronger generalization than they have. This includes dynamic environments, scenarios when the model's own performance alters the environment and adversarial situations. Even deliberate adjustments to features that go into calculating FICO scores which do not necessarily correlate with an ability to repay debt.

**2.4. Informativeness**

We can also use models to provide human decision makers with information, as opposed to using them to make decisions. Interpretations or explanations from models can be valuable even if they don't reveal the model's internal workings (like showing similar cases that support a diagnosis). Sometimes we use supervised learning approaches when our actual goal is more like unsupervised learning - exploring and understanding data patterns.

**2.5. Fair and Ethical Decision-Making**

As models are deployed in critical social contexts like credit approval and recidivism prediction, interpretability is important in sniffing out bias and unfairness. The EU passed that any individual subject to algorithmic decisions has the right to explanation, and the right to contest based on the explanation.


### 3. Properties of Interpretable Models

**3.1. Transparency**

3.1.1. SIMULATABILITY

3.1.2. DECOMPOSABILITY

3.1.3. ALGORITHMIC TRANSPARENCY

**3.2. Post-hoc Interpretability**

3.2.1. TEXT EXPLANATIONS

3.2.2. VISUALIZATION

3.2.3. LOCAL EXPLANATIONS

3.2.4. EXPLANATION BY EXAMPLE

### 4. Discussion

**4.1. Linear models are not strictly more interpretable than deep neural networks**

**4.2. Claims about interpretability must be qualified**

**4.3. In some cases, transparency may be at odds with the broader objectives of AI**

**4.4. Post-hoc interpretations can potentially mislead**

**4.5. Future Work**

### 5. Contributions

---

### Reflection      

**What are the strengths?** 

1. 

**What are the weaknesses?**      

1. 

**New Terms**  

1. 
