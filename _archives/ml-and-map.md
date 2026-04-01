---
title: "Maximum Likelihood and Maximum a Posteriori"
---

## Maximum Likelihood

### Introduction

If $\mathbf{x} = (x_1, x_2, \dots, x_n,\dots,x_N)$ denotes $N$ observations of the scalar variable $x\sim\mathcal{N}(\mu,\sigma^2)$, where the Gaussian distribution is i.i.d., the likelihood function is expressed as

$$
\begin{align}
    p(\mathbf{x}|\mu,\sigma^2) = \prod_{n=1}^N\mathcal{N}(x_n|\mu,\sigma^2),
\end{align}
$$

where $\mu$ and $\sigma^2$ are changable parameters. The log likelihood function can be written in the form

$$
\begin{align}
    \ln p(\mathbf{x}|\mu,\sigma^2) = -\frac{1}{2\sigma^2}\sum_{n=1}^N(x_n-\mu)^2 - \frac{N}{2}\ln\sigma^2 - \frac{N}{2}\ln(2\pi).
\end{align}
$$

By applying partial derivative method, the solutions of maximum likelihood are obtained by

$$
\begin{gather}
    \mu_{ML} = \underset{\mu}{\arg\max}\; \ln p(\mathbf{x}|\mu,\sigma^2) = \frac{1}{N}\sum_{n=1}^Nx_n\\
    \sigma^2_{ML} = \underset{\sigma^2}{\arg\max}\; \ln p(\mathbf{x}|\mu,\sigma^2) = \frac{1}{N}\sum_{n=1}^N(x_n-\mu_{ML})^2.
\end{gather}
$$

It is clear that for a Gaussian distribution, the solution for $\mu$ decouples from that for $\sigma^2$ so that we can first evaluate (1.3) and then subsequently use this result to evaluate (1.4).

### Bias of maximum likelihood

We first note that the maximum likelihood solutions $\mu_{ML}$ and $\sigma^2_{ML}$ are functions of the dataset values $x_1,\dots, x_N$. Suppose that each of these values has been generated independently from a Gaussian distribution whose true parameters are $\mu$ and $\sigma^2.$ Then, the expectations of $\mu_{ML}$ and $\sigma^2_{ML}$ with respect to these dataset values are

$$
\begin{align}
    \mathbb{E}[\mu_{ML}] &= \frac{1}{N}\sum_{n=1}^N\mathbb{E}[x_n] = \frac{1}{N}\sum_{n=1}^N\mu = \mu\\
    \mathbb{E}[\sigma^2_{ML}] &= \frac{1}{N}\sum_{n=1}^N\mathbb{E}[(x_n-\mu_{ML})^2] = \frac{1}{N}\sum_{n=1}^N\mathbb{E}[((x_n-\mu)-(\mu_{ML}-\mu))^2]\notag\\
    &= \frac{1}{N} \sum_{n=1}^N\mathbb{E}[(x_n-\mu)^2] - \frac{2}{N}\sum_{n=1}^N\mathbb{E}[(x_n-\mu)(\mu_{ML}-\mu)] + \frac{1}{N}\sum_{n=1}^N\mathbb{E}[(\mu_{ML}-\mu)^2]\notag\\
    &= \frac{1}{N}\sum_{n=1}^N\sigma^2 - 2\mathbb{E}[(\mu_{ML}-\mu)^2] + \mathbb{E}[(\mu_{ML}-\mu)^2] = \sigma^2 - \mathbb{E}[(\mu_{ML}-\mu)^2]\notag\\
    &= \sigma^2 - \text{Var}[\mu_{ML}] = \sigma^2 - \frac{1}{N}\sigma^2 = \left(\frac{N-1}{N}\right)\sigma^2.
\end{align}
$$

This shows that the maximum likelihood estimate of the variance will underestimate the true variance by a factor $(N-1)/N.$ This is an example of a phenomenon called $bias$ in which the estimator of a random quantity is systematically different from the true value.\par
Note that bias arises because the variance is measured relative to the maximum likelihood estimate of the mean, which itself is tuned to the dataset. The issue of bias in maximum likelihood is closely related to the problem of $over$-$fitting$. For a Gaussian distribution, the following estimate for the variance parameter is unbiased:

$$
\begin{align}
    \tilde{\sigma}^2 = \frac{N}{N-1}\sigma^2_{ML} = \frac{1}{N-1}\sum_{n=1}^N(x_n-\mu_{ML})^2.
\end{align}
$$

### Curve fitting

Suppose we have to fit polynomial curve to the given training data where there are $N$ input values $\mathbf{x}=(x_1,\dots,x_N)^\top$ and their corresponding target values $\mathbf{t} = (t_1,\dots,t_N)^\top.$ The polynomial curve is written as

$$
\begin{align}
    y(x, \mathbf{w}) = w_0 + w_1x + w_2x^2 + \cdots + w_Mx^{M} = \sum_{j=0}^Mw_jx^j,
