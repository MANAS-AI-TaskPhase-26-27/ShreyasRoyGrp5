
- Implemented on non linear data distributions.
- Advantage of Decision Trees as opposed to Regression:
    -  Linear and logistic regression create linear or parametric decision boundaries, making them struggle with *complex, non linear data distributions* unless we perform extensive manual feature engineering. 
    - Decision trees naturally handle non linear relationships by **recursively partitioning** the feature space into rectangular regions using axis aligned splits. 
    - They are intuitive, highly interpretable (act like a "20 questions" flowchart), require minimal feature scaling, and can natively handle both numerical and categorical variables.
    
- It is a Greedy, Top Down, Recursive Partitioning Algorithm. Top down because we start with the overall region and split downward. Greedy because at each step we pick the single best partition without looking ahead. Recursive because each child region is treated as a new independent partitioning problem.

- Our purpose is to partition the feature space into distinct regions so that we can isolate and purify our target distributions.
- We have a Region $R_p$ and we look for a split $S_p$ , such that $S_p(j, t) = (R_1: \{X \mid X_j < t, X \in R_p\}, R_2: \{X \mid X_j \ge t, X \in R_p\})$; where $j \rightarrow$ feature number and $t \rightarrow$ threshold.
- To choose splits, we define a Loss Function. We define $\hat{p}(c)$ as the proportion of examples in $R$ that are of class $c$. Our goal is to maximize the information gain (minimize children loss relative to parent loss).
- Loss $= 1 - \hat{p}(c)$; $\max_{j,t} (L(R_p) - (L(R_1) + L(R_2)))$; $L(R_p)$ (parent loss) is fixed, so we try to minimize children loss ($L(R_1) + L(R_2)$) instead.

- Why misclassification error is inconvenient to use for decision trees:
    - Misclassification loss measures the fraction (or absolute count) of incorrectly predicted examples in a region ($1 - \max_c \hat{p}_c$). It is inconvenient and insensitive because it does not change smoothly when data distributions shift within the same majority class. 
    - If a split changes the distribution of minority points without flipping the majority class label, misclassification loss remains completely unchanged, making it blind to improvements in region purity.
    
- Cross entropy loss and its benefit over misclassification loss:
    - Cross entropy loss (derived from information theory, measuring the expected bits needed to transmit class labels) is defined as: $\sum_{c} \hat{p}_c \log_2(\hat{p}_c)$. 
    - We use it over misclassification loss because it is strictly sensitive to changes in class probabilities and purity. 
    - Unlike misclassification loss, cross-entropy provides a smooth gradient of improvement, guiding the greedy algorithm to find cleaner partitions.
    
- **Formula for cross entropy loss:**
	 $L_{\text{cross}} = -\sum_{c=1}^{C} \hat{p}_c \log_2 \hat{p}_c$
    
- Geometric interpretation of cross entropy loss:
    - Geometrically, cross entropy loss plotted against class proportion $\hat{p}$ is a strictly concave curve. 
    - When we split a parent region into two child regions with different class proportions, the average loss of the children (represented by a "chord" connecting points on the curve) always lies _below_ the parent's loss curve. This vertical distance represents the information gain, showing graphically why any purity increasing split is favored.
    
- Comparison between cross entropy loss or gini loss:
    - Both Cross Entropy and Gini Impurity ($\sum \hat{p}_c (1 - \hat{p}_c)$) produce very similar strictly concave curves and perform comparably in practice. Gini impurity is slightly faster to compute because it avoids logarithmic calculations, while cross-entropy can sometimes be more sensitive to purer nodes. In practice, either works well, and choice is usually a matter of library defaults (e.g., CART uses Gini, ID3/C4.5 use Entropy).
    
