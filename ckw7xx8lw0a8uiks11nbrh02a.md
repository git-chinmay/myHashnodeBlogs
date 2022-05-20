## Multicollinearity

Everyone should be aware of this term in the machine learning field. Most of knows this but I’ve seen many professionals unaware that multicollinearity exists. This is especially prevalent in those machine learning folks who come from a non-mathematical background. And while yes, multicollinearity might not be the most crucial topic to grasp in your journey, it’s still important enough to learn. Especially if you’re sitting for data scientist interviews!

In this blogpost we will try to understand what is this MultiColinearity all about. we will become lazy and use short form MC for the term Multicollinearity.

**What is Multicollinearity?**

If you search the answer in any statistics book more or less the definition will be something like this "Its a phenomenon in a multiple regression model where one predictor variable can be linearly predicted from the others with a substantial degree of accuracy". In simple terms let we have a dataset where we have multiple independent variables like x1, x2... and a dependent variable y . If x1 and x2 are exhibiting collinearity means change in x1 causing automatic change in x2 then its called multicollinearity.


**What is the negative impact of Multicollinearity?**

MC's impact mainly observe on the Regression problems. Although it does not affect the accuracy of the model that much but its affect its reliability. we cannot say which feature actually making the more impact to target variable. We lose the interpretability.

Example:   Consider this equation Y = a1.x1 + a2.x2

Here if either x1 or x2 increase(considering other one fixed) that will make an effect to Y but if x1 and x2 are corelated then change in x1 makes automatically changes to x2 and then Y get changed due to both. We cannot say actually which variable make the changes to Y as an outsider.

understanding with real life example: 
When Robin watches TV he eats chips which makes him happy. The longer he watches TVs , longer eats chips and longer he stays happy. Here we cannot say that which one, TV or Chips makes Robin Happy. Here Watching TV and eating chips are highly corelated.


**What cause Multicollinearity problem?**

There are several reasons.

	- At the time of dataset creation we may have collected corelated features.

	- Bad Feature engineering. Ex: Creating BMI feature from weight and height feature which is redundant.

	- Dummy variable trap : Creating encoding separately for Male and Female columns which is highly redundant. We could represent both columns as a single gender column with 0 and 1.

	- Some times less amount of data also cause MC problem.

How to detect the Multicollinearity in dataset?

There are number of options available like using scatterplot and colinear matrix but VIF is the most common, reliable and easier one. 

VIF stands for Variance inflation factor. If you would like to deep dive what is VIF then read this   [blog](https://statisticsbyjim.com/regression/variance-inflation-factors/) . VIF determines the strength of the correlation between the independent variables. It is predicted by taking a variable and regressing it against every other variable. 

VIF = 1/1-R^2  

```
Where R^2 = 1 - (Ssres/Sstotal)  
R^2 : Determinant of coefficients
Ssres= Sum of square of the residual = Sum(y-yavg)
Sstotal = Sum of square of the total = Sum(y-y^) 
If R2 = 1 then VIF is high means variables are strongly corelated and vice versa.

``` 

Lets understand this with a practical example. We have a dummy dataset for employees salary.

![pic1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637418541896/Zv91_yZut.png)

Now we will run the VIF for each column and see the output

![pic2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637418610499/gfsQbOUYT.png)

VIF starts with 1 and if it goes beyond 2-5 then the variables are highly corelated. Here Age and Years of Service has more than 10. It indicates that these two variables can be easily predicted by others variables of the dataset.

Lets drop one of the variable Years service and recalculate the VIF for each column.

![pic3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637418709656/368Y_qLjc.png)

We can see that Years of service column's VIF reduced significantly. To remove the MC and use the dataset we will combine both the Age and Years of the service data and make it as one feature. Feature engineering is recommended than dropping the feature itself.

![pic4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637418845428/8LxjuwZlr.png)

Again calculating the VIF

![pic5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1637418893622/as9deMvDx.png)

**Notes:**

- 
Dropping variables should be an iterative process starting with the variable having the largest VIF value because its trend is highly captured by other variables. If you do this, you will notice that VIF values for other variables would have reduced too, although to a varying extent.

- MC may not be an problem every time. If you are more focused on individual feature's impact on target variable rather than group of features then removing MC is necessary.

 [GitHub link for above example.](https://github.com/git-chinmay/deep_learning_python_book_excs/tree/master/multi_colinearity) 

Hope you have liked this blogpost. Happy Learning!



