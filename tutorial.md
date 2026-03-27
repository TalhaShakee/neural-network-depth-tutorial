# How Does Neural Network Depth Affect Learning?
## A Tutorial on Multi-Layer Perceptrons and Network Architecture

### Introduction

Deep learning has significantly changed the landscape of machine learning, but still the question is: **why do deeper networks perform better?** This tutorial will explore the relation between the depth of neural networks and learning performance through experimentation.

In this tutorial, we'll use the digits dataset to train different depths of Multi-Layer Perceptrons (MLPs) and visualize the performance. This tutorial will help you understand how to make the best use of the depth of neural networks in your own projects.

**Key Question:** Does a deeper network always learn better, or are there optimal depths for different problems?

---

## Part 1: Understanding Network Depth

### What is Network Depth?

Network depth simply means how many hidden layers your neural network has.

- A **shallow network** (1-2 layers) can only learn simple patterns directly from input
- A **deep network** (5+ layers) can learn layer by layer representations, where each layer learns something different

```
Shallow Network (1 layer):    Deep Network (5 layers):
Input → Hidden → Output       Input → H₁ → H₂ → H₃ → H₄ → H₅ → Output
```

### Why Depth Matters

The basic idea is simple: if you stack more layers, each layer can learn features from the previous layer. For example:
- Layer 1 might learn edges from pixels
- Layer 2 might learn shapes from edges
- Layer 3 might learn parts from shapes
- Layer 4 might learn objects from parts

This **hierarchical learning** is powerful because each layer builds on what came before it.

### The Problems with Deep Networks

However, deeper networks come with challenges:

1. **Gradients disappear**: When you backpropagate errors through many layers, the signal gets weaker and weaker. By the time it reaches the first layer, there's almost nothing left to learn from.

2. **More parameters, more overfitting**: More layers means more weights to learn. With a small dataset, the network can memorize instead of generalize.

3. **Slower training**: Deeper networks take longer to train because there's more computation to do at each step.

---

## Part 2: Our Experiment

### What We Did

We trained 7 different MLPs, each with a different number of hidden layers:
- 1 hidden layer
- 2 hidden layers
- 3 hidden layers
- 4 hidden layers
- 5 hidden layers
- 7 hidden layers
- 10 hidden layers

### How We Made It Fair

To see only the effect of depth, we kept everything else the same:
- **Same layer width**: 128 neurons in each hidden layer
- **Same activation function**: ReLU (helps with deep networks)
- **Same optimizer**: Adam (handles deep networks well)
- **Same dataset**: Handwritten digits (8×8 images)
- **Same data split**: 80% training, 20% testing

### What We Measured

For each network, we tracked:
- How accurate it was on training data
- How accurate it was on test data
- How the loss changed over training epochs
- The gap between training and test accuracy (overfitting)

---

## Part 3: What We Found

### Finding 1: Depth Helps, But Not Infinitely

When we trained all the networks, here's what we got:

```
Depth (layers)    Train Accuracy    Test Accuracy
1                 99.65%            96.94%
2                 99.86%            96.94%
3                 99.72%            96.94%
4                 99.79%            96.94%
5                 99.30%            96.67%
7                 99.65%            96.39%
10                98.89%            95.28%
```

**What this tells us**: The test accuracy stayed the same from 1 4 layers, then started dropping. This means for this task, going deeper doesn't help. A shallow network (1 layer) was just as good as deeper ones.

### Finding 2: Deeper Networks Train Slower

Here's how many epochs each network needed to train:

```
Depth 1:  121 epochs
Depth 5:  61 epochs
Depth 10: 61 epochs
```

**What this tells us**: Interestingly, the deeper networks converged faster in terms of epochs, but they take longer in real time because each epoch involves more computation. So you get slower wall clock time even if epoch count is lower.

### Finding 3: Overfitting Gets Worse with Depth

The gap between training accuracy and test accuracy (overfitting):

```
Depth 1:  0.71% gap
Depth 2:  0.92% gap
Depth 3:  0.78% gap
Depth 10: 3.61% gap
```

**What this tells us**: Deeper networks overfit more. The 10 layer network memorized the training data much more than the 1 layer network did. This is expected because 10 layers has way more parameters to play with.

