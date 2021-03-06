---
layout: post
title:  "Model parameter estimation: Least squares vs. maximum likelihood"
date:   2021-12-21 21:45:23 +0000
categories: science
---

If you have read anything about modelling, you have probably encountered one or more of these terms before. This post provides a definition of least squares estimation and maximum likelihood estimation and describes their similarities and differences.

<br/>

[comment]: <> (**Contents**)
**Table of contents**
* TOC
{:toc}

<br/>


## What is a model?

A model is an idealised explanation for some real world observations. We are normally interested in the *forward model*, which describes observations in terms of the unobserved parameters:

<br/>

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\Large&space;y_i=f(x_i,\theta)+\epsilon_i">
</p>

<br/>

Here, beta is the unknown parameters. x are the conditions and epsilon is the noise component.

Defining the forward model in this concise form is useful because now we can estimate beta, for example by plugging in different values of beta and seeing which matches our observed data. This process is called model *parameter estimation*.

<br/>

## Parameter estimation

Parameter estimation aims to find the parameter values that 'best fit' the observations, according to some criteria. The function that quantifies the best fit criteria is called the objective function. The objective function is a function of the observations and the predictive distribution.

The best-fit parameter estimates are determined by maximising this objective function: 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}=\underset{\theta\;\in\;\Theta}{\arg\max}\;\mathrm{O}(y,\theta,f)">
<br/>
</p>


When the objective function is concave and its derivative has a closed-form solution, the best-fit parameter estimates can be derived analytically. When this is not the case however, numerical optimisation methods, such as gradient descent, may be used.

An example of maximising the objective function is shown in the figure below.

<p align="center">
<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig01.png" width="500">
<br/>
<font size="2"> <strong>Figure 1.</strong> Parameter estimation maximises the objective function.</font>
<br/>
<br/>
</p>

In addition to parameter estimation, the forward model allows us to assess the accuracy of parameter estimation procedure by comparing its performance on data generated by some ground truth model parameters.

The accuracy of estimated parameters can be quantified by its bias and variance, which are determined theoretically by the limiting behaviour of the parameter estimation over repeated experiments. 

Bias is the expected difference between the parameter estimate and its ground truth value: 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Bias} \lbrack \hat{\theta} \rbrack = \mathrm{E}\lbrack \hat{\theta} \rbrack - \theta_{GT}">
<br/>
<br/>
</p>

Variance refers to the sampling variability over repeated experiments. 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Var} \lbrack \hat{\theta} \rbrack = \mathrm{E}\lbrack (\hat{\theta} - \mathrm{E}\lbrack \hat{\theta} \rbrack )^2 \rbrack / n">
<br/>
<br/>
</p>

The figure below depicts the concepts of bias and variance in parameter estimation:

<p align="center">
<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig02.png" width="500">
<br/>
<font size="2"> <strong>Figure 2.</strong> Bias and variance of parameter estimation</font>
<br/>
</p>

The objective functions, bias and variance of the least squares and maximum likelihood parameter estimation techniques are described below. Following that, two examples are provided which demonstrate the conditions under which the two parameter estimation techniques provide equivalent estimates.

### Least squares

The objective function for least squares estimation (LSE) is the sum-of-squared differences betewen the oberved data and the model predictions:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;S=\sum_i (y_i - f(x_i,\hat{\theta}))^2">
<br/>
</p>

Least squares seeks to make the sum of squared differences as small as possible:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\theta_{LSE}=\underset{\hat{\theta}\;\in\;\Theta}{\arg\min}\;S">
<br/>
</p>

For linear functions, the solution can always be determined analytically via the normal equations:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}_{LSE}=(X^TX)^{-1}X^TY">
<br/>
</p>

According to the Gauss-Markov theorum, LSE is not only unbiased, but is the best linear unbiased estimator (BLUE) when the errors are uncorrelated, identically distributed, and with expected value of 0.

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Bias}\lbrack\hat{\theta}\rbrack=0 \,\, \mathrm{iff} \,\, \mathrm{E}\lbrack\epsilon\rbrack=0">
<br/>
</p>

Furthermore, the variance of the LSE is equal to:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{var}(\hat{\theta}_j) \approx \hat{\sigma}(X^TX)^{-1}\,_{jj}">
<br/>
</p>

where 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\sigma^2}=\frac{S}{n-m}">
<br/>
</p>

For non-linear functions, there is typically no analytic solution. Therefore, iterative optimisation techniques, such as the Gauss-Newton algorithm are often applied.

