In terms of the recent works on scene text detection pipeline, (from CRAFT paper):
![Image](/images/5.png)

**Character Region Awareness For Text Detection**, <a href="https://arxiv.org/abs/1904.01941">CRAFT</a>. The challenges associated with text detection, rather than regular object detection: huge aspect ratio variation, different fonts & backgrounds, skewed & curved text, and colored text.<br>
Key idea in CRAFT: A word is a collection of letters -> Use the affinity between letters to detect words. The authors are repropose idea: **Text detection problem -> Segmeantation problem**

The outputs of the CNN in CRAFT: 1. Region Score; 2. Affinity Score. (they are grey-scaled image, or can say a 2-channels image).

If have the output label and input image, we can train a CNN; (input a image, output a mask). 
So **how to generate the two maps for training data**?

##  **1. Similarity to Segmentation problem**
Before that question, let's look at the U-Net for semantic segmentation, and CRAFT use a very similiar network to solve this problem:
![Image](/images/3.png)

##  **2. What's the two maps menas?**
![Image](/images/2.png)
**1. Region score**
Basically indicates that these locations have a character in them and thery are centered at this point of highest probability.

**2. Affinity score**
Calculates the affnity between characters, or say the two letters are close together if there is high affinity at this location or part of same word.

##  **3. Generate Ground Truth**
![Image](/images/4.png)
Suppose we've got the bounding boxs around the letter (**character boxes**), then the **affinity boxes** are generating as the figure above.<br>
And the score generation module: warp a 2-D gaussain distribution based on the perspective transform between the boxes.<br>

**Problem**: large public datasets contain only word level segmentation!<br>
**Solution**: Synthesize the text examples. <br>

The authors used a semi-supervised approach:
1. crop out the word-level text
2. run through the network they trained on Synthetic data to get various region score
3. use watershed to get a segments
4. fit bounding boxes for segments (pseudo ground truth)

but cannot trust this data completely. A clever solution:<br>
If there is real data that is being used and also know what is this text, or the number of characters in this bounding box.<br>
So once the above automated technique produces the right number of bounding boxes, given a higher weight compared to those segmentation were incorrect. So they can use the real data also.<br>
Semi-supervised learning: using synthetic data to train CNN which is the supervised part. But for the real part they use these tricks which is not supervised.<br>
![Image](/images/6.png)

# License Plate Detection & Recognition

License plates are intrinsically rectanglar and planar objects, which are attached to vehicles for identification purposes. To take advantage of its shape, the noval CNN called **Warped Planar Object Detection Network** was proposed.

This network learned to detect LPs in a variaty of different distortions, and regresses coefficients of an affine transformation that upwarps the distorted LP into a retangular shape resembling a frontal view.

![Image](/images/p1.png)


The proposed architecture has a total of 21 convolutional layers, where 14 are inside residual blocks. The size of all convolutional filters is fixed in 3 by 3. ReLU activations are used throughout the entire network, save for in the detection block. There are 4 max pooling layers of size 2 by 2 and stride 2 that reduces the input dimensionality by a factor of 16.

Finally the detection block has two parallel convolutional layers:<br>
1. inferring the probability, activated by a softmax function;<br>
2. another for regressing the affine parameters, without activation.<br>
![Image](/images/p2.png)
