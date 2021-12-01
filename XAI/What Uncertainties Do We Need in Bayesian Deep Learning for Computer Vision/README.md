## Summary

[Paper URL](https://dl.acm.org/doi/pdf/10.5555/3295222.3295309)

- Two types of uncertainty:

  - Aleatoric uncertainty: 
    - captures noise inherent in the observations
    - uncertainty which cannot be reduced by more data collection. 
    - vary for different inputs to the model
    - further categorized: 
      - Homoscedastic uncertainty: 
        - stays constant for different inputs
      - Heteroscedastic uncertainty:
        - depends on the inputs to the model
        - with some inputs potentially having more noisy outputs than others
        - important for computer vision applications
        - Example: 
          - for depth regression, highly textured input images with strong vanishing lines are expected to result in confident predictions, whereas an input image of a featureless wall is expected to have very high uncertainty
    - Example:
      - trying to estimate the distance to vehicles seen in camera images, we'd expect the distance estimate to be more noisy/uncertain for vehicles far away from the camera.
  - Epistemic uncertainty/model uncertainty: 
    - accounts for uncertainty in the model parameters
    - uncertainty which can be explained away given enough data.

- Objective:
  1. Capture an accurate understanding of aleatoric and epistemic uncertainties, in particular with a novel approach for classification
  2. Improve model performance by 1 − 3% over non-Bayesian baselines by reducing the effect of noisy data with the implied attenuation obtained from explicitly representing aleatoric uncertainty
  3. Study the trade-offs between modeling aleatoric or epistemic uncertainty by characterizing the properties of each uncertainty and comparing model performance and inference time.

- To model *epistemic uncertainty* in a neural network (NN)
  - puts a prior distribution over its weights W 
  - compute the posterior p(W | X, Y)
  - Such a model is referred to as a Bayesian neural network (BNN). 
  - BNNs are easy to formulate, but difficult to perform inference in.
  - Different approximations exist
  - Paper use Monte Carlo dropout (Use dropout during both training and testing. During testing, run multiple forward-passes and (essentially) compute the sample mean and variance).

- To model aleatoric uncertainty, 
  - assumes a (conditional) distribution over the output of the network (e.g. Gaussian with mean u(x) and sigma s(x))
  - learns the parameters of this distribution (in the Gaussian case, the network outputs both u(x) and s(x)) by maximizing the corresponding likelihood function. 
  - Note that in e.g. the Gaussian case, one does NOT need extra labels to learn s(x), it is learned implicitly from the induced loss function. The authors call such a model a heteroscedastic NN.

- Experiments
  - Without uncertainty
  - Only epistemic uncertainty
  - Only aleatoric uncertainty
  - Both aleatoric and epistemic uncertainty

- Finding: 
  - modeling both aleatoric and epistemic uncertainty improves over the baseline result (roughly 1-3% improvement over no uncertainty)
  - the main contribution is modeling the aleatoric uncertainty, suggesting that epistemic uncertainty can be mostly explained away in this large data setting.
  - Qualitatively, for depth regression, aleatoric uncertainty is greater for large depths, reflective surfaces and occlusion boundaries in the image

- More experiments and finding
  -  compare reduced training set sizes (1, 1⁄2, 1⁄4) and unrelated test datasets.
  - Finding:
    - the *aleatoric uncertainty* remains roughly constant for the different cases.
      - Aleatoric uncertainty cannot be explained away with more data
      - Aleatoric uncertainty does not increase for out-of-data examples (situations different from training set), whereas epistemic uncertainty does.

    - the *epistemic uncertainty* clearly decreases as the training datasets gets larger 
      - epistemic uncertainty can be explained away with enough data
      - the epistemic uncertainty is larger when we train on dataset T1-train and test on dataset T2-test, than when we train on dataset T1-train and test on dataset T1-test

    - These results reinforce the case that epistemic uncertainty can be explained away with enough data.
    - Still required to capture situations not encountered in the training set. This is particularly important for safety-critical systems, where epistemic uncertainty is required to detect situations which have never been seen by the model before.

    - The aleatoric uncertainty models add negligible compute
    - epistemic models require expensive Monte Carlo dropout sampling
    - 50× slow-down for 50 Monte Carlo samples.