The LSE for non-linear functions is normally biased, the extent to which depends on the form of the non-linear function. However, what is certain is that because it the LSE is still consistent, the bias will asymptotically decrease towards zero with an increasing number of observations. For non-linear least squares, regularisation methods may be applied to reduce the sampling variability of the estimate at the cost of increased bias.

*Applications*

Least squares is commonly used as a method to find the parameter values for a model that is linear. This is because the de-facto analytical solution to a linear model, when it exists, is also the least squares solution. This makes least squares a very popular method. But it can also be used to find the parameter values for more complex models, under certain conditions that are described later.

### Maximum likelihood

*Objective function & solution*

The best-fit criteria for maximum likelihood estimation (MLE) is the likelihood of the data given the current estimate.

So, let's define what likelihood is. In the context of model fitting, likelihood, L, is the joint probability of seeing your observed data given the model and parameter values.

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;L(\theta|y,x)=\prod_i p(y_i|\hat{\theta},x_i)">
<br/>
</p>

The likelihood is a joint probability over all data points, and hence (assuming independent measures) is a product over the likelihood of each data point.

To simplify calculations, we normally work with the log-likelihood because it simplifies the calculations. 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;l(\theta|y,x)=log(L(\theta|y,x))\\\\">
<br/>
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\text{   }\text{   }\text{   }=\sum_i log(p(y_i|\hat{\theta},x_i))">
<br/>
</p>

This does not change the solution because the logarithm is a monotonic function.

The optimisation finds the parameter values which produce the highest likelihood of seeing the observed data. 

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\theta_{MLE}=\underset{\hat{\theta}\;\in\;\Theta}{\arg\max}\;l(\theta|y,x)">
<br/>
</p>

(check text book for when it can be solved analytically) As before, this can in some cases be solved analytically, but usually requires optimisation.

*Bias and variance*

MLE has been shown not only to be unbiased, but to be the minimal variance unbiased estimator (MVUE & consistent estimator) when the number of samples goes to inifinity:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Bias}\lbrack\hat{\theta}\rbrack=0 ">
<br/>
</p>

This applies to all functions so long as the global MLE exists (uniquess of parameters-> function values) and the likelihood function itself is continuous.

The variance of the MLE is:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Var}\lbrack\hat{\theta}\rbrack=\frac{I^{-1}}{\sqrt{n}} ">
<br/>
</p>

where I is the fisher information matrix:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;I_{jk} = \mathrm{E}\left[-\frac{\partial^2 l(\theta|x)}{\partial\theta_j\partial\theta_k}\right]">
<br/>
</p>

Hence, as n goes to infinity, MLE is normally distributed with variance equal to the inverse of the fisher information matrix (hessian matrix of the second partial derivative of the log-likelihood function), which in this case is equal to sigma/sqrt(n).

*Applications*

Maximum likelihood is commonly used ...


## Examples

The following two examples illustrate the circumstances under which the LSE and MLE are equivalent and when they are different.

### 1: LSE and MLE are equivalent

To illustrate the case when the two methods are equivalent, let's generate ten fake data points using the simplest possible model, yi = 1\*2 + e, where e\~N(0,1).

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig0.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

These points were generated using the forward model in equation 1, with sigma set to 1. The dotted line shows the ground truth parameter that we wish to estimate. This is useful to know for assessing the accuracy of LSE and MSE.

The least squares estimate occurs when the partial derivative of S with respect to x-hat is equal to zero:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\frac{\partial S}{\partial \hat{\theta}} = \sum_i (y_i - \hat{\theta}) =0">
<br/>
</p>

With our simple forward model, the least squares estimate can be calculated analytically (as with all linear models). As shown, the least squares estimate occurs when x-hat is equal to the mean of the observations:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}_{LSE}=\bar{y}">
<br/>
</p>

which is with our data is X.

Figure X shows the sampling distribution of the LSE:

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig1.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

As you can see, the estimated parameter is not always equal to the ground truth parameter theta=2. This is because the mean is derived from the set of observations, which are samples that contain noise. 

In agreement with the conditions in equation X, LSE is unbiased, with its expected value equal to the ground truth. Furthermore, its sampling variance is X, which is equal to sigma/sqrt(n), according to equation X (and the central limit theorum).

Now, let's look at the MLE.

Normally distributed noise with sigma=1 has the following probability density function:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;p(y|\hat{x},\mu=0,\sigma=1) = \frac{1}{\sqrt{2\pi}}e^{\frac{(y-\hat{\theta})^2}{4}}">
<br/>
</p>

