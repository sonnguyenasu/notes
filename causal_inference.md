# Causal Inference Note

## 1. Notation and Definition
### 1.1. Notation

|Notation|Name|Definition|
|-|-|-|
|$W$|Unit|The atomic research object in treatment effect study|
|$W=w$|Treatement|Action that applies to a unit|
|$Y(W=w)$|Potential outcome|For each unit-treatment pair, the outcome of that treatment when applied on that unit|
|$Y^F$|Observed outcome|The outcome of the treatment that is actually applied. $Y^F=Y(W=w)$ when $w$ is the treatment actually applied.|
|$Y^{CF}(W=w')$|counter factual outcome|outcome if the unit had taken another treatment|
||Pre-treatment variable|Variable that are not affected by the treatment,also known as *background variable*|
||Post-treatment variable|Variable that are affected by the treatment|

### 1.2. Definitions
**Average treatment effect**
$$ATE=  \mathbb{E}[Y(W=1) - Y(W=0)]$$

**Average treatment effect on the treated group**
$$ATT= \mathbb{E}[Y(W=1)|W=1] - \mathbb{E}[Y(W=0)|W=1]$$
where $Y(W=i)|W=1$ are the potential outcome of the treated group

**Conditional Average Treatment Effect**
$$CATE=\mathbb{E}[Y(W=1)|X=x] -\mathbb{E}[Y(W=0)|X=x]$$
where $Y(W=i)|X=x$ are the potential outcome of the subgroup with $X=x$

**Individual Treatment Effect**
$$ITE_{i}=Y_{i}(W=1) - Y_{i}(W=0)$$
where $Y_{i}(W=w)$ are the potential outcome of unit $i$

### 1.3. Formal objectives
For causal Infernece, our objective is to estimate the effect from the observational data. Formally, given observational data $\{X_{i},W_{i},Y_{i}^F\}_{i=1}^N$, where $N$ is the number of units in the datasets, the goal is to estimate the treatment effects defined above.

## 2. Assumptions
- **Stable Unit Treatment Value Assumption (SUTVA)**: The potential outcome of an unit is not affected by treatment given to other units. Also, there are no different forms or versions of each treatment level that leads to different outcomes.
- **Ignorability:** Given background variable $X$, treatment assignment $W$ is independent to the potential outcomes, i.e., $W\perp Y(W=0),Y(W=1)|X$

With ingnorability, given same background variable value, the assignment mechanism should be the same whatever the potential outcome is. Also, the potential outcome should be the same regardless of assignment of $W$, given similar $X=x$. This is also known as *unconfoundedness assumption*, where units with similar background variable $X$, their treatment assignment can be viewed as random.

- **Positivity:** For any value of $X$, the treatment assignment is not deterministic: $P(W=w|X=x)>0,\forall w,x$

With such assumptions, the relationship between observed and potential outcome can be viewed as:
$$\mathbb{E}(Y(W=w)|X=x) = \mathbb{E}[Y(W=w)|W=w,X=x] (\text{ignorability)}$$
$$=\mathbb{E}[Y^F|W=w,X=x]$$

Then:
$$ITE_{i}= Y_{i}(W=1)-Y_{i}(W=0)$$
$$= W_{i}Y_{i}^F-W_{i}Y^{CF}_{i}+ (1-W_{i})Y^{CF}_{i} + (1-W_{i})Y^F_{i}$$

$$ATE=\frac{1}{N} \sum_{i}ITE_{i}$$
$$ATT=\frac{1}{N_{T}}\sum_{i:W_{i}=1}ITE_{i}$$
$$CATE=\frac{1}{N_{x}}\sum_{i:X_{i}=x}ITE_{i}$$


## 3. Approaches

### 3.1. Reweighting method
In sample re-weighting method, a key concept is balancing score $b(x)$. $b(x)$ is a general reweighting score, which is the function of x satisfying $W\perp b(x)$, where $W$ is the treatment assignment and $x$ is the background variable. Most trivial design of balancing score $b(x)=x$ due to ignorability assumption.

Propensity score: The conditional probability of treatment given background variables

$$e(x) = Pr(W=1|X=x)$$

In detail, a propensity score indicates the probability of a unit being assigned to a particular treatment given a set of observed covariates. Balancing scores that incorporate propensity score are the most common approach

**Propensity score based sample reweighing** Propensity scores can be used to reduce selection bias by equating groups based on these covariates. Inverse propensity weighting (IPW) also named as inverse propability of treatment weighting, assigns a weight $r$ to each sample:
$$r=\frac{W}{e(x)} + \frac{1-W}{1-e(x)}$$
where $W$ is the treatment assignment ($W=1$ denotes being treated group, $W=0$ denotes the control group), and $e(x)$ denotes the propensity score.

After reweighting, the IPW estimator of ATE is:
$$\hat{ATE}_{IPW} = \frac{1}{n} \sum_{i=1}^n \frac{W_{i}Y_{i}^F}{\hat{e}(x_{i})}-\frac{1}{n}\sum_{i=1}^n\frac{(1-W_{i})Y_{i}^F}{1-\hat{e}(x_{i})}$$

However, in practice, the correctness of the IPW estimator highly relies on the correctness of the propensity score estimation, and slightly misspecification of propensity scores would cause ATE estimation error dramatically.  To handle this, Doubly Robust (DR) estimator also named as AugmentedIPW is proposed. DR estimator combines the propensity score weighting with the outcome regression, so that the estimator is robust even when one of the propensity score or outcome regression is incorrect (but not both). In detail, DR estimator is formalized as:
$$\hat{ATE}_{DR} = \frac{1}{n}\Big\{\Big[\frac{W_{i}Y_{i}^F}{\hat{e}(x)}-\frac{W_{i}-\hat{e}(x_{i})}{\hat{e}(x_{i})}\hat{m}(1,x_{i})\Big]-\Big[\frac{(1-W_{i})Y_{i}^F}{1-\hat{e}(x_{i})}-\frac{W_{i}-\hat{e}(x_{i})}{1-\hat{e}(x_{i})}\hat{m}(0,x_{i})\Big]\Big\}$$

where $\hat{m}(\cdot,x_{i})$ are the regression model estimation for group $\cdot$



### 3.2. Stratification method

### 3.3. Matching method

### 3.4. Tree-based method

### 3.5. Representation method

### 3.6. Multi-task method

### 3.7. Meta-learning method

