---
published: true
layout: post
title: Using a Generative Adversarial Network (GAN) to Create Novel Artistic Images
---
In this project I used a Generative Adversarial Network (GAN) architecture to generate NEW artistic images that capture the style of the Indian artists, Raja Ravi Varma [1848 - 1906] and Sattiraju Lakshmi Narayana (Bapu) [1933 - 2014]. [(Link to Github Repo of Source Code)](https://github.com/aakashpydi/GenerativeAdversarialNetwork).

**Introduction:** The objective of this project was to understand Generative Adversarial Network (GAN) architecture, by using a GAN to generate NEW artistic images that capture the style of a given artist(s). The Generative Adversarial Network (GAN) architecture was introduced in 2014 by Dr. Ian GoodFellow
[(link to paper)](https://arxiv.org/abs/1406.2661). GANs consist of generator and discriminator neural networks. The generator neural network takes random noise as an input, and generates an image as an output, whilst the discriminator neural network takes an image as an input and produces a probability value as an output. The probability value evaluated by the discriminator indicates the probability of the input image being a part of the training set. The objective of the generator is to ‘fool’ the discriminator with the images it generates, whilst the objective of the discriminator is to correctly ‘find’ the images produced by the generator. This adversarial, zero sum nature of the competing networks, leads to the generator increasingly producing images that probabilistically have a greater likelihood of belonging to the training set. An example architecture for a GAN is given below.

![]({{site.baseurl}}/images/gan-bapu-varma-images/gan-example.JPG)

**Other Works:** I was introduced to GAN architecture when I came across a 2017 paper by Nvidia scientists, titled [‘Unsupervised Image to Image Translation Networks.’](https://arxiv.org/pdf/1703.00848.pdf) They used a variant of GAN (coupled GAN) architecture to train models that could effectively take an input image of (i) a photograph taken in day time, and produce a convincing output image of the photograph in night time, (ii) an animal, and produce a convincing output image of the animal morphed into a different animal, (iii) an individual, and produce a convincing output image of an individual with a feature added (changed hairstyle, adding accessories). An example from their paper is given below. For each pair of images, the left image is the input and the right image is the output.

![]({{site.baseurl}}/images/gan-bapu-varma-images/nvidia-paper.JPG)

To further understand this model, I found a 2017 paper titled [‘Unpaired Image to Image Translation using Cycle-Consistent Adversarial Networks’](https://arxiv.org/pdf/1703.10593.pdf) by scientists at Berkeley. They also used a variant of GAN (cycle GAN) architecture to train models that could effectively take an input image of (i) a photograph, and produce a convincing output image of the photograph in the style of various painters, (ii) a photograph taken in summer time and produce a convincing output image of the photograph in winter time. An example from their paper is given below.

![]({{site.baseurl}}/images/gan-bapu-varma-images/berkeley-paper.JPG)

The GAN models discussed so far essentially convert an input image to a modified output image that has some desired features. I wanted to see if GAN architecture can be used to generate NEW artistic images that capture the style of a given artist(s). I subsequently found a 2017 independent research project of a couple of individuals at Facebook titled [‘GANGogh: Creating Art with GANs’](https://towardsdatascience.com/gangogh-creating-art-with-gans-8d087d8f74a1). They used GAN architecture to (i) understand the style of various artists and then (ii) create a novel application of learned styles to generate novel art.

![]({{site.baseurl}}/images/gan-bapu-varma-images/facebook-paper.JPG)

**My Contribution:** I hope that my main contribution through this project will be to provide developers who are (i) short on time, (ii) short on processing power (iii) want to use small, custom datasets, and (iv) produce novel, impressionistic, artistic images capturing the style of a given artist, an approach through which to learn about GAN architecture. Time and processing power were a big constraint for me as I had to start my project work very late on a slow virtual machine. I was thereby interested in dramatically cutting down the time taken for training a model. I did this by using a very, small custom dataset (~200
images). However, the tradeoff associated with this choice is that, it becomes much more likely that the generator or the discriminator ‘overpowers’ the other (ie the generator learns much faster than the
discriminator, or the discriminator learns much faster than the generator). This degenerates the output image into noise. I addressed this issue by introducing the use of conditional backpropagation. The loss associated with a net was backpropagated when above a certain threshold value. I found this to be effective by analyzing the loss generated by the models. This approach allows for the generation of
novel impressionistic images that effectively capture the style of a given artist with a relatively small training time and dataset. Note that the small dataset causes the generated images to be impressionistic in nature. However, this is okay, as we achieve the objective of capturing the style of the artist through a new impressionistic image, and dramatically cutting processing costs. The small dataset also allows us to capture the styles of smaller, regional artists without too many works. I used the cifar10 model for benchmarking. As the cifar10 dataset has 60,000 images, took dramatically higher processing time, and consequently produces less impressionistic images, it serves as a good contrast.

**Results:**

![]({{site.baseurl}}/images/gan-bapu-varma-images/results.JPG)

![]({{site.baseurl}}/images/gan-bapu-varma-images/analysis.JPG)

**Discussion:** We can see that the images produced are impressionistic in nature. The Ravi Varma model and the Bapu model, were particularly effective at generating novel impressionistic images in the style of the artists.
The combined model clearly has a poorer impressionistic quality. We can infer that this is likely because the discriminator can learn a lot faster in this case, as there are two dramatically contrasting styles, in a very small dataset (~400 images). This inference is buttressed by the loss graphs given above. Notice how the generator and discriminator loss take a substantial deviation from each other at one point in the graph corresponding to the combined model. This is despite the conditional backpropagation. Clearly, we need more data when attempting to generate novel impressionistic images in the style of MORE than one artist. Consider the cifar10 benchmarking example. We can see that the output images are far less impressionistic despite having 10 different class labels. This is due to the much larger dataset size (~60,000 images). However, the key metric for us to consider is the training time per iteration given above. Notice how training the Cifar10 model took a dramatically higher time than the other models. We were thereby able to (i) cut training time substantially, (ii) use small, custom datasets and (iii) create novel impressionistic images in the style of a given artist.

[(Link to Video Presentation)](https://www.youtube.com/watch?v=4UGvb_A2p7U)
