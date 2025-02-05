# Overfitting vs underfitting

Two fundamental concepts in machine learning:

### Overfitting

Overfitting occurs when a model is too complex and learns the training data too well, including the noise and random fluctuations. As a result:

* The model performs extremely well on the training data.
* But it fails to generalize well to new, unseen data.

Symptoms:

* High training accuracy.
* Low test accuracy.
* Model is too complex (e.g., too many parameters).

### Underfitting

Underfitting occurs when a model is too simple and fails to capture the underlying patterns in the training data. As a result:

* The model performs poorly on both the training and test data.

Symptoms:

* Low training accuracy.
* Low test accuracy.
* Model is too simple (e.g., too few parameters).

### The Goal

The goal is to find a balance between overfitting and underfitting, where the model is complex enough to capture the underlying patterns in the data but not so complex that it overfits.

### Techniques to Prevent Overfitting

1. Regularization: Add a penalty term to the loss function to discourage large weights.
2. Dropout: Randomly drop out neurons during training to prevent reliance on specific neurons.
3. Early stopping: Stop training when the model's performance on the validation set starts to degrade.
4. Data augmentation: Increase the size of the training set by applying transformations to the existing data.
5. Ensemble methods: Combine the predictions of multiple models to reduce overfitting.

### Techniques to Prevent Underfitting

1. Increase model complexity: Add more layers, neurons, or parameters to the model.
2. Increase training data: Add more data to the training set to help the model learn.
3. Increase training time: Train the model for a longer period to help it converge.
4. Feature engineering: Create new features or transform existing ones to help the model learn.
