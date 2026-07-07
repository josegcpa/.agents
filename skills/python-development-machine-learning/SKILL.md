---
name: python-development-machine-learning
description: Contains opinionated general instructions and guidelines on how to develop machine learning software using Python.
---

Always read `python-development-general` skill before developing Python code.

## Instructions

- When developing machine-learning models, prioritise `scikit-learn` for data pre-processing and pipeline construction, and include simple models (i.e. regularised logistic regression) together with more advanced models (i.e. CatBoost, XGBoost, LightGBM)
- Unless otherwise noted, always opt for nested cross-validation to determine the best hyperparameters. Set random seeds and use deterministic settings where possible to make experiments reproducible.
- Fit all preprocessing and feature engineering steps inside the cross-validation loop; never fit them on the full dataset before splitting.
- Choose evaluation metrics that match the problem (e.g., stratified metrics for imbalanced classification, appropriate regression metrics). Report confidence intervals when feasible.
- Handle class imbalance with appropriate techniques (e.g., class weights, resampling, or focal loss) rather than ignoring it.
- Persist tabular models with `joblib` as part of a scikit-learn pipeline. For neural networks, use `torch.save` / `torch.load` with `weights_only=True` and save the model architecture definition alongside the weights.
- When developing neural networks, use `torch` for model development. Prefer mixed precision training (`torch.amp`) for large models and place tensors on the correct device explicitly.
- When developing neural networks for radiology/nuclear medicine, use MONAI (`uv install monai`) for data loading, (pre-)processing and data augmentations (`monai.transforms`), and try to use `monai.networks` if they are available. If there is no pre-determined spacing default to the common one. If models are to be trained with more than on sequence, resampling should be performed to match the space of the first sequence. If you want to perform sliding window prediction, use MONAI's `monai.inferers.SlidingWindowInferer`
- When developing neural networks for histopathology, use `torchvision` for data loading, (pre-)processing and data augmentations (`torchvision.transforms`), and for networks use `torchvision.models`.
