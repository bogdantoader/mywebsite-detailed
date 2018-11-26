---
Title: Research
---

## Super-resolution

<!--![aaa](/example_signal.png)-->


The convolution of a discrete measure, `$x=\sum_{i=1}^ka_i\delta_{t_i}$`, with a local window function, `$\phi(s-t)$`, is a common model for a measurement device whose resolution is substantially lower than that of the objects being observed. Super-resolution concerns localising the point sources `$\{a_i,t_i\}_{i=1}^k$` with an accuracy beyond the essential support of `$\phi(s-t)$`, typically from `$m$` samples `$y(s_j)=\sum_{i=1}^k a_i\phi(s_j-t_i)+\eta_j$`, where `$\eta_j$` indicates an inexactness in the sample value.   We consider the setting of `$x$` being non-negative and seek to characterise all non-negative measures approximately consistent with the samples. 


<img src="/img/example_signal.png" style="max-width:100%;min-width:40px;float:center;margin-right:5%;border-radius:4%" alt="signal" />

We first show that `$x$` is the unique non-negative measure consistent with the samples provided the samples are exact, i.e. `$\eta_j=0$`, `$m\ge 2k+1$` samples are available, and `$\phi(s-t)$` generates a Chebyshev system. This is independent of how close the sample locations are and _does not rely on any regulariser beyond non-negativity_.

Moreover, we characterise non-negative solutions `$\hat{x}$` consistent with the
samples within the bound `$\sum_{j=1}^m\eta_j^2\le \delta^2$`.  Any such
non-negative measure is within `${\mathcal O}(\delta^{1/7})$` of the
discrete measure `$x$` generating the samples in the generalised
Wasserstein distance.  Similarly, we show using somewhat different
techniques that the integrals of `$\hat{x}$` and `$x$` over
`$(t_i-\epsilon,t_i+\epsilon)$` are similarly close, converging to one
another as `$\epsilon$` and `$\delta$` approach zero.  We also show how to
make these general results, for windows that form a Chebyshev system,
precise for the case of `$\phi(s-t)$` being a Gaussian window.

The main innovation of these results is that non-negativity alone is
sufficient to localise point sources beyond the essential sensor
resolution and that, while regularisers such as total variation might
be particularly effective, they are not required in the non-negative setting.



### Preprint:

* _Sparse non-negative super-resolution --- simplified and stabilised_ </br>
  Armin Eftekhari, Jared Tanner, Andrew Thompson, Bogdan Toader and Hemant Tyagi</br>
  Submitted 
  [[arXiv]](https://arxiv.org/abs/1804.01490)


### Conference paper:


* _Non-negative super-resolution is stable_ </br>
  Armin Eftekhari, Jared Tanner, Andrew Thompson, Bogdan Toader and Hemant Tyagi</br>
  Presented at [IEEE Data Science Workshop 2018](https://2018.ieeedatascience.org), EPFL</br>
  [[IEEE Xplore]](https://ieeexplore.ieee.org/document/8439120/)
  [[SigPort]](http://sigport.org/documents/non-negative-super-resolution-stable)
  [[Paper]](/ET4_superresolution_dsw2018.pdf)
  [[Poster]](/ieee_dsw_poster.pdf)


