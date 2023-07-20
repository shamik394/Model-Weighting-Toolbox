# Model-Weighting-Toolbox
Tested on Python 3.6.13
# BMA Function Definitions

*   The key idea with BMA is to combine different models to improve prediction performance.
*   BMA works by sampling different weights and then evaulating the performance of the combined model using those weights.
* The key metric for evaluation is RMSE, which is defined as $RMSE = \sqrt{\frac{1}{N}\sum_{i=1}^N (y_i-\hat{y}_i)^2}$, where $y_i$ is the $i$th observation and $\hat{y}_i=\sum_{m=1}^M w_m\hat{y}_{im}$ is the weighted average of the $M$ models' prediction values.
* By repeatedly sampling weights, one can obtain an estimate of the optimal weights according to $RMSE$.
* The best set of weights (x_{best}) as well as the posterior set of weights (x_{posterior}) are provided to the user as:
$$ (weights_{BMA,optimal},weights_{BMA,posterior}) $$.

# Calculating independence for BMA (for lines #51-53)

* More recently, weighting based on model independence has been an additional criterion to consider alongside model skill. This consideration of model independence has emerged due to models having common bases of model structure, parameterizations, and associated programming code, all of which can result in a lack of independence between climate models. Here, independence information is estimated in the post-processing of the model weights, and we determine model independence information using the posterior distribution of the BMA weights. To quantify independence here, we take the sum of the correlation matrix of the posterior samples and subtract 1 from this. This results in a negative independence score for models that have independent posterior samples and a positive independence score for models that have dependent posterior samples.

# AIC Definition

AIC can also be used to weight different models. AIC captures some notion of model fit, so it makes sense to use this information to weight higher the models that fit the data the best. The formula for AIC is $AIC = -2 \hat{L} + 2 p$, where $\hat{L}$ is the the estimated likelihood from the model and $p$ is the number of parameters in the model.

When used for weighting, a typical approach is to use the weight

$$
weights_{AIC} = \frac{e^{-1/2 \cdot AIC_m}}{\sum_{m=1}^M e^{-1/2 \cdot AIC_m}}
$$

for the $m$th model. This way, the weights add to one, and can be used to calculate a weighted average of the predictions of each model, just like with BMA: $\hat{y}_i=\sum_{m=1}^M w_m\hat{y}_{im}$.

The difference between this approach (AIC) and BMA is that BMA samples and tests the performance of different weights to see which weights are optimal. Whereas AIC simply uses the fit of each model to determine the weights.
