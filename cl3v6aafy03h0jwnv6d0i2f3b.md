## Fukushima Disaster

### Introduction

Linear regression is an attempt to build a model from existing data. It uses the existing data to fit a linear equation that exhibits the least error from the actual points. Even within the existing data, a regression equation has limited ability to predict actual dependent variable values. Here we will see how a wrong assumption led to a catastrophic nuclear disaster. Here we will basically do the white paper study originally published by Munich Personal RePEc Archieve(MPRA).

Linear regression is limited in several ways. Like..
 

- It assumes only linear relationships between variables.
- It is very sensitive to outliers.
- Data points must be independent. etc.

If any of these assumptions are not met, the models get less accurate. The value of the dependent variable cannot be legitimately estimated if the value of the independent variable is outside the range of values in the sample data that served as the basis for determining the linear regression equation. There is no statistical basis to assume that the linear regression model applies outside of the range of the sample data.

Even though there is a strong correlation exists between x, y we can not confirm there is also a strong causation. It might that both the dependent and independent variables are equally affected by third unknown factor.

The weaknesses in linear regression are all magnified the further from the mean values we get. Since prediction lies beyond the sampled data, it is as far from the mean as we can get, and thus the errors are compounded at their largest state.

### What went wrong?

The safety analysis for the Fukushima nuclear power plant was based on historical data dating back to 1600 CE. The plant was designed to withstand a maximum earthquake of 8.6 magnitude, and a tsunami as high as 5.7 meters. The earthquake on March 11, 2011 measured 9.0 and resulted in a >14-meter-high tsunami.

The design basis was developed from a mistake in the regression analysis of the historical earthquake data. The structural engineers responsible for the design overfit their model to the existing data, rather than use the accepted Gutenberg-Richter model, they saw a kink in the data and assumed that the appropriate regression was not linear, but rather a polynomial. The engineers, in their pursuit of following the data too closely, came up with a very different conclusion.

What Fukushima engineer’s polynomial regression model saw


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654062132534/PreJPy4HK.png align="left")

What the Gutenberg-Richter liner regression model saw from the same data

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654062108845/uXzy8k-Fw.png align="left")

The Gutenberg-Richter method uses a simple linear regression to predict frequency versus magnitude based on historical data for a region and has been proven correct. The Gutenberg-Richter model clearly predicts that higher magnitude earthquakes are possible, and that a magnitude 9 earthquake is about 70 times more likely than predicted (7.1x10-3 versus 1x10-4).

### Conclusion

The non-conservative assumption made by these engineers resulted in a design that was not able to withstand neither a strong enough earthquake, nor a high enough wave. The damage that resulted from this event is widely known. This apparently small assumption has resulted in a huge impact to Japan’s economy, and should be a lesson to all on the power of predictive models.

I hope you have liked what you read here. Happy learning!
