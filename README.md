# Model-Weighting-Toolbox
Tested on Python 3.6.13

# BMA Function Definitions

*   The key idea with BMA is to combine different models to improve prediction performance.
*   BMA works by sampling different weights and then evaluating the performance of the combined model using those weights.
* The key metric for evaluation is RMSE, which is defined as
```math
RMSE=\sqrt{\frac{1}{N}\sum_{i=1}^N(y_i-\hat{y}_i)^2}
```
* By repeatedly sampling weights, one can obtain an estimate of the optimal weights according to $RMSE$.

# Calculating independence for BMA (for lines #51-53)

* More recently, weighting based on model independence has been an additional criterion to consider alongside model skill. This consideration of model independence has emerged due to models having common bases of model structure, parameterizations, and associated programming code, all of which can result in a lack of independence between climate models. Here, independence information is estimated in the post-processing of the model weights, and we determine model independence information using the posterior distribution of the BMA weights. To quantify independence here, we take the sum of the correlation matrix of the posterior samples and subtract 1 from this. This results in a negative independence score for models that have independent posterior samples and a positive independence score for models that have dependent posterior samples.

# AIC Definition

AIC can also be used to weight different models. AIC captures some notion of model fit, so it makes sense to use this information to weight higher the models that fit the data the best. 

Please visit the documentation for the corresponding title in the nb.

## The Sanderson Approach for both skill and independence

* Models differ in their skill to simulate observations, and there is an inherent lack of independence between models.
*There are weighting strategies in the literature for use with climate model ensembles, which consider both model performance skill as well as the interdependency of models that arises from common development and calibration practices of the models.
*These strategies are used to estimate a model average from several ensemble members that were simulated from dependent models of varying skill.
*In this context, we use the RMSE as a representation of skill, however, users can create and use different options for this metric.
*This score is used as an input to develop our skill metric weights. We follow a similar recipe as Sanderson et al. (2015, 2017) and estimate model weights using
the skill metrics with:

$$
w_{m,Skill(i)} = A * e^{ - ( \frac{ \delta_{i,(obs)}}{Dq} )^2 }
$$

* where A is a normalizing constant such that the sum of all the weights is equal to 1, δi(obs) is the “Median Skill” score for each model, and Dq is the radius of model quality, which determines the degree to which models with poor skill should be down‐weighted. A very small value of Dq will allocate a large fraction of weight to the single best‐performing model. Equally, as Dq approaches infinity, the model average will include more information from the non-skill-weighted models. For our study, we use a value of Dq = 0.9
* The independence scores for each model can be calculated using the intermodel distances, which are a function of the RMSE between each of the models. The intermodel distance matrix is constructed with the RMSE values, and the similarity scores, S(δi,j), are then calculated for each pair of models i and j using this distance matrix. This score is calculated as follows:

$$ 
S(\delta_{i,j}) = e^{ - ( \frac{ \delta_{i,j}}{Du} )^2 }
$$

* where δi j is the RMSE values from the intermodel distance matrix and Du is the radius of similarity (Sanderson et al., 2015), which is a free parameter that determines the distance scale over which models should be considered similar and thus down‐weighted for co‐dependence. We choose a value of Du = 0.5, which is shown by Sanderson et al. (2015), to be an appropriate value. In theory, two identical models will produce a similarity score of S(δi,j) = 1, and this score approaches 0 as the distance between the models grows infinitely.

* The independence scores, wu(i), are calculated using the similarity scores for each model in the following manner:

$$
w_u(i) = \left[ 1 + \sum_{j\neq i}^n S(\delta_{i,j}) \right]^{-1}
$$ 

* where n is the total number of models. Finally, the equation used to combine the uniqueness and skill information to create the Sanderson set of model weights is:

$$ 
weights_{Sanderson}(i) = A * w_{m,Skill(i)} * w_{u}(i)
$$

* where A is a normalizing constant such that the sum of all the weights is equal to 1.


