# PLS-chapter

## Using latent variables in statistics

Manifest variables are measured variables that convey information about composite variables, i.e., latent variables, that are not directly measurable. This concept grounds deeply into social sciences, where it is common practice to measure various aspects of intangible concepts such as intelligence, satisfaction, happiness... But the very idea of finding measurable proxies for less tangible concepts is common to any science (Kaplan 1946). We will come back to this idea of "intangible" latent variable after briefly presenting the NIPALS algorithm for PLS (and some of its variants), which will at the same time allow us to distinguish PLS regression from its close relative, the principal component regression.<br><br>

## NIPALS

Originally, PLS methods (encompassing regression, but also other applications) were developed using the NIPALS algorithm, which stands for nonlinear iterative partial least squares. This method goes beyond the PLS realm, and can be used more generally to find principal components in a data matrix. Along with singular value decomposition, it is one of the two main algorithms used to conduct principal component analysis (PCA). A short description of NIPALS will be useful here to clarify the meaning of terms like **scores**, and **loadings**, which will be exploited later on in the PLS context specifically. <br> <br>

## NIPALS for principal component analysis

In a nutshell, in order to find orthogonal principal components of a $n\times k$ matrix $X$ using NIPALS, the following steps are followed:

1. Take a random column $k$ in $X$ (most often used, but this could also be a vector of length $n$ filled with random numbers).

