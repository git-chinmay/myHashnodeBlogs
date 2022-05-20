## Understanding Bayes theorem in plain English

In probability theory and statistics, Bayes' theorem (alternatively Bayes' law or Bayes' rule ) named after Thomas Bayes, describes the probability of an event, based on prior knowledge of conditions that might be related to the event. Despite being overloaded with seemingly complex concepts, it conveys an important lesson about how observations change our beliefs about the world.

We can explain the Bayes formula in pure English. Let's take it apart! 

This is the popular Bay's equation

![1.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642573081425/L-NO1f7UW.jpeg)

Essentially, the Bayes formula describes how to update our models, given new information. To understand why, we will look at a simple example with a twist: tossing a biased coin.

Suppose that we have a magical coin! When tossed, it can come up with heads or tails, but not necessarily with equal chance. The catch is, we don't know the exact probabilities. So, we have to perform some experiments to find that out. 

To mathematically formulate the problem, we denote the probability of heads with 𝑥.

What do we know about 𝑥? At this point, nothing. It can be any number between 0 and 1.

Instead of looking at 𝑥 as a fixed number, let's think about it as an observation of the experiment 𝑋. To model our (lack of) knowledge about 𝑋, we assume that each value is equally probable. This distribution is called the prior.


![5.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642573965542/v2OCrljo4.jpeg)

(Note that we are working with probability density functions, not distributions. The values of the density function are not probabilities, despite the notation!)

So, suppose that we tossed our magical coin, and it landed on tails. How does it influence our model?

We can tell that if the probability of heads is some 𝑥, then the likelihood of our experiment resulting in tails is 1-𝑥.

![6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642574149332/V3bjaGm9E.png)

However, we want to know the probability distribution with the condition and the event in the other way around: we are curious about our probabilistic model of the parameter, given the result of our previous experiment.

This is called the posterior distribution.

![7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642574230711/96XY_dBnT.png)

Now let's put everything together!

The Bayes formula is exactly what we need, as it expresses the posterior in terms of the prior and the likelihood.

![2.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642574290756/LUzKVstNH.jpeg)

It might be surprising, but the true probability of the experiment resulting in tails is irrelevant. Why? Because it is independent of 𝑋. Also, the integral of the posterior evaluates to 1. Here it is 0.5, but this can be hard to evaluate analytically in the general case.

![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1642574356562/pS7nYlPEV.png)

So, we have our posterior. Notice that it is more concentrated around 𝑥 = 0. (Recall that 𝑥 is the probability of heads.)

This means that if we only saw a single coin toss and it resulted in tails, we guess that the coin is biased towards that.

![4.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1642574455637/0kR3v4qD5.jpeg)

Of course, we can do more and more coin tosses to refine the posterior further.

To summarize, here is the Bayes formula in pure English. (Well, sort of.)

```
posterior ∝ likelihood x prior
``` 
Or, in other words, 

> the Bayes formula just describes how to update our models given new information!

Hope you liked the post. Happy learning!



