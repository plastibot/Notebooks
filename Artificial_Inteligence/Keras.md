

First import Keras. Import modules as you need them.

```python3
import keras
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers.normalization import BatchNormalization
from keras import regularizers
from keras.optimizers import SGD #Stochastic Gradient Descent
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

#Create each layer. By default kernel_initializer is set to 'glorot_uniform' which is good enough.
model.add(Dense(64, activation='relu', input_shape=(784,), kernel_regularizers.l2(0.01), kernel_initializer='glorot_normal')) 
model.add(Dense(64, activation='relu'))
model.add(Dense(10, activation='softmax'))

#Here is an example of a Conv3D Network LeNet style
model = Sequential()
model.add(Conv2D(32, kernel_size=(3, 3), activation='relu', input_shape=(28, 28, 1)))
model.add(Conv2D(64, kernel_size=(3, 3), activation='relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))
model.add(Flatten())
model.add(Dense(128, activation='relu'))
model.add(Dropout(0.5))
model.add(Dense(n_classes, activation='softmax'))

#Chech your model architecture
model.summary()
```

Configure your model and train it.
```python3
model.compile(loss='categorical_crossentropy', optimizer=SGD(lr=0.1), metrics=['accuracy'])

#or using adam for faster learning rate

model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

model.fit(X_train, y_train, batch_size=128, epochs=200, verbose=1, validation_data=(X_test, y_test))
```
