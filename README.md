# Mixture-of-Experts-LM-Heads_for_Configurable-Product-Recommendation

## Results on laptop dataset

### laptop dataset

|method|Processor|Graphic Card|Hard Disk|RAM|Screen|
|---|---|---|---|---|---|
|Single Task|47.1|55.7|48.0|45.8|62.4|
|MT-DNN|47.8|59.3|47.4|51.7|62.5|
|LACO|46.6|57.7|48.1|52.0|62.7|
|MTOP|47.4|57.3|47.8|52.5|60.7|
|ModernBERT|48.9|58.4|49.0|**55.1**|63.9|
|MoELMH|**50.1**|**61.7**|**50.1**|53.2|**66.9**|

## Results for extended experiments

### Car seat dataset

|method|Seat Type|Weight Range|Installation Type|Harness Type|
|---|---|---|---|---|
|Single Task|40.5|57.1|64.9|65.1|
|MT-DNN|41.9|59.8|67.3|67.0|
|LACO|41.7|60.5|67.5|67.0|
|MTOP|40.6|60.3|67.0|67.1|
|ModernBERT|42.2|58.6|67.9|67.1|
|MoELMH|**42.7**|**61.1**|**68.0**|**68.2**|

### bike dataset

|method|Bike Type| Age Range| Wheel Size| Number of Speeds| Brake Style| Frame Material| Suspension Type
|---|---|---|---|---|---|---|---|
|Single Task|52.9|74.5|61.7|57.6|54.8|67.7|49.5|
|MT-DNN|56.8|76.1|62.3|58.4|53.1|67.6|49.7|
|LACO|57.1|76.1|61.8|57.8|54.3|67.9|**50.7**|
|MTOP|55.8|75.7|61.3|59.5|55.0|65.4|47.7|
|ModernBERT|55.9|76.3|62.1|58.1|54.1|68.1|49.1
|MoELMH(old)|58.0|77.0|64.1|**60.4**|**55.6**|67.6|50.1|
|MoELMH(new)|**59.2**|**77.5**|**64.3**|**59.8**|**55.1**|**68.4**|50.5|

### smartwatch dataset

|method|Screen Size| Display Type| Battery Life
|---|---|---|---|
|Single Task|43.9|45.7|62.5|
|MT-DNN|44.4|45.9|63.7|
|LACO|44.2|44.9|64.6|
|MTOP|43.3|45.4|65.2|
|ModernBERT|44.1|44.9|64.9|
|MoELMH(old)|45.8|47.0|**67.2**|
|MoELMH(new)|**46.3**|**47.2**|**66.5**|

# Theoretical foundation

## Formal system

We attempt to analyze the theoretical foundation of MoELMH from the view of linear regression. In this formal system, we compare the MoELMH method and baseline method. The baseline method refers to the model that uses a single linear layer for each attribute task respectively, while MoELMH allows expert linear layers to be shared among tasks.

## Analysis object: Parameter Esimation Error

In this formal system, we focus on the parameter estimation error to investigate how MoELMH method behaves differently from the baseline method.

## Parameter Estimation Error of baseline method

We start from looking at the estimation error of the baseline method.

### Problem setup

![image](./img/problem_setup.png)

The data model in our problem is:
![image](./img/problem_setup_data_model.png)

The corresponding baseline model is:
![image](./img/problem_setup_baseline.png)

### Derivation

#### Step 1
![image](./img/baseline_error_step1.png)
OLS estimator is obtained from:
![image](./img/OLS_estimator_derivation.png)

#### Step 2
![image](./img/baseline_error_step2.png)
This form of estimation error is derived from below. The below derivation is further proved under the appendix section.
![image](./img/baseline_error_step2-1.png)
![image](./img/baseline_error_step2-2.png)
So we have the form of estimation error shown above as:
![image](./img/baseline_error_step2-3.png)

#### Step 3
![image](./img/baseline_error_step3-1.png)
![image](./img/baseline_error_step3-2.png)

