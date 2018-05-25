

First import Keras. Import modules as you need them.

```python3
import keras
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers.normalization import BatchNormalization
from keras import regularizers
from keras.optimizers import SGD
```

Load and preprocess your data

```python3
(X_train, y_train), (X_test, y_test) = mnist.load_data()

X_train = X_train.reshape(60000, 784).astype('float32')
X_test = X_test.reshape(10000, 784).astype('float32')

X_train /= 255
X_test /= 255

n_classes = 10
y_train = keras.utils.to_categorical(y_train, n_classes)
y_test = keras.utils.to_categorical(y_test, n_classes)
```

Design Neural Network architecture

```python3
#Create an instance of the class
model = Sequential()

#Create each layer
model.add(Dense(64, activation='relu', input_shape=(784,)))
model.add(Dense(64, activation='relu'))
model.add(Dense(10, activation='softmax'))

#Chech your model architecture
model.summary()
```

Configure your model and train it.
```python3
model.compile(loss='categorical_crossentropy', optimizer=SGD(lr=0.1), metrics=['accuracy'])

model.fit(X_train, y_train, batch_size=128, epochs=200, verbose=1, validation_data=(X_test, y_test))
```