\end{align}
and each target value $t$ follows a Gaussian distribution as
\begin{align}
    p(t|x,\mathbf{w},\beta) = \mathcal{N}(t|y(x,\mathbf{w}),\beta^{-1}),
\end{align}
$$

where $y(x,\mathbf{w})$ is its mean and $\beta=1/\sigma^2$ is a precision. Since every input is independent to each other,

$$
\begin{gather}
    p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta) = \prod_{n=1}^N \mathcal{N}(t_n|y(x_n,\mathbf{w}),\beta^{-1})\\
    \ln p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta) = -\frac{\beta}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 + \frac{N}{2}\ln\beta - \frac{N}{2}\ln(2\pi)\\
    \frac{\partial\ln p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)}{\partial\beta}\bigg|_{\beta=\beta_{ML}} = -\frac{1}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 + \frac{N}{2\beta_{ML}}=0\\
    \frac{1}{\beta_{ML}} = \frac{1}{N}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 = \frac{2}{N}E(\mathbf{w}),
\end{gather}
$$

where $E(\mathbf{w})$ is a sum-of-squares error. By minimizing $E(\mathbf{w})$, we can obtain maximum likelihood of weights:

$$
\begin{align}
    \mathbf{w}_{ML} = \underset{\mathbf{w}}{\arg\min}\; E(\mathbf{w}) = \underset{\mathbf{w}}{\arg\min}\; \frac{1}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2.
\end{align}
$$

After computing $\beta_{ML}$ and $\mathbf{w}_{ML},$ we can make predictive distribution for new values of $x$ because we have a probabilistic model that gives the probability over $t$ as

$$
\begin{align}
    p(t|x,\mathbf{w}_{ML},\beta_{ML}) = \mathcal{N}(t|y(x,\mathbf{w}_{ML}),\beta_{ML}^{-1}).
\end{align}
$$

## Maximum a Posteriori: A Step Towards Bayes

For simplicity, assuming $\alpha$ as a precision of distribution and $\mathbf{w}$ as a $M$-dimension multivariate random variable, we can consider a Gaussian distribution of the form

$$
\begin{align}
    p(\mathbf{w}|\alpha) = \mathcal{N}(\mathbf{w}|0,\alpha^{-1}\mathbf{I}) = \left(\frac{\alpha}{2\pi}\right)^{(M+1)/2} \exp\left(-\frac{\alpha}{2}\mathbf{w}^\top\mathbf{w}\right).
\end{align}
$$

By Bayes' Theorem, the posterior distribution for $\mathbf{w}$ is proportional to the product of the prior distribution and the likelihood function as 

$$
\begin{align}
    p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta) &\propto p(\mathbf{x},\mathbf{t},\mathbf{w},\alpha,\beta) = p(\mathbf{t}|\mathbf{x},\mathbf{w},\alpha,\beta)p(\mathbf{w}|\mathbf{x},\alpha,\beta)p(\mathbf{x},\alpha,\beta)\notag\\
    &= p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha)p(\mathbf{x},\alpha,\beta) \propto p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha).
\end{align}
$$

This is possible because $\mathbf{t}$ is independent to $\alpha$ and $\mathbf{w}$ is independent to $\mathbf{x}$ and $\beta.$ Because it is MAP, we can now determine $\mathbf{w}$ by finding the most probable value of $\mathbf{w}$ given the data. Optimal weight parameter obtained by MAP is

$$
\begin{gather}
    \mathbf{w}_{MAP} = \underset{\mathbf{w}}{\arg\max}\; p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta) = \underset{\mathbf{w}}{\arg\min} -\ln p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha).
\end{gather}
$$

Negative logarithm of $p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha)$ is

$$
\begin{align}
    -\ln p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha) &= \frac{\beta}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 - \frac{N}{2}\ln\beta + \frac{N}{2}\ln(2\pi) - \frac{N+1}{2}\ln\frac{\alpha}{2\pi} + \frac{\alpha}{2}\mathbf{w}^\top\mathbf{w}\\
    &= \frac{\beta}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 + \frac{\alpha}{2}\mathbf{w}^\top\mathbf{w} - \underbrace{\frac{N+1}{2}\ln\alpha - \frac{N}{2}\ln\beta + \frac{2N+1}{2}\ln(2\pi)}_{\text{constant}}, 
\end{align}
$$

making the objective as

$$
\begin{align}
    \mathbf{w}_{MAP} = \underset{\mathbf{w}}{\arg\min}\left[\frac{1}{2}\sum_{n=1}^N(y(x_n,\mathbf{w})-t_n)^2 + \frac{\lambda}{2}\mathbf{w}^\top\mathbf{w}\right],
\end{align}
$$

where $\lambda=\alpha/\beta$ is a regularization parameter.

