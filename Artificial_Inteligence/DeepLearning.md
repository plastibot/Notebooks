# Deep Learning Notes

## Cost Functions

### Quadratic Cost


### Cross-Entropy

Allows to learn faster when the derivative of the function is larger. For example:

```python3
from numpy import log
def cross_entropy(y, a)
    return -1*(y*log(a) * (1-y)*log(1-a))
    
>>> cross_entropy(1, 0.99)
0.010005

>>> cross_entropy(0, 0.01)
0.01005

>>> cross_entropy(0, 0.3)
0.35667

>>> cross_entropy(0, 0.6)
0.91629

>>> cross_entropy(0, 0.8)
1.60943
```


## Gradient Descent
Things to take into consideration

1. Learning rate is important. Too big and you will over shoot.
2. Batch Size: Using mini batches know as Stochastic Gradient Descent.
3. Second order calculations. Takes a look at the acceleration (momentum) of the learning to automatically adjust the learning rate.
    - AdaGrad
    - RMProp
    - Adam

### unstable Gradients
Unstable gradients is a prblemw with grsdient descent.

### Back Propagation


## Neuron Layers

### Dense

### Softmax

## Initialization

1. Glorot Uniform
2. Glorot Normal


## Overfitting
To minimize overfitting, you can do the following:
 1. You can add L1/L2 regularizations to 
 2. Dropout
 3. Artificially expand our data
 
 ## Types of Layers
 1. Dense
 2. Max Pooling - used to reduce the size of your image by oicking the highest value of a 2 x 2 array
 3. Flatten - Used to convert from cnvolutional to dense layers.
 4. Convolutional Layers
 
 ## Bath Normalization
 
 
