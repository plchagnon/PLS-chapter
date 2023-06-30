# PLS-chapter

## Very idea of latent variables:

Manifest variables are measured variables that convey information about composite variables, i.e., latent variables, that are not directly measurable. This concept grounds deeply into social sciences, where it is common practice to measure various aspects of intangible concepts such as intelligence, satisfaction, happiness... But the very idea of finding measurable proxies for less tangible concepts is common to any science (Kaplan 1946). 


## NIPALS vs SIMPLS

NIPALS stands for nonlinear iterative partial least squares and SIMPLS stands for statistically inspired modification of PLS. Other kernel-based methods have been developed to address specific cases such as datasets where observations ($N$) are much more numerous than predictors $K$, or reverse situations ($K>>N$). 

In all cases, however, PLS regression models boil down to an objective quite similar to classical, multiple linear regression models:

$$Y=\beta X + \epsilon$$

However, the way the $\beta$ coefficients are calculated is a bit different. As opposed to traditional ordinary least square methods, that will simply find the combination of coefficients that minimizes squared errors on predictions, here $\beta$ coefficients are computed using the following equation:

$$ \beta = \frac{C' W}{P'W}$$

All these three new elements ($C$, $W$ and $P$) involve latent variables:

<br>

$C$ are the weights of $Y$ on the $A$ latent variables defined

$W$ is a $K\times A$ matrix representing the **weights** of the $K$ predictors

$P$ is a $K\times A$ matrix representing the **loadings** of the $K$ predictors

<br>
<br>
********** Deflation: what is its purpose and how it differs between NIPALS and SIMPLS (NIPALS = both $X$ and $Y$ are deflated, SIMPLS = deflation of the covariance matrix $X'Y$ directly)

<br>
<br>

## Selecting the number of latent variables to model
Just like any other modeling technique, in PLS we face a trade-offs between minimizing error while providing effective predictive power. A pitfall to avoid is overfitting. In PLS regression models, the squared errors of the calibrated model will become smaller as the number of latent variables increase. However, very large numbers of latent variables do not necessarily translate into higher capabilities for the model to accurately predict new observations (i.e., ones that have not been "seen" by the model during the calibration phase) in the validation step. Typically, the number of latent variables to be kept ($A$) is determined by plotting cross-validation results for different numbers of latent variables tested. Typically, a biplot of errors on prediction in $Y$ and number of latent variables $A$ in $X$ will show that as we increase $A$, we see a decrease in prediction errors $Y$ up to a point where these errors start increasing back. This points to a "sweet spot" number of latent variables to keep in the model, and this initial exploration is essential to build the optimal PLS regression model, which optimally minimizes root squared errors during calibration while also providing efficient predictive power during validation.