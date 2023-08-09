# Model-Weighting-Toolbox
Tested on Python 3.9

The user should install the packages specified in the requirements.txt file.

# File Descriptions

* The file "tasmax_moddat1(smaller) (1).csv" is the second example in the toolbox and is a contemporary scientific sample that has already been built into the toolbox.
* The file " Model_Weighting_Toolbox_(cleaner_version)(2).ipynb" is the toolbox.

# BMA Function Definitions

*   The key idea with BMA is to combine different models to improve prediction performance.
*   BMA works by sampling different weights and then evaluating the performance of the combined model using those weights.
* The key metric for evaluation is RMSE, which is defined as
```math
RMSE=\sqrt{\frac{1}{N}\sum_{i=1}^N(y_i-\hat{y}_i)^2}
```
* By repeatedly sampling weights, one can obtain an estimate of the optimal weights according to $RMSE$.

# Calculating independence for BMA (for lines #51-53)

* Please visit the documentation for the corresponding title in the toolbox.

# AIC Definition

AIC can also be used to weight different models. AIC captures some notion of model fit, so it makes sense to use this information to weight higher the models that fit the data the best. 

Please visit the documentation for the corresponding title in the toolbox.

## The Sanderson Approach for both skill and independence

* Please visit the documentation for the corresponding title in the toolbox.
