# Causal Analysis: Effects of Nursing Home Facilities on Health Inspection Rating

[![Python 3.7](https://img.shields.io/badge/python-3.7-blue.svg)](https://www.python.org/downloads/release/python-380/)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-no-red.svg)](https://github.com/jonahwinninghoff/Springboard/graphs/commit-activity)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

**[Overview](#overview)** | **[Findings](#findings)** | **[Method](#method)** | **[Datasets](#data)** | **[Statistics](#statistics)** | **[Wrangling](#wrangling)** | **[Modeling](#model)** | **[Econometrics](#econ)** | **[Insights](#insights)** | **[Future](#future)** | **[References](#refer)**

See the presentation ([PPT](https://github.com/jonahwinninghoff/Springboard-Capstone-Project-2-Causal-Analysis-Nursing-Home-Facilities/raw/main/Documentation/home_nurse.pptx) or [PDF](https://github.com/jonahwinninghoff/Springboard-Capstone-Project-2-Causal-Analysis-Nursing-Home-Facilities/blob/main/Documentation/home_nurse.pdf))

See the [complete report](https://github.com/jonahwinninghoff/Springboard-Capstone-Project-2-Causal-Analysis-Nursing-Home-Facilities/raw/main/Documentation/Report.pdf)


## OVERVIEW <a id='overview'></a>

<p align="justify"> Established by the United States Centers for Medicare and Medicaid Services (CMS) in 2009, the star rating is a system that helps family members make decisions in which nursing home their senior family members would reside. The decision process of choosing the right facility is not simple. With the COVID-19 pandemic still happening, the system fails largely due to a lack of data audit and self-report bias. For example, the hypothesis testing is unable to confirm that the number of COVID-19 deaths at five-star facilities is different from one-star facilities on a significance level (Silver-Greenberg and Gebeloff, 2021). The stipulation in this study is that the CMS institution may face a massive decline in public trust. For that specific reason, the causal analysis is vital to addressing this problem.</p>

## KEY FINDINGS <a id = 'findings'></a>

<p align="justify">The analysis shows that based on the health inspection rate during the COVID-19 pandemic, licensed practical nurses do well. However, when licensed nurses spend at least one hour with the residents, the chances of a good health inspection rating decline. Another important finding is that having at least one family member involved in the decision-making process boosts positive health inspection ratings. However, family involvement is not common. </p>

## METHOD <a id='method'></a>

<p align ='justify'> With attempts to establish a causal relationship, two different frameworks will be used: data science and econometrics. The data science approach begins by locating patterns from the data to test the model against the existing data. The role of econometrics is reversed. For example, the econometric approach begins by writing a causal model of economic behavior and its underlying assumptions, followed by determining whether the available data fits in the causal model.</p>

## DATASETS <a id ='data'></a>

<p align = 'justify'> Two datasets obtained from CMS databases are <i> Minimum Data Set </i> (MDS) <i> Quality Measures </i> and <i>Provider Information </i> datasets. The MDS dataset contains over 15,000 different providers from 50 states, including the District of Columbia. The target variable is the measure quality score. No variables hold predictive power for measuring quality scores. Some features are useful for statistical insights. The second dataset contains more than 80 features with at least 15,000 entities. At least 70 features are usable for prediction.</p>

- [MDS Quality Measures](https://github.com/jonahwinninghoff/Springboard_Capstone_Project/blob/main/Assets/NH_QualityMsr_MDS_Jun2021.csv.zip?raw=true)
- [Provider Information](https://github.com/jonahwinninghoff/Springboard_Capstone_Project/blob/main/Assets/NH_ProviderInfo_Aug2021.csv.zip?raw=true)

## STATISTICAL INFERENCE <a id ='statistics'></a> 

<p align = 'justify'> The measured quality in the MDS dataset is in use to describe the overall rating of what each facility does well associated with every measure code. The measure code is associated with a measure description explaining how the score is calculated. As indicated by the complete data quality report, the score is not normally distributed. The Empirical Cumulative Distribution Function (ECDF) tool is undertaken to compare empirical distribution with theoretical  <i>Beta</i> distribution and determine if the empirical distribution is parametric. The alpha and beta for the theoretical distribution are unknown. The identifications for both parameters are:</p> 

<div align = 'center'> <img src="https://latex.codecogs.com/svg.image?\hat\alpha&space;=&space;\bar&space;x&space;\left[\frac{\bar&space;x&space;(1-&space;\bar&space;x)}{s^2&space;-&space;1}\right]" title="\hat\alpha = \bar x \left[\frac{\bar x (1- \bar x)}{s^2 - 1}\right]" />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src="https://latex.codecogs.com/svg.image?\hat\beta&space;=&space;(1-&space;\bar&space;x)&space;\left[&space;\frac{\bar&space;x&space;(1-&space;\bar&space;x)}{s^2-1}&space;\right]&space;" title="\hat\beta = (1- \bar x) \left[ \frac{\bar x (1- \bar x)}{s^2-1} \right] " /> </div>

<p align = 'justify'>Because the continuous score is ε(0,100) and cross-sectional, the continuous version of binomial called <i>Beta</i> distribution is used. Both parameters help build the theoretical distribution by using the random generator (Sinharay, 2010). As the result shows, the empirical distribution is not consistent with the theoretical distribution. The shape of the distribution is unlikely to change due to the Law of Large Numbers. The adjusted density plot is a demonstration.</p>

![image](https://user-images.githubusercontent.com/52142917/137194232-c0272480-efe7-4d39-bb61-7b11cae9b590.png)

<p align = 'justify'>The permutation test can be used instead for testing the null hypothesis. This hypothesis is that two groups are within the same distribution. Since this hypothesis tests against 13 different measure codes, the alpha level with Bonferroni correction is 0.38%. The purpose of Bonferroni correction is to ensure that the chance of Type I Error is minimal.</p>

![image](https://user-images.githubusercontent.com/52142917/137818719-e8b1fa63-c4f8-4104-b529-1f414e4632e7.png)

<p align = 'justify'> The expected difference is by averaging scores tied with a particular measure code and subtracting by these without the code. As indicated by the plot, the confidence interval is difficult to see due to small standard errors. The margins of errors are between 0.001 and 0.002. As the plot shows, the expected difference for catheter-related measure code is lower than -40, while the expected difference for depressive-related measure code is higher than 60. Intuitively, the facilities are likely to perform poorly when treating residents who have catheters inserted and left in their bladders. The facilities do better with treating residents with depressive symptoms. However, this plot is not established with a causal relationship due to self-report bias (CMS, 2021).</p>

## DATA WRANGLING <a id ='wrangling'></a>

<p align = 'justify'> As mentioned earlier, the <i> Provider Information </i> dataset has many features. The challenge with this dataset is that several features are redundant (some are perfectly correlated). The wrangling process requires automation techniques to identify redundancies and leakages. The leakage refers to when the features in the training process do not exist when integrating the production, which, in turn, causes the predictive scores to overestimate. This is a common mistake in the data science field. For example, the total weighted health survey score as a feature predicting the health inspection rating is a form of leakage. The final process is to investigate if several leaked features are overlooked. As a result, 30 leaked features were found in the dataset. As a result, the number of features is reduced. </p>

## PREDICTIVE MODELING <a id ='model'></a>

<p align = 'justify'>The objective for predictive modeling is that the model should explain at least 80% of the variance for the target variable, and it should be generalizable. Not only that but it should also be well-calibrated. In order to determine if the criteria could be met, this dataset is separated into three sets: training set, validation set, and testing set. The training set is in use for the model to learn while the validation set is in use for tuning hyperparameters. Finally, the testing set is an unseen dataset. The feature and model selections are undertaken to maximize model performance. </p> 
  
<p align = 'justify'> There are 36 features in total besides the target variable. An attempt to select features manually is not possible since the total possible combinations are 68,719,476,736. Two automation are, for that specific reason, in use to optimize a model, which is the least shrinkage and selection operator (lasso) and Bayes optimal feature selection.</p>

<p align = 'justify'>  The lasso regularization is a popular method that proves to be successful in data science. This function is robust to the outlier but it is not differentiable due to piecewise function inderivative (Boehmke, 2021). This algorithm has the ability to identify unimportant predictors due to sparsity (predictors are set to 0). The Bayes optimal feature selection is different because it is an iterative process by balancing its needs of exploration and exploitation depending on three functions.</p>

- <p align = 'justify'> <b>Objective function:</b> The true shape of this function is not observable, and it can only reveal some data points that can otherwise be expensive to compute.</p>

- <p align = 'justify'> <b>Surrogate function:</b> The probabilistic model is being built to exploit what is known, and it alters in the light of new information.</p>

- <p align = 'justify'> <b>Acquistion function:</b> this function is to calculate a vector of hyperparameters that is likely to yield the higher local maximum of objective function using surrogate function.</p>

<p align = 'justify'>One of these studies shows that the approach has the ability to compete with many state-of-the-art methods (Ahmed et al., 2015).</p>

<p align = 'justify'> As a result, the best lambda for lasso is 0.005, and the R-squared score is 32.19% for the validation set, while the R-squared score for Bayes optimal feature selection is 32.87%. However, the number of features in total for lasso is 16, which is lower than the Bayesian approach (equal to 21 features in total).</p>

<p align = 'justify'>The best model is light gradient boosting, which is equal to 43.47% for the validation set. This model has already been optimized using Bayesian search theory. The R-squared score for the testing set is 41.92%. The root mean square error (RMSE) score is 0.97. For example, if this model predicts that the health inspection rating is 3.5, the actual score may fall somewhere between 2.53 and 4.47. The error scores indicate how large the error is by averaging the difference between predicted and actual values.</p>

## ECONOMETRIC METHOD <a id = 'econ'></a>

<p align = 'justify'> To the extent that the causal relationship fails to be established, the previous discussion indicates the limits of the data science approach. Resolving these issues is by adopting the econometric approach. Some suggest that the minimal requirement for this approach is that the adjusted R-squared score must be positive. In a technical sense, the approach stipulates with model misspecification. More importantly, Gauss Markov assumptions are fundamental to the econometric approach, though the causal model may depart from some assumptions, as the following: </p>

- **A1:** linearity in parameters
- **A2:** no perfect collinearity
- **A3:** zero conditional mean error
- **A4:** homoskedasticity and no serial correlation
- **A5:** normality of the error

<p align = 'justify'>For this analysis, the causal model does not follow A1 and A4 assumptions. The endogenous variable (or target variable) is limited between 1 and 5 points and is also known to be a Limited Dependent Variable (LDV). The popular solution is logarithmic transformation without Monte Carlo simulation. The transformation is, unfortunately, incorrect. This particular model misspecification is called Duan's Smear (Goldstein, 2020).</p>

<p align = 'justify'> The solution is to use a nonlinear least square called Probit with a Quasi-Maximum Likelihood Estimation condition (QMLE). This model is more efficient for heteroskedasticity. The heteroskedasticity is the variance of the residual term that is not constant through the regression. For example, the causal model is consistent and asymptotically normal, where V is not proportional to A. The assumption for consistency is:</p>

<div align = 'center'><img src="https://latex.codecogs.com/svg.image?\sqrt{n}(\hat\theta_{QMLE}&space;-&space;\theta_0)\sim^a&space;N(0,A^{-1}VA^{-1})&space;" title="\sqrt{n}\hat\theta_{QMLE} - \theta_0)\sim^a N(0,A^{-1}VA^{-1}) " /></div>

<div align = 'center'><img src="https://latex.codecogs.com/svg.image?A&space;=&space;-&space;E\left&space;[&space;H(\omega_i,\theta_0)&space;\right&space;]" title="A = - E\left [ H(\omega_i,\theta_0) \right ]" />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://latex.codecogs.com/svg.image?V&space;=&space;E[&space;s(\omega_i,\theta_0)s(\omega_i,\theta_0)']" title="V = E[ s(\omega_i,\theta_0)s(\omega_i,\theta_0)']" /></div>

<p align = 'justify'>Because the Breusch-Pagan test confirms that the error variances are all equal and is rejected at the significance level, this assumption for consistency is in use. At least one coefficient is heteroskedastic. More importantly, the causal model is contemporaneously exogenous, a weaker version of strict exogenity. In other words, the serial correlation may exist since the <i>Provider Information</i> dataset is cross-sectional. There is another part that is worth mentioning. The abuse icon is a dummy variable that is used for the Wald test of homogeneity with IV Probit. The null hypothesis that the endogeneity does not exist failed to be rejected at 5% alpha level.</p>

<div align = 'center'><img src="https://latex.codecogs.com/svg.image?y_i&space;=&space;\phi&space;(\beta_0&space;&plus;&space;\beta_1&space;bed_i&space;&plus;&space;\beta_2&space;hr_i&space;&plus;&space;cond_i&space;\beta)&space;&plus;&space;u_i" title="y_i = \phi (\beta_0 + \beta_1 bed_i + \beta_2 hr_i + cond_i \beta) + u_i" /></div>

<p align = 'justify'>The causal model contains &#934;(.) that is Probit function. The <i>cond<sub>i</sub>&#946;</i> is a vector of conditions that include the level of quality care and competency the staff provides, type of location area, and kind of environment residents live in, and kind of vulnerability the residents have.</p>

## ACTIONABLE INSIGHTS <a id='insights'></a>

<p align = 'justify'> When the causal relationship is established, each coefficient of exogenous variables is statistically different from zero at 5% using Huber-White (HCO covariance type) robust standard errors. The pseudo-R-squared score is 32.54%, while the p-value for LLR is below 5%. The null hypothesis for LLR is that the fit of the intercept-only model and causal model is equal and is rejected. Every coefficient of exogenous variables is an important contributor to this model except for intercept. The p-value for the intercept is 0.881.</p>

![image](https://user-images.githubusercontent.com/52142917/137193687-e4111f44-9e3d-4820-aefe-a1a469574a6a.png)

<p align = 'justify'> When all variables are being controlled, and the causal model is specified, the plot shows that the registered and licensed practical nurses have significant effects on health inspection ratings as expected. Interestingly, with the nurse aides being present in facilities, their positive effect on health inspection ratings is relatively less. When the interaction term is applied, there are a few interesting results that are important to mention.</p>

- <p align = 'justify'>When the <i>h<sub>i</sub></i> interacts with <i>nur_aid<sub>i</sub></i>, the increase in a number of hours that the nurse aides spend with residents each day has little to no impact on health inspection rating.</p>
- <p align = 'justify'>The registered nurses that interact with <i>h<sub>i</sub></i> have a positive impact on this rating on the significance level.</p>
- <p align = 'justify'>More surprisingly, when the <i>h<sub>i</sub></i> interacts with <i>lpn<sub>i</sub></i>, the coefficient is -0.0881 with a margin of error equal 0.07, and that is, the incremental increase in the number of hours that license practical nurses spend has a negative impact on this rating on the significance level.</p>
- <p align = 'justify'>Having family members on the council has a positive impact on this rating, but when the <i>bed<sub>i</sub></i> interacts with this variable, the coefficient is -0.011 with a margin of error equal to 0.008, which means an increase in a number of certified beds that lead to the decrease in this rating even with family members on the council.</p>

<p align = 'justify'>Tethered to the causal analysis, the measured quality for depressive-related code is higher, while the score for catheter-related code is lower. There is a potential linkage between the roles of nurses. For example, the role of registered nurses is to administer medication and treatments, while the role of licensed practical nurses is to comfort the residents and provide basic care, including the insertion of catheters.</p>

## FUTURE RESEARCH <a id = 'future'></a>

- <p align = 'justify'>As mentioned earlier, there is a potential linkage between the roles of nurses and measure codes. However, the linkage is undetermined since the two datasets are incompatible based on the record linkage. Hopefully, future study has this particular dataset that can be used to establish the causal relationship.</p>
- <p align = 'justify'>The CMS pilot program is established by either helping to provide financial aid for the LPN-to-RN career pathway or promoting awareness of existing programs. Randomized controlled trials (RCT) should be used to identify the real impacts of either approach on health inspection ratings over time. </p>
- <p align = 'justify'>The alternative study is the existing dataset that can establish a causal relationship by comparing particular facilities that have undergone LPN-to-RN training with those that do not.</p>
- <p align = 'justify'>The direct solution to inflation in self-report data is to adopt AI solutions, including Natural Language Processing. For example, if the data audit is undertaken to identify inflation and adjust, this information can be used to train either machine learning or deep learning in order to make the data audit cost-effective.</p>

## REFERENCES <a id = 'refer'></a>

"Technical Details." *Nursing homes including rehab services*, the Centers for Medicare and Medicaid Services, Sep. 2021. https://data.cms.gov/provider-data/topics/nursing-homes/technical-details#health-inspections

Ahmed, S., Narasimhan, H., and Agarwal, S. "Bayes Optimal Feature for Supervised Learning with General Performance Measures." arXiv, 2015. http://auai.org/uai2015/proceedings/papers/72.pdf

Boehmke, B. “Regularized Regression.” *UC Business Analytics R Programming Guide*, University of Cincinnati, 2021. http://uc-r.github.io/regularized_regression#lasso

Datta, A., Fredrikson, M., Ko, G., Mardziel, P., and Sen, S. "Proxy Discrimination in Data-Driven Systems." *Theory and Experiments with Learnt Programs*, arXiv, 25 Jul. 2017. https://arxiv.org/abs/1707.08120

Freud, RJ., Wilson, WJ., and Mohr, DL. "Inferences for Two or More Means." *Statistical Methods, Third Edition*, Academic Press, 2010. https://doi.org/10.1016/B978-0-12-374970-3.00006-8

Goldstein, Nathan. "Lecture 1. Foundations of Microeconometrics." *Microeconometrics*, Zanvyl Krieger School of Arts and Sciences Johns Hopkins University, 2021.

Kaynig-Fattkau, V., Blitzstein, J., and Pfister, H. "CS109 - Data Science." *Decision Trees*, Harvard University, 2021. https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=c22cbde8-94dd-42ad-86ef-091448ad02e4

Khandelwal, P. "Which algorithm takes the crown: Light GBM vs XGBOOST?" Analytics Vidhay, 12 Jun 2017. https://analyticsvidhya.com/blog/2017/06/which-algorithm-takes-the-crown-light-gbm-vs-xgboost/

Park, Y. and Ho JC. "PaloBoost." *An Overfitting-robust TreeBoost with Out-of-Bag Sample Regularization Techniques*, Emory University, 22 Jul 2018. http://arxiv-export-lb.library.cornell.edu/pdf/1807.08383

Silver-Greenberg, Jessica, and Robert Gebeloff. “Maggots, Rape and Yet Five Stars: How U.S. Ratings of Nursing Homes Mislead the Public.” *How U.S. Ratings of Nursing Homes Mislead the Public*, New York Times, 13 Mar. 2021. https://www.nytimes.com/2021/03/13/business/nursing-homes-ratings-medicare-covid.html

Sinharay, S. "Coninuous Probability Distributions." *The International Encyclopedia of Education*, Elsevier Science, 2010. https://doi.org/10.1016/B978-0-08-044894-7.01720-6

