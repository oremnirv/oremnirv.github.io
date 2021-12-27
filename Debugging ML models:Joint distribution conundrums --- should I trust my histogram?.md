To get intuition about the fit of an ML model to the ground truth distribution (assuming we have a crystal ball) and where our model might be going haywire, we typically make plots --- for example... 

Currently, we do not have a simple way (we may do T-SNE) to plot joint distributions and compare them. So instead, we use certain charateristic plots from the joint distribution. Meaning, if two joint distributions coincide, their histograms should too. 

Let's take a specific example (simplified and imaginary):
In London, in a specific 'representative' latitude and longitude in 
a given month ($t \in {Jan, Feb, .., Dec}$) we have its associated average temperatures in (˙C) $y_t \in R$. We would like to know the monthly average temperatures for the year 2020 given the data for years 2010-2019. Now, it is the modeller's job to invent a probability model. Ignoring the detials of the specific model, we will just say we have decided to model the joint distribution without any indpendence assumptions through a model named M1. i.e, we would like to assess $P(y_Jan, y_Feb, .., y_Dec| M1)$ in 2020. This joint distribution is in 12 dimensions and we can't visualize it. But, if the model's output joint distribution matches the ground truth joint distribution, then the histogram for a typical month should match as well. To construct a typical month, we just take many samples from the joint distribution $P(y_Jan, y_Feb, .., y_Dec| M1)$ and plot them all in a histogram. But what if they don't? Does that mean that we should search for an alternative model? 

A very simple experiment would iiluminate the conundrum. 
Let's look at a 2D normal distribution of two indepndent r.v X and U as in figure XXX each distributed according to N(0, 1). Since X and U are independent, we know that P(X|U=u) = P(X) for any value of u. i.e, we expect to see P(X|U=u) ~ N(0, 1). This is also true for any range of values for u. Informally, when can think of ranging u values as a concatention process. First sample many values from P(X|U=1.96), then concatenate many values from P(X|U=1.961) etc. Since each one of those is distributed as N(0, 1) the concatenation of their values is still N(0, 1).        

Let's take then u>1.96 and u<2.04 and check if the histogram of P(X|u) is equal to the histogram of P(X). We can see in figure XXX that they don't. This is still true for samples of size 100,000,000 points from X and U each.

Here, we have an example that mathematically we know should match -- P(X) must have the same distribution as P(X|U=u) and yet practically they aren't always the same.   

This example is a cautionary tale not to dismiss a model just because it doesn't have the exact same histogram.  

So in what situations should we expect it to actually match? 
In the example we ran above we choose arbitrarly a range for the u values. If we were to choose a different range, we would see that the histograms do not match. 
Although, they should. So there is some uncertainty in the produced histograms, which we can in turn quantify. 

If however, we would change the range in our model (e.g. different slices in non overlapping times) and the histograms do look the same (for "enough" samples of different ranges) then we at least know that our model produces on kind of histogram. If our model represents the ground truth, we would expect the histograms to coincide. Otherwise, we would need to think how to debug our model --- but that doesn't mean that our new model's histogram would then match the ground truth histogram.   


  
    