For this noise model the log-likelihood is then:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;l(\hat{x}|y,\sigma,\mu)=-\frac{n}{2}ln(2\pi)-\frac{n}{2}ln(\sigma^2)-\frac{1}{2\sigma^2}\sum_i (y_i - \hat{\theta})^2">
<br/>
</p>

The maximum log-likelihood over different values of x-hat occurs when the partial derivative of the log-likelihood with respect to x-hat is equal to zero:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\frac{\partial l}{\partial \hat{\theta}}=\frac{1}{\sigma^2}\sum_i \hat{\theta} - y_i=0">
<br/>
</p>

As you can see, in the case of gaussian noise, this also occurs when

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}_{MLE}=\bar{y}">
<br/>
</p>

The sampling distribution of the MLE is shown below:

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig2.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

As expected from equation x, MLE is also unbiased and distributed with sampling variance = sigma/sqrt(n).

In this example both LSE and MLE are unbiased, have equal variance and are therefore equivalent. This is because the conditions for zero bias have been met for both of the estimators.

Next, we show an example where the LSE and MSE are different.

### 2: LSE and MLE are different

In this example, the conditions for zero bias of LSE are violated by changing the distribution of the noise component from gaussian to rayleigh. This violated the gauss-markov theorum because the expected value of the rayleigh distribution is not equal to 0.

As before, let's generate ten fake data points using another simple model, yi = 1\*2 + e, but this time e\~R(0,1).

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig3.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

These points were generated using the forward model in equation 1, with v set to 0 and sigma set to 1. The dotted line shows the ground truth parameter that we wish to estimate. 

As the properties of the noise component are not considered, the LSE takes the same definition as before: when the partial derivative of S with respect to x-hat is equal to zero:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}_{LSE}=\bar{y}">
<br/>
</p>

which with our initial observations is 3.5. 

Below shows the sampling distribution of the LSE.

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig4.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

The expected value across samples is equal to 3.5, which differs from the ground truth parameter by a bias of 1.5.

As the least squares solution is ybar, the expected bias is equal to the expected value of the rayleigh distribution:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{E}\lbrack y \rbrack=\sigma\frac{\pi}{2}=\frac{\pi}{2}">
<br/>
</p>

In contrast, the MLE does consider the parameters of the noise distribution.

The rayleigh distribition has the following probability density function:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;p(y|\hat{x},v=0,\sigma=1) = \frac{y-\hat{x}}{\sigma^2}e^{\frac{-(y-\hat{x})^2}{2\sigma^2}}">
<br/>
</p>

For a rayleigh noise the log-likelihood is:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;l(\hat{x}|y,\sigma,v)=\frac{y-\hat{\theta}}{\sigma^2}e^{\frac{-(y-\hat{\theta})^2}{2\sigma^2}} ">
<br/>
</p>

The maximum log-likelihood over different values of x-hat occurs when the partial derivative of the log-likelihood with respect to x-hat is equal to zero:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\frac{\partial l}{\partial \hat{\theta}}=\sum_i -\frac{1}{y_i - \hat{\theta}} + \sum_i -(\hat{\theta}-y_i)">
<br/>
</p>

The solution for theta is:

<p align="center">
<br/>
<img src="https://latex.codecogs.com/svg.latex?\Large&space;\hat{\theta}_{MLE}=?">
<br/>
</p>

The sampling distribution of the MLE is shown below:

<br/>
<img src="{{ site.url }}/assets/posts/Leastsquares_vs_forwardmodel_vs_likelihood/fig5.png">
<br/>
<font size="2"> <strong>Figure 1:</strong>XXX. </font>
<br/>

The MLE provides unbiased estimation because its expeted value is equal to the ground truth parameter value.


## Summary



<br/>

## References



*Maths*

$x \leq 5$

In line maths: <img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">
Another in line maths: <img src="https://render.githubusercontent.com/render/math?math=e^{i %2B\pi} =x%2B1">

Another in line maths: <img src="https://latex.codecogs.com/svg.latex?e^{i %2B\pi} =x%2B1">

another in line: $`a^2+b^2=c^2`$

Before we start, let's first define what we mean by a model.




<!-- [here][this-post-link] -->
<!-- [this-post-link]({{ site.baseurl }}{% link _posts/2021-11-16-Tissue-weighted-mean.markdown %}) -->

