### Model-Weighting-Toolbox

Tested on Python 3.6.13

### BMA Function Definitions

* The key idea with BMA is to combine different models to improve prediction performance.
* BMA works by sampling different weights and then evaulating the performance of the combined model using those weights.
* The key metric for evaluation is RMSE, which is defined as $$RMSE = \sqrt{\frac{1}{N}\sum_{i=1}^N (y_i-\hat{y}_i)^2}$$
    * Where $y_i$ is the $i$th observation and $$\hat{y}_i=\sum_{m=1}^M w_m\hat{y}_{im}$$ 
    * is the weighted average of the models' prediction values.




* By repeatedly sampling weights, one can obtain an estimate of the optimal weights according to RMSE
* The best set of weights ($x_{\text{best}}$) as well as the posterior set of weights ($x_{\text{posterior}}$) are provided to the user as: 

$$ (\text{weights}_{\text{BMA,optimal}},\,\text{weights}_{\text{BMA,posterior}})$$

### Calculating independence for BMA (for lines #51-53)

More recently, weighting based on model independence has been an additional criterion to consider alongside model skill. This consideration of model independence has emerged due to models having common bases of model structure, parameterizations, and associated programming code, all of which can result in a lack of independence between climate models. Here, independence information is estimated in the post-processing of the model weights, and we determine model independence information using the posterior distribution of the BMA weights. To quantify independence here, we take the sum of the correlation matrix of the posterior samples and subtract 1 from this. This results in a negative independence score for models that have independent posterior samples and a positive independence score for models that have dependent posterior samples.

### AIC Definition

AIC can also be used to weight different models. AIC captures some notion of model fit, so it makes sense to use this information to weight higher the models that fit the data the best. The formula for AIC is $$\text{AIC} = -2\hat{L} + 2p$$ where $\hat{L}$ is the the estimated likelihood from the model and $p$ is the number of parameters in the model.

When used for weighting, a typical approach is to use the weight $$\text{weights}_{\text{AIC}} = \frac{e^{-\frac{1}{2}\text{AIC}_m}}{\sum_{m=1}^M e^{-\frac{1}{2}\text{AIC}_m}}$$

for the 
$$m\hat{y}_i=\sum_{m=1}^M w_m\hat{y}_{im}$$.

The difference between this approach (AIC) and BMA is that BMA samples and tests the performance of different weights to see which weights are optimal. Whereas AIC simply uses the fit of each model to determine the weights.

### The Sanderson Approach for both skill and independence

* Models differ in their skill to simulate observations, and there is an inherent lack of independence between models. 
    * There are weighting strategies in the literature for use with climate model ensembles, which consider both model performance skill as well as the interdependency of models that arises from common development and calibration practices of the models.
    * These strategies are used to estimate a model average from several ensemble members that were simulated from dependent models of varying skill.
    * In this context, we use the RMSE as a representation of skill, however users can create and use different options for this metric. 
    * This score is used as an input to develop our skill metric weights. We follow a similar recipe as Sanderson et al. (2015, 2017) and estimate model weights using the skill metrics with:
    
    $$w_{m,\text{Skill}(i)} = Ae^{- \big (\frac{\delta_{i,\text{(obs)}}}{D_q} \big )^2}$$

* Where $A$ is a normalizing constant such that the sum of all the weights is equal to 1, $\delta_{i,\text{(obs)}}$ is the “Median Skill” score for each model, and $D_q$ is the radius of model quality, which determines the degree to which models with poor skill should be down‐weighted. A very small value of $D_q$ will allocate a large fraction of weight to the single best‐performing model. Equally, as $D_q$ approaches infinity, the model average will include more information from the non‐skill weighted models. For our study, we use a value of $D_q = 0.9$

* The independence scores for each model can be calculated using the intermodel distances, which are a function of the RMSE between each of the models. The intermodel distance matrix is constructed with the RMSE values, and the similarity scores, $S(\delta_{i,j})$, are then calculated for each pair of models $i$ and $j$ using this distance matrix. This score is calculated as follows:

$$S(\delta_{i,j}) = e^{-\big ( \frac{\delta_{i,j}}{D_u} \big )^2}$$

* Where $\delta_{i,j}$ are the RMSE values from the intermodel distance matrix and $D_u$ is the radius of similarity (Sanderson et al., 2015), which is a free parameter that determines the distance scale over which models should be considered similar and thus down‐weighted for co‐dependence. We choose a value of $D_u = 0.5$, which is shown in Sanderson et al. (2015), to be an appropriate value. In theory, two identical models will produce a similarity score of $S(\delta_{i,j}) = 1$, and this score approaches 0 as the distance between the models grows infinitely.

* The independence scores, $w_u(i)$, are calculated using the similarity scores for each model in the following manner:

$$w_u(i) = \bigg \{ 1 + \sum_{j\neq i}^n S(\delta_{i,j}) \bigg \}^{-1}$$

* Where $n$ is the total number of models. Finally the equation used to combine the uniqueness and skill information to create the Sanderson set of model weights is

$$\text{weights}_{\text{Sanderson}}(i) = Aw_{m,\text{Skill}(i)}w_u(i)$$

* Where $A$ is a normalizing constant such that the sum of all the weights is equal to 1