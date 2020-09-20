# License Plate Detection & Recognition

License plates are intrinsically rectanglar and planar objects, which are attached to vehicles for identification purposes. To take advantage of its shape, the noval CNN called **Warped Planar Object Detection Network** was proposed.

This network learned to detect LPs in a variaty of different distortions, and regresses coefficients of an affine transformation that upwarps the distorted LP into a retangular shape resembling a frontal view.

![Image](/images/p1.png)


The proposed architecture has a total of 21 convolutional layers, where 14 are inside residual blocks. The size of all convolutional filters is fixed in 3 by 3. ReLU activations are used throughout the entire network, save for in the detection block. There are 4 max pooling layers of size 2 by 2 and stride 2 that reduces the input dimensionality by a factor of 16.

Finally the detection block has two parallel convolutional layers:<br>
1. inferring the probability, activated by a softmax function;<br>
2. another for regressing the affine parameters, without activation.<br>
![Image](/images/p2.png)