- Benefit for curves in Decision Trees to be concave:
    - Impurity measures are concave because uncertainty is highest when classes are evenly mixed (50/50) and lowest when pure (0% or 100%). The benefit of a strictly concave curve (via Jensen's Inequality) is that any split that separates mixed data into purer subsets guarantees that the weighted average loss of the children will be lower than the parent loss, ensuring positive information gain.

- Reason for **not** normalizing features in Decision Trees:
	- Unlike Logistic Regression or Gradient Descent (which rely on continuous weights, magnitudes, and cost function gradients), decision trees are *non parametric* and rely purely on *sorting and thresholds*.

	- **Why scaling doesn't affect trees:** If we have a feature like age ranging from 0 to 100, and you decide to normalize it to a mean of 0 and standard deviation of 1, the _relative order_ of the data points does not change. The algorithm will simply find a different threshold number $t$ (e.g., instead of splitting at age <= 30, it might split at age <= -0.45), resulting in the **exact same tree splits and predictions**.


### Regression Trees

- Instead of predicting a majority class, we predict the mean of the target values in that region.
    
- **Formula for $\hat{y}_m$ (Mean of $y_i$ over $R_m$):**
    
    - _Answer / Addition:_ $\hat{y}_m = \frac{1}{\vert{}R_m\vert{}} \sum_{i \in R_m} y_i$
        
- **Loss_Squared formula:**
    
    - _Answer / Addition:_ $L_{\text{squared}} = \sum_{i \in R_m} (y_i - \hat{y}_m)^2$
        
- How to actually search for these splits:
	  brute force across possible thresholds, which is efficient here.
    
- Too many categories can vastly increase the number of possible splits (for $n$ categories, we would have $2^n$ possible splits), so we try to keep them minimal. For binary classification with categorical features, we can sort categories by the proportion of positive examples in each and search through them linearly (reducing complexity from $2^Q$ to $Q$).
    

### Regularization of Decision Trees

- **Minimum leaf size** (leaf size $\rightarrow$ minimum number of examples in each region).
    
- **Maximum depth**.
    
- **Maximum number of nodes. What do nodes mean here?**
    
    - _Answer:_ "Nodes" refer to the total count of internal decision split nodes and terminal leaves in the entire tree. Limiting maximum nodes caps the overall capacity of the tree to prevent overfitting.
        
- **Minimum decrease in loss.** _(This seems effective, but in practice this leads us to stop our algorithm too early because it misses beneficial multi-step interactions where an initial suboptimal split enables a great subsequent split)._
    
- **Pruning:** After growing the entire tree, we take a validation set and calculate the misclassification error on the validation set for each leaf/node we might remove, pruning backwards.
    

### Runtime

- Consider we have $n$ examples with $f$ features, with $d$ as the depth of our tree. The test time would be $O(d)$. Typically $d < \log_2(n)$ for balanced trees.
    
- **Each point after splitting would be a part of $O(d)$ nodes.**
    
    - "Each point belongs to $O(d)$ nodes" means that as a tree grows to depth $d$, any single data point passes through $d$ ancestor nodes from the root down to its leaf. At *each* node along its path, evaluating or sorting features takes $O(f)$ time. Across all $n$ data points and $d$ nodes, the total training time complexity is $O(n \cdot f \cdot d)$, which simplifies to $O(nf \log_2 n)$ for a balanced tree where $d \approx \log_2 n$.
        

### Cons

- Decision trees often struggle with features that act additively with one another (e.g., diagonal decision boundaries like $x_1 = x_2$ require an inefficient staircase of axis-aligned splits; linear regression handles this easily).
    
- High variance, making them prone to overfitting.
    
- Because of the first two cons, single decision trees often have lower predictive accuracy on complex tasks.
    

### Ensembling

- **What is ensembling and how does it improve upon decision trees?**
    
    - Ensembling combines multiple individual models (weak learners) to produce a single, robust prediction. It improves upon decision trees by drastically reducing variance (via bagging/Random Forests) or bias (via boosting/Gradient Boosting), turning unstable single trees into high-performance models.
        
- **Why are we dropping the independence assumption?**
    
    - Real-world models trained on the same dataset are never truly independent; they exhibit correlation ($\rho > 0$). Dropping the i.i.d. assumption reveals the true ensemble variance formula: $\text{Var}(\text{mean}) = \rho \sigma^2 + \frac{1-\rho}{M}\sigma^2$. This shows that to minimize variance, we must increase the number of models ($M$) _and_ decrease their correlation ($\rho$).
        
- We want a larger number of models ($M$), which are optimally decorrelated (lowering $\rho$).
    
- Ways to ensemble:
    
    - Different algorithms (SVM, trees, etc.).
        
    - Different training sets.
        

### Bagging (Bootstrap Aggregation)

- **Consider a true population $P$ (What is population and what do we mean by 'true' here?):**
    
    - The "true population $P$" refers to the theoretical, complete underlying distribution of all possible data points in the real world. Since we never have access to the entire population, we use our training set $s$ as an empirical sample. Bootstrapping treats $s$ as the population, allowing us to draw new samples (with replacement) to simulate having multiple independent training sets.
        
- **Bootstrapping reduces $\rho$ in bias-variance analysis, but slightly increases bias due to random subsampling (What is bias here and how is it increased?):**
    
    - "Bias" refers to the model's systematic error in capturing the true pattern. Bias increases slightly during bagging because each bootstrap sample $z$ contains only about $\frac{2}{3}$ of the original training set $s$ (due to sampling with replacement). Training on smaller subsets makes each individual tree slightly simpler/less complex, raising bias slightly—though this is heavily outweighed by the reduction in variance.
        
- **What is the lower and upper bound of how much we can decrease $\rho$ using bootstrapping?**
    
    - While there is no strict mathematical lower bound formula, intuitively $\rho$ cannot drop to zero because all bootstrap samples $z$ are drawn from the same training set $s$ (sharing ~67% identical data points on average). This caps how much $\rho$ can be reduced through bootstrapping alone, motivating Random Forests (which introduce random _feature_ subsets to further decorrelate trees).
        
- **Random Forests:** Recall that decision trees are high-variance and low-bias, making them an ideal fit for bagging. At each split, consider only a fraction of your total features. We want to decrease $\rho$ and decrease correlations between models (done implicitly by restricting available features at each split).
    

### Boosting

- **Unlike bagging which decreases variance, boosting is done to decrease bias. It is additive in nature:**
    
    - "Additive in nature" means that instead of averaging independent models (like bagging), boosting builds models sequentially. Each new model ($G_m$) is trained specifically to correct the residual errors/mistakes of previous models, and the final prediction is a weighted sum: $G(x) = \sum \alpha_m G_m(x)$.
        
- **We determine a weight $\alpha_m$ for every classifier $G_m$, which is proportional to $\log\left(\frac{1 - \text{err}_m}{\text{err}_m}\right)$ :**
    
    - The weight $\alpha_m = \frac{1}{2} \ln\left(\frac{1 - \text{err}_m}{\text{err}_m}\right)$ originates from AdaBoost's mathematical derivation (minimizing exponential loss). It assigns higher voting weight to classifiers with lower error rates ($\text{err}_m < 0.5$) and negative/low weight to high-error classifiers. Each $G_m$ is then trained on a reweighted training set. This is called AdaBoost.
        

### Feature Importance & Out-of-Bag (OOB) Error Estimation

- **Feature Importance:** In decision trees and random forests, feature importance measures how much a given feature reduces impurity (e.g., Gini impurity or cross-entropy) averaged across all trees in the forest. Features used at the top of trees or across many trees receive higher importance scores, helping with model interpretability and feature selection.
    
- **Out-of-Bag (OOB) Error Estimation:** Because bagging samples ~67% of the training data with replacement for each bootstrap sample $z$, roughly 33% of the training examples are left out ("out-of-bag") for any given tree. We can pass these OOB examples through the trees that didn't see them during training to calculate an unbiased validation error without needing a separate validation set or cross-validation.


