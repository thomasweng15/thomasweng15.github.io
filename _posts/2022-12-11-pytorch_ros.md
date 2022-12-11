---
layout: post
title: Deploying Python 3 PyTorch Networks in Python 2 for ROS
author: Thomas Weng
comments: true
tags: 
- how-to
- robotics
---

Scenario: you train a neural network in Python 3 with PyTorch, and want to deploy your network on a real robot using [ROS](https://wiki.ros.org/). However, ROS only supports Python 2 packages, and getting ROS to work with Python 3 is difficult.[^1] 

While [ROS 2](https://docs.ros.org/en/rolling/index.html) does have support for Python 3, it is still in development. In the meantime, here is a possible workaround for running Python 3 networks on ROS 1.

The workaround is to load the weights of a Python 3 PyTorch model, process the weights into a format compatible with Python 2 PyTorch, then save the output as a text file to be loaded in Python 2. 

In the following gist, `module_save_util.py` processes the Python 3 model and saves the weights for Python2: [link to gist](https://gist.github.com/thomasweng15/f0de0a3a0e91e5d50c867274dae0d821). `module_load_util.py` can then be called in Python 2 to load the processed weights.

In our case, we trained networks with PyTorch 1.8.0/1.8.1 and deployed with PyTorch 1.3.1. I cannot give any guarantees on whether the performance is the same after porting the weights to Python 2, though in my own experiments I did not see a drop. That said, it has been a while since I have done this solution so your mileage may vary. 

Others at the Robotics Institute have proposed the following solutions, though I haven't tried them myself:
* Write your learning node in C++ and load your PyTorch model with libtorch
* [https://robostack.github.io/](https://robostack.github.io/)
* [Medium: How to setup ROS with Python 3](https://medium.com/@beta_b0t/how-to-setup-ros-with-python-3-44a69ca36674)

Credit goes to [Sujay Bajracharya](https://www.linkedin.com/in/sujay-bajracharya/) for developing the solution in this post! If there is code in the gist above from other sources, I am happy to give credit as well.

Full gist:
<script src="https://gist.github.com/thomasweng15/f0de0a3a0e91e5d50c867274dae0d821.js"></script>

---
Footnotes

[^1]: [ROS Answers: Python3 and python2 on package](https://www.notion.so/Deploying-Python-3-PyTorch-Networks-in-Python-2-for-ROS-4af07223085546649141b6c4df40c05e)