#### Appendix

**For expectation's derivation**
![image](./img/baseline_appendix_expect_0.png)
![image](./img/baseline_appendix_expect_1.png)
For the first term,
![image](./img/baseline_appendix_expect_2.png)
For the second term,
![image](./img/baseline_appendix_expect_3.png)
For the third term,
![image](./img/baseline_appendix_expect_4.png)
Combining these three terms, we have
![image](./img/baseline_appendix_expect_5.png)
Apply this identity to the estimation error, we have the form of expectation used in the step 2.
![image](./img/baseline_appendix_expect_6.png)

**For covariance's derivation**
![image](./img/baseline_appendix_cov_1.png)
We leverage two rules for covariance, the first rule is constant shift rule:
![image](./img/baseline_appendix_cov_2.png)
The second rule is linear transform rule:
![image](./img/baseline_appendix_cov_3.png)
Applying these two rules to the weight \tide{w_t}:
![image](./img/baseline_appendix_cov_4.png)
Finally, we substitute the noise term and we have the form of covariance used in the step 2.
![image](./img/baseline_appendix_cov_5.png)

## Parameter Estimation Error of MoELMH

### Setup

**Data model**

We have data model as below:
![image](./img/MoE/MoE_problem_setup_datamodel.png)
![image](./img/MoE/MoE_problem_setup_1.png)
Turning data model into a matrix form.
![image](./img/MoE/MoE_problem_setup_2.png)

**Assumptions**

![image](./img/MoE/MoE_assumption_1.png)
Assumption 2 is supported by our mutual information loss. The MI loss in our framework encourage router network to assign a uniquely different expert LM head subset to different task.
![image](./img/MoE/MoE_assumption_2.png)
We assume the true model is saprse.
![image](./img/MoE/MoE_assumption_3.png)
![image](./img/MoE/MoE_assumption_4.png)

**OLS Estimator**

To avoid the identifiability problem and bilinear complication, we assume coefficient matrix ***A*** is known. To support this claim, we can adjust our framework by training the router network first and then the expert LM heads. That is we can train the coefficient matrix ***A*** and expert heads ***V*** separately. In this case, the trained coefficient matrix ***A*** can be treated as known when we train the expert LM heads ***V***.
![image](./img/MoE/MoE_oracle_estimator.png)

**Identifiability problem**

![image](./img/MoE/MoE_identifiability_problem.png)

### Derivation

#### Step 1: Obtaining covariance of the expert *V*'s estimator
![image](./img/MoE/MoE_proof_step_1.png)
![image](./img/MoE/MoE_proof_step_2.png)

#### Step 2: Obtaining covariance of the task parameter *W*'s estimator
![image](./img/MoE/MoE_proof_step_3.png)
![image](./img/MoE/MoE_proof_step_4.png)

#### Step 3: Obtaining parameter estimation error for MoE
![image](./img/MoE/MoE_proof_step_5.png)
![image](./img/MoE/MoE_proof_step_6_1.png)
![image](./img/MoE/MoE_proof_step_6_2.png)

### Remark for extension to multi-class classification via One-vs-Rest

**Our analysis is about structural insight, not loss-specific.**
**Theory captures the mechanism(Statistical sharing effect), not exact loss.**
![image](./img/MoE/MoE_remark_extension.png)

## Local Approximation Theorem connecting squared-loss surrogate and cross-entropy loss formulation

### Setup

**Data model**
![image](./img/local_approx/cross_entropy_model.png)

**Assumptions: Non-degeneracy (bounded probabilities)**

> Besides the 4 assumptions(A1-A4) introduced in the analysis for MoE method, we introduce one more assumption:

![image](./img/local_approx/local_approx_assumption.png)

**Estimator**

![image](./img/local_approx/cross_entropy_estimator.png)

### Local quadratic form of cross-entropy loss around optimum

