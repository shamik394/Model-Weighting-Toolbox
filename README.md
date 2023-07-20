# Model-Weighting-Toolbox
Tested on Python 3.6.13
# BMA Function Definitions

*   The key idea with BMA is to combine different models to improve prediction performance.
*   BMA works by sampling different weights and then evaulating the performance of the combined model using those weights.
* The key metric for evaluation is RMSE, which is defined as $RMSE = \sqrt{\frac{1}{N}\sum_{i=1}^N (y_i-\hat{y}_i)^2}$, where $y_i$ is the $i$th observation and $\hat{y}_i=\sum_{m=1}^M w_m\hat{y}_{im}$ is the weighted average of the $M$ models' prediction values.
* By repeatedly sampling weights, one can obtain an estimate of the optimal weights according to $RMSE$.
* The best set of weights (x_{best}) as well as the posterior set of weights (x_{posterior}) are provided to the user as:
$$ (weights_{BMA,optimal},weights_{BMA,posterior}) $$.

