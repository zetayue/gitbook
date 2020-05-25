# Better Training

### Configure capacity with nodes and layers

1. make sure model has sufficient capacity \(can be over-trained\)

### Configure what to optimize with loss functions

1. distill all aspects of the model down into a single number in such a way that improvements in the number are a sign of a better model
2. understand the relationship of specified loss with objective
3. understand implication of loss
4. expect the value range of loss term is possible \(possible after certain data scaling\)
5. customized loss
use one or few samples and simple MLP, and SGD (or any optimizer) to verify the loss can be learned to reduce with gradient descent

6. weighted loss

### Configure gradient precision with batch size

1. smaller batches, as noisy updates to the model weights, brings \(generally\) a more robust model, regularizing effects and lower generation error
2. small batch results in generally rapid learning but a volatile learning process with higher variance in the performance metrics.
3. small batches \(more noisy updates\) to the model generally require a smaller learning rate
4. large batch sizes may enable the use of larger learning rate

### Configure the speed of learning with learning rate

1. too large learning rate could make gradient descent inadvertently increase rather than decrease the training error.
2. too large learning could lead to gradient explosion \(numerical overflow\), or oscillations in loss.
3. too small learning rate could result in slower training and stuck training in a region with high training error
4. too small learning rate could demonstrate over-fitting-like pattern
5. observe training it learns to quickly \(sharp rise/decline and plateau\) or learn to slowly \(little or no change\)
6. learning rate schedule
7. the method of momentum is designed to accelerate learning, especially in the face of high curvature, small but consistent gradients, or noisy gradients

### Stabilize learning with data scaling

1. unscaled input variables can result in slow or unstable learning process
2. unscaled target variables on regression problems can result in exploding gradients causing the learning process to fail

### Fixing vanishing gradients with ReLU \(sigmoid and hyperbolic tangent not suitable as activations\)

1. when using ReLU in the network, to avoid “dead” nodes in the begining of training,consider setting the bias to small value, such as 0.1 or 1.0
2. use ‘he-normal’ to initialize the weights
3. extensions of ReLU

### Fixing exploding gradients with gradient clipping

1. a chosen vector norm and clipping gradient values that exceed a preferred range
2. only solves numerical stability issues, no implication of overall model performance
3. possible cause: learning rate that that results in large wight updates, data prep that allows large differences in the target variable, loss function that allows calculation of large error values

### Accelerate learning with batch normalization

train deep neural networks with tens of layers is challenging as they can be sensitive to the initial random weights and configuration of the learning algorithm. one possible reason for this difficulty is the distribution of the inputs to layers deep in the network may change after each mini batch when the weights are updates. this can cause the learning algorithm to forever change a moving target. this change in the distribution of inputs to layers in the network is referred to by the internal covariate shift. batch normalization is a technique for training very deep neural networks that standardize the inputs to a layer for each mini batch. the weights of a layer are updated given an expectation that the prior layer outputs values with a given distribution.

1. to standardize the inputs to a network, applied to either activations of a prior layer or inputs directly accelerates training, providers some regularization effect, reducing generalizing error 
2. could make network training less sensitive to weight initialization 
3. probably use before the activations 
4. enable the use use large learning rate \(also increase decay rate if learning rate schedule is in place\)

### Greedy layer-wise pre-training

1.the choice of initial parameters for a deep neural network can have a significant regularizing effect \(and, to a lesser extent, that it can improve optimization\)

### Jump start with transfer learning

### Blogs
1. what should I do if my network does not learn [https://stats.stackexchange.com/questions/352036/what-should-i-do-when-my-neural-network-doesnt-learn]

### Issues Log

