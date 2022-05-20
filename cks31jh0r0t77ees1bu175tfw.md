## Anatomy of a Neural Network

If you see an artificial neural network (ANN or simply NN), we can tell its consists of below four main things

- Layers
- Input data corresponding to targets
- Loss function
- Optimizer

Lets see about these components

**Layers**

It is the fundamental data structure in NN. A layer is a data processing module that takes as input one or more tensors & the outputs as one or more tensors. The layer has a state, the layer's `weight`, one or several tensors learned with stochastic gradient descent, which together contains the network's `knowledge`. Different layers are appropriate for different tensor formats & different type of data processing. For instance 

- simple vector data stored in 2D tensors of shape `(sample, features)` is often processed by `densely connected or fully connected` layers. 
- Sequence data stored in 3D tensors shape (samples, timestamp, features) is typically pro9cessed by `recurrent layers` such as a LSTM layer.
- Image data stored in 4D tensor is usually processed by 2D `convolution` layers.



```
from keras import layers
layer= layers.Dense(32, input_shape= (784,))
``` 
In above code we are creating a layer that will only accept as input 2D tensors where the first dimensions 784 and the batch is unspecified, and thus any value would be accepted. This layer will return a tensor where first dimension has been transformed to 32.

**Models: Networks of Layers**

The topology of a network defines the `hypothesis space`. *We can define Machine learning as Searching the useful representation of input data within a predefined space of possibilities , using guidance from a feedback signal.* By choosing a network topology, we can constrain the space of possibilities or hypothesis space to a specific series of tensor operations, mapping input data to output data. What we will be searching for is a good set of values for the weight tensors involved in these tensors operations.

Note: Picking the right network architecture is more an `art` than science.

**Loss functions & Optimizations**

These two are keys to configuring the learning process. Once the network architecture is defined, we still have to choose these two

- Loss Function or Objective function
- Optimizer

A  neural network that has multiple output may have multiple loss functions (one per output). But the gradient descent process must be based on a single scalar loss values, so for multi loss network, all losses are combined via averaging into a single scaler quantity.

Choosing the right objective function for the right problem is extremely important. Your network will ruthlessly take any shortcut it can to minimize the loss, so if the objective does not fully correlated with success for the task at hand, your network will end up things you may not have wanted. 

So these are basic things we have to consider while dealing with a deep learning neural networks, hope you have like the blog. Happy learning.



