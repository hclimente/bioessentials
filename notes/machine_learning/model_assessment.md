<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    TeX: {
      equationNumbers: {
        autoNumber: "AMS"
      }
    },
    tex2jax: {
      inlineMath: [ ['$','$'] ],
      displayMath: [ ['$$','$$'] ],
      processEscapes: true,
    }
  });
</script>
<script type="text/javascript"
        src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

# Model selection

> *All models are wrong; some are useful.* Abraham Lincoln

Model selection is the problem of choosing the *best* model among a set of competing models. There are several dimensions that can help us define what *best* means, such as:

* Sensitivity: we want all the important parameters to capture all the complexity of the data.
* Specificity/parsimony: we favor simpler models, with fewer parameters, to avoid overfitting.
* Future predictive ability: we want the model to generalize well.
* Selection consistency: the model size converges in probability to the true model size.

Usually we require a trade-off between different terms. That is because different approaches take emphasize different dimensions: cross-validation, for instance, focuses on future predictive ability. For example, we typically balance goodness of fit (how well the model describes the data) and parsimony (to avoid over-fitting). It can be interpreted as the trade-off between the bias introduced by a model too small and the variance that comes by a model too large. It's worth mentioning that the true model is unbiased and only has the necessary variance.

## Cross-validation

We split the data in two parts. We train the model using one of them (*training set*), and test it on the other (*testing set*).

## Information approaches/penalized log-likelihood

If we knew the right subset of features to study, we would just need to maximize the fit to the observed data ie maximize the log-likelihood. Because we do not, we can consider parsimony along with the fit, and maximize the following:

$$L(X,y,\hat{\theta})-c(\hat{\theta})$$

Where:

* $\hat{\theta}$ is a vector of parameters (for linear models,
this is ($\hat{β}, σ^{2}$)).
* $L(X,y,\hat{\theta})$ is the likelihood function of the model.
* $c(\hat{\theta})$ is a measurement of model complexity, usually some norm that measures how big \hat{\theta} is.

In general, these measurements take this form:

$$L(X,y,\hat{\theta})-\lambda p_{in}$$

Where:

* $\lambda$ is a factor that controls the penalty for complexity.
* $p_{in}$ is the number of parameters included in the model.

### Likelihood

The likelihood of a model M is $L=p(x\|\hat{\theta},M)$. It assumes the data is generated by the model plus some gaussian noise.

### Akaike Information Criterion

> The AIC of Akaike [1973] is motivated by the Kullback-Leibler discrepancy,
which (loosely) measures how far a model is from the truth. We assume
that the population or process from which the data was sampled is governed
by an unknown, perhaps nonparametric, true likelihood function $f(X,y)$,
and we want to approximate the unknown $f$ by a model-specific parametric
likelihood $g(X,y|θ)$. The “discrepancy” between $f$ and $g$, or “information lost”
by representing $f$ by $g$, is defined as $KL(f, g) = E(\ln f(x))− E(\ln g(X,y|θ)).$

$E(ln f(x))$ is unknown and the same for all the models being compared, so we really only care about $E(\ln g(X,y\|θ))$. It must be adjusted by the fact that θ is estimated from X and y:

$$E(\ln g(X,y|θ))=E_yE_x(\ln g(X,y|\hat{\theta}(X,y)))$$

Akaike showed that a good estimator for this term is $L(X,y,\hat{\theta})-p_{in}$. Hence the best model should minimize this function:

$$AIC=2L(X,y,\hat{\theta})-2p_{in}$$

AIC doesn't work well when $n$ is small (eg $n < 40p$), such as in GWAS. Small-sample corrections have been proposed. One such example is $AIC_c$:

$$AIC_c=AIC+\dfrac{2p_{in}(p_{in}+1)}{n−p_{in}−1}=-2L(X,y,\hat{\theta})+2(\dfrac{n}{n−p_{in}−1})p_{in}$$

### Bayesian Information Criterion

The Bayesian Information Criterion (BIC) assumes that the true model is present among the models. It tries to find it looking for the most probable model given the data:

$$Pr(M_i|x,y)=Pr(M_i)Pr(y|x,M_i)$$

Assuming equal priors (ie ignorance), $Pr(M_i\|x,y)\propto Pr(y\|x,M_i)$ and $Pr(y\|x,M_i)$ can be well approximated by $exp(1/2BIC)$, where

$$BIC=−2L(y|x,\hat{\theta}_{M_i})−ln(n)(p_{in}+ 2)$$

> $(p_{in}+ 2)$ is the number of parameters in the model, including the intercept and variance. Thus if we want to choose the model with the highest posterior probability, we need only choose the one with lowest BIC. Classically, $n$ here is the number of subjects and is thought to represent the amount of information in the sample.

#### Comparison to AIC

When BIC and AIC agree, we can be confident about our model. When they do not, it really depends on our data:

* High $n$: BIC is usually preferred: BIC has a stricter selection criteria, and tends to pick smaller models, while AIC tends to overfit. Additionally BIC is a consistent model selection criteria: if the number of models is finite and the true one is among them, the probability of detecting it approaches 1 as $n$ tends to infinite.
* Small $n$: with the rather modest sample sizes we usually work with, BIC tends to underfit. Also, this nice asymptotic property is based on unrealistic assumptions. In those cases, AIC or AIC<sub>c</sub> are recommended.

## Adaptative model selections

The $\lambda$ factor is a data-adaptive penalty derived using the generalized degrees of freedom for a given modeling procedure.

## Bayesian approaches

Google: Bayes factor, model evidence.

## Sources

* Dziak, J., Li, R., & Collins, L. (2005). Critical Review and Comparison of Variable Selection Procedures for Linear Regression, 1–69.