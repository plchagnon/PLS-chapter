# PLS-chapter

## Very idea of latent variables:

Manifest variables are measured variables that convey information about composite variables, i.e., latent variables, that are not directly measurable. This concept grounds deeply into social sciences, where it is common practice to measure various aspects of intangible concepts such as intelligence, satisfaction, happiness... But the very idea of finding measurable proxies for less tangible concepts is common to any science (Kaplan 1946). 


## NIPALS vs SIMPLS

NIPALS stands for nonlinear iterative partial least squares and SIMPLS stands for statistically inspired modification of PLS. Other kernel-based methods have been developed to address specific cases such as datasets where observations ($N$) are much more numerous than predictors $K$, or reverse situations ($K>>N$). 

In all cases, however, PLS regression models boil down to an objective quite similar to classical, multiple linear regression models:

$$Y=\beta X + \epsilon$$

However, the way the $\beta$ coefficients are calculated is a bit different. As opposed to traditional ordinary least square methods, that will simply find the combination of coefficients that minimize squared errors on predictions, here $\beta$ coefficients are computed through matrix algebra:

$$ \beta = \frac{C' W}{P'W}$$

All these three new elements ($C$, $W$ and $P$) involve latent variables. 