## Summary

[Paper URL](http://proceedings.mlr.press/v119/kristiadi20a/kristiadi20a.pdf)


- Objective:
  1. we provide theoretical analysis on why ReLU networks equipped with Gaussian distributions over the weights mitigate the overconfidence problem in the binary classification setting,
  2. we show that a sufficient condition for having this property is to be “a bit” Bayesian: employing a last-layer Gaussian approximation—in particular a Bayesian one, and
  3. we validate our theoretical findings via a series of comprehensive experiments involving commonly-used deep ReLU networks and Laplace approximations in both binary and multi-class cases

- overview
  - Uncertinaity is important in decision matking 
  - (point estimation) ReLU nets are asymptotically overconfident
  - Gaussian structure over the weight migigate this 
  - Asimple Bayes nethod (last layer Laplace) is usfficient


- setting
  - $f_{\theta}: R^n -> R$ binary ReLU net, yield a piecewise-affine fuction
  - $p(y=1):= \sigma(f_{\theta}(x))$  prediction
  - Confidence: conf: $R^n -> R \ with \ x -> max_{i∈ \{0,1\}} \ p(y = i|x)$
  - Problem: For any $x \ and ∈ > 0, \ there \ is \ δ > 0 $ s.t. conf(δx) >= 1-∈. Moreover, $lim_{δ->∞} \ conf(δx) = 1$
  - Assign Gaussian over weight $N(\theta|µ, \sum)$
    - MAP estimation infers µ and not $\sum$
  - Predition = $p(y = 1|x, D) = \int σ(fθ(x)) p(θ|D) dθ$
  - The integral is intractable. approximation (Probit approximation, due to spiegelhalter & Lauritzen)//
   $ p(y = 1|x) = σ(f_µ (x) / \sqrt{1 + \pi/8(∇_\theta f_\theta(x)|µ)^T \sum(∇_\theta f_\theta(x)|µ)})$

- Results
  - Assumption (J comes from piecewise affine structure of the net; s is singular and λ is eigenvalues)
    - jjk
  - If $ f_\theta$ has no bias papameter, they can also show:
    - this prperty holds for some finite δ, not just in the limit 
  - Reason for getting reslts:
    - Addition structure in the covariance
    - Prediction via marginalization
  - Fix all but last layer of $f_\theta$
  - Assign a Gaussian $N(w|µ,\ \sum)$ over the last layer's weight
  - then:
 
- Last layer Laplace approximation:
  - $f_{\theta_{MAP}}$ an L-layer MAP-trained ReLU net
  - $\phi_{\theta_{MAP}}$ the First L - 1 layers
  - Fit Gaussian N(w|w_{MAP}, H^-1)$ arounf last layer's weight $w_{MAP}$, Where H is Hessian of negative log-posterior
  - Prediction of any x (binary clf):
      $p(y=1|x) \approx \ σ( f_{\theta_{MAP}}(x)/ \sqrt{1 + \pi/8 \phi_{\theta_{MAP}}(x)^T H^-1 \phi_{\theta_{MAP}}(x)})$
  - Or via (Last layer) MC integral

- Imp:
  - ReLU net + point estimat = asymptotic overconfidence
  - (Additional) Gaussian structure to the weights migigate this
    - Theoretically, any gausian will do
    - But, we fouused on basian methods
  - Simple last layer Laplace work well
    - Thus: "being a bit bayesian" helps
