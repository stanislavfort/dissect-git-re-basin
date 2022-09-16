# (Tentatively?) failing to replicate the key effect from the Git Re-Basin paper (https://arxiv.org/abs/2209.04836)

by [Stanislav Fort](https://twitter.com/stanislavfort) on Sep 15 2022

I have been really intrigued by the Git Re-Basin [paper](https://arxiv.org/abs/2209.04836) and was really pleased that the authors provided a [codebase](https://github.com/samuela/git-re-basin). Since the code wasn't ready for a one-click-replication, I decided to make Colabs that could be run from top to bottom using bits and pieces of the authors' code, trying. to observe the linear connectivity of the permuted network with the one towards which it was permuted. 



**tldr: Even starting from the authors' own code, I wasn't able to replicate the key observation that the permutation applied to a network moves it to a linearly connected convex basin of another network, suggesting a single basin for all networks module symmetries.**



I have two Colabs, one attempting to replicate it on [MNIST with an MLP](https://github.com/stanislavfort/dissect-git-re-basin/blob/master/git_rebasin_Stan_MLP_MNIST.ipynb) and the other using a [ResNet on CIFAR-10](https://github.com/stanislavfort/dissect-git-re-basin/blob/master/git_rebasin_Stan_ResNet_CIFAR10.ipynb). You should be able to run both within minutes on a free GPU. If you are time-limited, the MNIST with an MLP is **significantly faster** to run, so I would start with that.

The key plots you'll be getting are:

<img src="https://github.com/stanislavfort/dissect-git-re-basin/blob/master/rebasin_summary_MLP_MNIST.png?raw=true" ALIGN="center" height="100%" width="100%">





<img src="https://github.com/stanislavfort/dissect-git-re-basin/blob/master/rebasin_summary_resnet_cifar10.png?raw=true?raw=true" ALIGN="center" height="100%" width="100%">



To understand the images, let's first discuss what happens in the Colabs (I'm heavily relying on the author's training and permutation generating code here):

1. Train a first network, I call it **Model 1**
2. Train a second network, I call it **Model 2**
3. Develop a special permutation that takes the weights of Model 2 and turns it closer towards Model 1, I call the resulting model **Model 2 + permutations**

The key claim in the paper is that **Model 1** and **Model 2 + permutations** are now **within the same linearly connected convex basin on low loss**. This is unexpected, because certainly Model 1 and Model 2 weren't, and the permutation was such that Model 2 + permutations is *literally* the same function as Model 2. Provided that this is the case, the authors draw a strong and surprising conclusion that all networks might actually secretely be in the same low-loss, convex, linearly connected basin and that the appearance of multiple basins might be a chimera due to the very many symmetries of the weight space.

In my replacation, however, I was not able to see that the Model 2 + permutations would become linearly connected to Model 1. I ran the ResNet for 100 epochs until a fairly low loss / high accuracy (and the MNIST MLP was also well optimized) yet there is still a sizeable bump when I linearly walk in the weight space from Model 1 (red circle) to Model 2 + permutations (aqua triangle) on the yellow linear cut. The bump is actually comparable to walking between the Model 1 (red circle) and Model 2 (blue square) that are completely independent.

Also, if the effect were there, you should see **2 distinct low loss basins** (since the Model 2 + permutations should be living in a single one together with Model 1) in the plots above, while it is clear that there are **3 distinct low loss basins** there.

**Takeways:**

It is strange that I wasn't able to replicate **the key finding** of the Git Re-Basin paper. **If you see an error in my Colabs that would explain it, let me know on Twitter @stanislavfort**. On the other hand, if the effect is indeed so brittle that I wasn't able to see if *even starting from the author's very code* it might not be such a conceptual shift as I (and many others) were expecting. However, it is still possible that I made a mistake somewhere in my replication which is causing this.