2. Regress every single column of $X$ against this vector $k$ using ordinary least squares (OLS). The regression coefficients (referred to as **loadings** are stored in a vector $p$, and can theoretically be calculated (we do not have to perform every single OLS regressions one by one) as $p=\frac{X'k}{k'k}$. 

3. This vector $p$ is rescaled to unit magnitude: $p=\frac{p}{\sqrt{p'p}}$

4. Regress all **rows** of $X$ against this vector $p$ to generate a new vector $k$ (of length $K$) which is said to contain **scores**, and again, can be calculated in a single step equation as $k=\frac{Xp}{p'p}$. 

At this stage, one can clearly see that $k$ and $p$ are inter-dependent and can be calculated from one another. This "circularity" allows the algorithm to optimize values (**loadings** and **scores**) iteratively. In other words, we can re-run steps 2-4 and update values in *p* and *k* up until very little change is observed in these vectors (i.e., when changes fall below a pre-defined threshold). Typically this takes not much more than 200 iterations.
 
Upon convergence, the **loadings** and **scores** of the first component can be extracted from $p$ and $k$. We can move on to extract the same **loadings** and **scores** from the subsequent (i.e., 2<sup>nd</sup>, 3<sup>rd</sup>, ...) components, simply by redefining $X$ as $X=X-tp'$ and re-doing steps 1-4 above. In other words, at every step/component, we work with residuals from the previous step, in such a way that successive components are always orthogonal, and thus independent, from each other. This process is typically referred to as *deflation*. <br> <br>

## NIPALS for PLS

In the presentation of NIPALS above applied to a single matrix, the sole objective was to identify principal orthogonal components to that matrix. In PLS regression, we work with two matrices: dependent variables in $Y$ (or a single 1-column data matrix if we only have 1 dependent variable) and predictors in $X$. The goal then becomes threefold: (1) identify principal components for $Y$, (2) identify principal components for $X$ and (3) maximize the relationship between the two. Briefly, we can outline the NIPALS algorithm for PLS as follows:

1. As in the general NIPALS algorithm for PCA, we regress every column of $X$ against a vector. However, here, this vector is neither a set of random numbers, of a column of $X$, but a column of $Y$, which we can note here $u$. This provides us with a vector of **loadings**, or weights ($w$) calculated as follows: $w=\frac{1}{u'u}\cdot X'u$

2. $w$ is normalized to unit length such that $w=\frac{w}{\sqrt{w'w}}$

3. Rows of $X$ are regressed against $w$. This provides a **scores** vector $t$ defined as: $t=\frac{1}{w'w}\cdot Xw$

4. These **scores** calculated for $X$ are used to calculate **loadings** for $Y$, stored in a vector $c$, such that $c=\frac{1}{t't}\cdot Y't$.

5. As for $X$, **scores** for $Y$ are stored in a new vector $u$ and calculated as $u=\frac{1}{c'c}\cdot Yc$.

These steps above are repeated, updating sequentially values in $w$, $t$, $c$ and $u$ until convergence. These relate to $X$ loadings and scores, and $Y$ loadings and scores, respectively, for the first component (here, **latent variable**). Then follows the deflation process, that is, the removal from $X$ and $Y$ of the variability already explained by this first latent variable. For $X$, this is done by calculating new loadings $p$ from $X$ scores in $t$: $p=\frac{1}{t't}\cdot X't$. One will note this is exactly how we calculated loadings of $Y$ above, although here we perform the calculation with the $X$ matrix. This is done to calculated predicted values of $X$ based on the first component (latent variable) extracted above. We calculate predicted $X$ values as : $\hat{X}=tp'$. For the second component (latent variable), we will now simply used the residuals, $X-\hat{X}$ as our $X$-space. In a similar manner, $\hat{Y}=tc'$, and the residuals ($Y-\hat{Y}$) are used for the next component.

One additional note we might want to make here is that we generally define a weight matrix $R$ such that loadings for $X$ actually relate to the original matrix $X$. If you recall the process above, the $X$ loadings stored in vectors $w$ during the iteration process only relate to the original $X$ matrix when looking at the first component. Loadings from the second component actually refer to the residual variability of $X$ left unexplained by the first component. This is thus the matrix $R$ that should be used when interpreting relationships between predictors in $X$ and principal components or latent variables. This matrix is calculated as: $R=\frac{W}{P'W}$, where $W$ and $P$ are the matrices storing the $w$ and $p$ loadings and scores calculated sequentially from the different principal components during model computation.

One additional feature that might be of interest when interpreting the model is to evaluate how much variability is explained by each of our principal components. This can be done by comparing the variance in the residuals for the $i^{th}$ component with the variance of the initial $X$ matrix. As the algorithm goes on, progressively less and less variance in $X$ will be left unexplained, and $R^2$ will increase. The same can be done for $Y$. These $R^2$ values can also be computed column by column, for both $X$ and $Y$, such that we can look specifically at linkages between specific predictors in $X$, or predicted variables in $Y$, and principal components individually. <br><br>

## Variants of NIPALS

A few words can be said on NIPALS variants that were developed to increase computational speed or to address specific case studies. For example, SIMPLS (which stands for statistically inspired modification of PLS), is a faster alternative to NIPALS that proceeds slightly differently during the deflation process. Indeed, it deflates the crossproduct matrix of $X$ and $Y$ as opposed to deflating $X$ and $Y$ individually as we did for NIPALS. Other kernel-based methods have been developed to address specific cases such as datasets where observations ($N$) are much more numerous than predictors $K$. Typically, the choice of the algorithm will have only minor influence of the model quality. This is especially true when $Y$ contains a single column (dependent variable), as is often the case, for example, in soil spectroscopy.  <br><br>

## Selecting the number of latent variables to include
Just like any other modeling technique, in PLS we face a trade-offs between minimizing error while providing effective predictive power. A pitfall to avoid is overfitting. In PLS regression models, the squared errors of the calibrated model will become smaller as the number of latent variables increase. However, very large numbers of latent variables do not necessarily translate into higher capabilities for the model to accurately predict new observations (i.e., ones that have not been "seen" by the model during the calibration phase) in the validation step. Typically, the number of latent variables to be kept ($A$) is determined by plotting cross-validation results for different numbers of latent variables tested. Typically, a biplot of errors on prediction in $Y$ and number of latent variables $A$ in $X$ will show that as we increase $A$, we see a decrease in prediction errors $Y$ up to a point where these errors start increasing back. This points to a "sweet spot" number of latent variables to keep in the model, and this initial exploration is essential to build the optimal PLS regression model, which optimally minimizes root squared errors during calibration while also providing efficient predictive power during validation. <br><br>

## Relationship with principal component regression (PCR)
There are many similarities between PCR and PLS, the first one being the usefulness of these methods when dealing with high-dimensionality datasets containing colinear predictors. Both methods extract new intangible variables (principal components of latent variables) from the data. However, the information exploited to do so is where these techniques differ the most. For PCR, a PCA is run on the predictors $X$ such that this step is fully blind of the dependent variable(s) in $Y$. This means that the sole purpose of the principal components we uncover during the PCA is to best represent variability in $X$, now into a set of successive orthogonal axes. Data in $Y$ only come in second wave, where the scores vectors, associated with each component of the PCA, become predictors in a classical multiple linear regression. 

In PLS regression, the very definition of principal components (latent variables) already involves the dependent variable(s) in $Y$. Latent variable definition involve the intrinsic structure in $X$- space, the intrinsic structure in $Y$- space and the **relationship between $X$ and $Y$**. 
