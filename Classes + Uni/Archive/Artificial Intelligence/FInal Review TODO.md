Regularization manifests itself in the w vector, the vector will be sparser and some elements will be close to 0. the higher order terms limit the linear model.

there is a connection between cross entropy and mean square error. there is a way to connect them and we must know how. 

binary classification the only hard thing is deriving the binary cross entropy formula, and associating it with the entropy which we have seen before.

we also saw that binary cross entropy penalizes confident wrong decisions.

what is per example loss?

Problem we will have: Maximum likelihood problem (given a probability distribution -> come up with expression about w*'s for that distribution)

logistic regressor is a linear model generalized by the precesne of non linearity because the posterior is always 0 or 1

we define a fully connected neural network

![[Screenshot 2025-10-17 at 7.14.12 AM.png]]

general version of cross entropy fraction when we have multiple classes, maybe we need to know how to derive cross entropy or log [[Log-Likelihood]] with more than 2 things in it?

understood what back prop is doing

a convolutional neuron is a filter which is a linear operation followed by addition followed by relu or some non-linearity

we have many cnns inside a layer so we have many outs

we need to understand how many parameters we have in a layer and how to calculate given the shape of like depth and stuff get the parameters for that layer which is + the weights

residual networks are example picturizers that solve the gradient problem and open flexibility + ensemble learning issue 

using resnets together with regression and classifiers to build object detectors. 

we have some understanding on metrics, like IoU, how average pressision is defined.

then we learned about the rcnn to define proposals we can submit to classification/regression branches

