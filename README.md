# org.apache.commons.math4.stat.descriptive.rank.Percentile
**Constructor**
- **`Percentile()`**
Constructs a Percentile with the following defaults.
- **`	Percentile(double quantile)`**
Constructs a Percentile with the specific quantile value and the following default method type: Percentile.EstimationType.LEGACY default NaN strategy: NaNStrategy.REMOVED a Kth Selector : KthSelector

**Methods**

- **`double evaluate(double p)`**
Returns the result of evaluating the statistic over the stored data.
If weights haven't been set, it will return the non-weighted result.

- **`double evaluate(double[] values, double[] weights, double p)`**
Returns an estimate of the pth weighted percentile of the values in the values array.

- **`double evaluate(double[] values, double[] weights, int begin, int length, double p)`**
Returns an estimate of the pth weighted percentile of the values in the values array, starting with the element in (0-based) position begin in the array and including length values.

- **`setData(double[] values, double[] weights)`**
Set the data array with corresponding weights.

- **`setData(double[] values, int begin, int length)`**
Set the data array with corresponding weights in the given interval.

- **`Percentile withEstimationType(Percentile.EstimationType newEstimationType)`**
Build a new instance similar to the current one except for the estimation type.
Currently, Only R-7 support estimating weighted percentiles.

**Formula**    
Given an ordered sample ![formula](https://render.githubusercontent.com/render/math?math=X_1\le\X_2\le\cdot\cdot\cdot\le\X_n) with respective weights ![formula](https://render.githubusercontent.com/render/math?math=W_1,\W_2,\cdot\cdot\cdot,\W_n.)   
Define:  
![formula](https://render.githubusercontent.com/render/math?math=S_k=(k-1)W_k%2B(N-1)\sum_{i=1}^{k-1}W_i)
For an interpolation of quantile ![formula](https://render.githubusercontent.com/render/math?math=p), find ![formula](https://render.githubusercontent.com/render/math?math=k) such that ![formula](https://render.githubusercontent.com/render/math?math=\frac{S_k}{S_n}\le\p\le\frac{S_{k%2B1}}{S_n}). 
Our estimate would then be: ![formula](https://render.githubusercontent.com/render/math?math=X_k%2B(X_{k%2B1}-X_k)\frac{pS_n-S_k}{S_{k%2B1}-S_k})  
[Source](https://stats.stackexchange.com/questions/13169/defining-quantiles-over-a-weighted-sample)

**Examples**
```sh
Percentile p = new Percentile().withEstimationType(Percentile.EstimationType.R_7);
double[] dataset = { 1, 2, 3, 4, 5 };
double[] weights = { 1, 1, 1, 1, 1 };
>>>3.0

Percentile p = new Percentile().withEstimationType(Percentile.EstimationType.R_7);
double[] dataset = { 1, 2, 3, 4, Double.NaN};
double[] weights = { 1, 1, 1, 1, 1};
System.out.println(p.evaluate(dataset, weights, 50));
>>>2.5
/**Note: Because the default NaNStrategy is REMOVED,
         the size of dataset is four after removing nan.
*/

Percentile p = new Percentile().withEstimationType(Percentile.EstimationType.R_7);
double[] dataset = { 1, 2, 3, 4, 5};
double[] weights = { 1, 2, 3, 4, 5};
System.out.println(p.evaluate(dataset, weights, 50));
>>> 3.6666666666666665
```