**Squared Loss's Reminder**: Originally, we are analyzing the squared loss. The squared loss has the following form for our data model Y=HVA.
![image](./img/local_approx/squared_loss_form_1.png)
![image](./img/local_approx/squared_loss_form_2.png)
Then we apply Taylor expansion on the cross-entropy loss and compare the expansion with the above squared loss. 
![image](./img/local_approx/cross_entropy_taylor_expan.png)
Based on the local approximation of cross-entropy loss to squared loss, we apply our previous anlaysis results to the cross-entropy loss under local approximation setting.

**Application of previous analysis result**: 

#### step 1: Covariance of expert V
From the previous analysis result, we knew that the covariance has the below form:
![image](./img/MoE/MoE_proof_step_2.png)
In squared loss case, the covariance is calculated by the following 
> (AA \otimes HH)^-1

Similarly in quadratic form of cross-entropy loss, the covariance is calculated by the following term
> Hessian of cross-entropy L on expert V = nabla^2 L(V)

That is:
> Cov(V) = (Hessian of cross-entropy L on expert V)^-1 = (nabla^2 L(V))^-1

**Note**: The above solution of Cov(V) can be rigorously derived. The detail of derivation will be placed in appendix.

#### step 2: Analysis on Hessian matrix
We derive and analyze the structure of the Hessian matrix as below. The Hessian matrix can be derived by taking the derivative of the cross-entropy loss twice.
![image](./img/local_approx/Hessian_structrue_1.png)
![image](./img/local_approx/Hessian_structrue_2.png)
![image](./img/local_approx/Hessian_structrue_3.png)

#### step 3: Parameter estimation error
With the structure of the Hessian matrix, we apply the Hessian to the parameter estimation error.
![image](./img/local_approx/para_estim_error_bound_1.png)
![image](./img/local_approx/para_estim_error_bound_2.png)
Finally, we transfer our original results under squared loss surrogate to the cross-entropy loss formulation.

### Appendix

#### Covariance = (Hessian)^-1

The cross-entropy loss in our problem is as below:
![image](./img/local_approx/covariance_setup_cross_entropy_loss_1.png)
We formulate the first and second derivative of cross-entropy loss as follows:
![image](./img/local_approx/covariance_setup_cross_entropy_loss_2.png)

Then we apply the Taylor expansion to the first derivative of cross-entropy loss around the optimum to derive the solution of the estimator \theta^\hat.
![image](./img/local_approx/covariance_taylor_expansion.png)

With the solution of the estimator \theta^\hat, we take covariance for both sides.
![image](./img/local_approx/covariance_derivation.png)

Then we calculate the Cov(\nabla L) and \nabla^2 L respectively.

For Cov(\nabla L):
![image](./img/local_approx/covariance_cov_gradient.png)
For \nabla^2 L:
We have an identity stating the equivalence of Hessian and Fisher information as below:
![image](./img/local_approx/identity_Hessian_Fisher.png)
Therefore, 
![image](./img/local_approx/covariance_hessian.png)

Now we can derive the covariance of the expert parameter as below. The result reveal that the covariance approximate the inverse of the Hessian.
![image](./img/local_approx/covariance_result.png)

#### Bias term in MSE parameter estimation error
Since our estimator is derived from the cross-entropy loss formulation now, the estimator become biased causing the non-zero bias term. **Note**: The estimator in squared loss setting is unbiased so the bias term is zero in squared loss setting.

Now the MSE parameter estimation error in our problem is as below. The first term is the tr(Cov) which is already finished above. The second term is the bias term and the bias term can be shown negligible to the first term, so that we can ignore the bias term and focus on the first term.
![image](./img/local_approx/MSE_param_estim_error.png)
We present a concise argument below to show that the bias term is negligible compared to the the first term tr(Cov).
![image](./img/local_approx/MSE_bias_term_1.png)
![image](./img/local_approx/MSE_bias_term_2.png)
![image](./img/local_approx/MSE_bias_term_3.png)