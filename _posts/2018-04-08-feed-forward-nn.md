---
published: true
layout: post
title: Implementing and Testing a Feed Forward Neural Network
---
I implemented and tested a feed forward neural network using pytorch in this project. [(Link to Github Repo of Source Code)](https://github.com/aakashpydi/FeedForwardNeuralNet). I tested my implementation using, AND, OR, NOT and XOR networks. [Link to this post on medium.](https://medium.com/@aakashpydi/implementing-and-testing-a-feed-forward-neural-network-a2178daf27df?source=friends_link&sk=4e397496e68d502e01ad3de943a3be09)  

The forward propagation neural network is implemented in neural_network.py. The functionality of the classes defined in logic_gates.py relies on this neural network. The class representations of the AND, OR, NOT, and XOR gates is given in logic_gates.py.

The implemented classes were tested in test.py. The output of executing test.py is given below.

![]({{site.baseurl}}/images/feedforwardnn_images/test_output.png)
