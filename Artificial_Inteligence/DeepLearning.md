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

### Back Propagation


## Neuron Layers

### Dense

### Softmax