---

## Part 4: What This Means For You

### When Should You Go Deep?

Use deeper networks when:
- Your task is genuinely complex (image recognition, natural language, etc.)
- You have lots of training data (thousands or millions of examples)
- You have time and computing power to spare
- You're using modern techniques like batch normalization to help training

Skip deep networks when:
- Your task is simple (like our digit classification)
- You have limited data (a few hundred examples)
- You're on a tight budget for computation
- You're just starting debug with shallow networks first

### A Simple Rule

**Start shallow, then add depth only if you need it.** Here's the process:

1. Build a 2 layer network and see how it performs
2. Try 3, 4, 5 layers and track the accuracy
3. Stop adding layers when accuracy stops improving
4. Don't add layers just because they exist

For our digit task, that means stopping at 1-2 layers. For ImageNet or natural language processing, you'd need 50+ layers.

### How to Train Deep Networks Better

If you do decide to go deep, use these techniques:

1. **Batch Normalization**: Normalize the inputs to each layer so they don't get weird values
2. **Skip Connections**: Let information bypass some layers, like in ResNets
3. **Good Weight Initialization**: Start with weights in a sensible range
4. **Better Optimizers**: Use Adam instead of basic SGD
5. **Dropout**: Randomly disable neurons during training to prevent memorization
6. **Layer Normalization**: Another way to keep layer inputs stable

---

## Part 5: Try These Yourself

Now that you understand depth, try modifying our code to explore:

### Experiment 1: What About Width?
Keep the depth at 3 layers, but change the number of neurons per layer (try 32, 64, 128, 256). Does width matter as much as depth?

### Experiment 2: Harder Dataset
Use MNIST (bigger 28×28 images) instead of digits. Does depth help more on a harder task?

### Experiment 3: Different Activation Functions
Replace ReLU with tanh or sigmoid. How do different activation functions change the optimal depth?

### Experiment 4: Add Regularization
Use the `alpha` parameter in scikit-learn to add L2 regularization. Does this change which depth is best?

### Experiment 5: Batch Normalization
Add batch normalization between layers. Can it help the 10 layer network perform better?

---

## Part 6: The Science Behind This

This work builds on decades of research. Here are the key papers:

1. **LeCun et al. (1998)** - The original work on neural networks for digit recognition. They showed networks could learn from raw pixels.
   - [Gradient-based learning applied to document recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf)

2. **Bengio et al. (2013)** - This paper explained why deep networks are hard to train. They showed that gradients get smaller as you backpropagate, making it hard for early layers to learn.
   - [On the difficulty of training deep feedforward neural networks](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf)

3. **He et al. (2016)** - ResNets introduced skip connections, which let information bypass layers. This made it possible to train very deep networks (100+ layers).
   - [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)

4. **Ioffe & Szegedy (2015)** - Batch Normalization is now standard practice for training deep networks. It stabilizes the values flowing through the network.
   - [Batch Normalization: Accelerating Deep Network Training](https://arxiv.org/abs/1502.03167)

5. **Glorot & Bengio (2010)** - This paper showed that how you initialize network weights matters a lot, especially for deep networks.
   - Understanding the difficulty of training deep feedforward neural networks

---

## Conclusion

After running this experiment, here's what we learned:

**Depth is not magic.** A deeper network doesn't automatically learn better. What matters is:

1. **The task complexity**: Simple tasks (like digits) don't need deep networks. Complex tasks (like ImageNet) do.

2. **Your data**: You need enough data to take advantage of deep networks. Otherwise, the extra layers just memorize.

3. **Your resources**: Deep networks are slower and need more computation. If you're resource constrained, go shallow.

4. **Your technique**: Modern tricks (batch norm, skip connections) make deep networks work better. Without them, very deep networks struggle.

The real lesson: **Experiment and measure.** Don't assume deeper is better. Train networks of different depths on your task, measure the results, and pick the one that works best.

Start with shallow networks. Add depth incrementally. Stop when you stop seeing improvement. This empirical approach will serve you well in all your machine learning projects.

Happy learning!

---

**GitHub Repository**: [https://github.com/TalhaShakee/neural-network-depth-tutorial](https://github.com/TalhaShakee/neural-network-depth-tutorial)
