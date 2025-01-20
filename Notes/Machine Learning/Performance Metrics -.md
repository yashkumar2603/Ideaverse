### R-Squared -
$$R_{squared} = 1 - \frac{\sum{(y_i - \hat{y_i}})^2}{\sum{(y_i - \bar{y_i})^2}}$$
The closer this value is to 1, the better the performance of the model is, this is very much used in linear regression.
The numerator is basically very close to the cost function, and the denominator is more like a normalization factor.

### Modified R-squared -

If we just keep on adding new features, then $R^2$ value increases, even if the new feature is not correlated to the output.
Formula - $$Adj. R^2 = 1 - \frac{(1-R^2)(N-1)}{N-P-1}$$
$N = No. of Data points$
$P = No. of Features$ 
$R = R^2$ value calculated from previous formula


### Cost Functions -
When choosing between Mean Squared Error (MSE), Mean Absolute Error (MAE), and Root Mean Squared Error (RMSE) as the cost function for linear regression, the decision depends on the specific characteristics of your problem and the behavior you want to penalize more. Here's a breakdown of when each should be used:

### 1. **Mean Squared Error (MSE)**
   - **Formula**: $$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2 $$
   - **Use case**: 
     - When you want to penalize **larger errors more** than smaller ones because the error is squared. This makes it highly sensitive to outliers.
     - MSE is more useful if your problem allows for some small errors but you want to avoid large deviations, as larger errors will disproportionately affect the cost.
   - **Sensitivity**: High sensitivity to outliers due to squaring the error.
   - **Application**: Often used when minimizing variance or in problems where larger errors are significantly more costly, such as financial forecasting or scientific predictions where large deviations need to be avoided.
- Ordinary Least Squares is used to fit the data and MSE is used to find the accuracy of the model. There is no as such difference in the formula, the only thing is that OLS is used continuously with the then-found out parameters. While, MSE is found using the finally found out parameters for the linear regression line.

### 2. **Mean Absolute Error (MAE)**
   - **Formula**: $$MAE = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y_i}|$$
   - **Use case**:
     - When you want to treat all errors **equally** regardless of their size. MAE is more robust to outliers than MSE since the errors are not squared.
     - It’s ideal if your problem involves scenarios where you care more about the **median** error than the average, or if you have many outliers that you don’t want to disproportionately influence your model.
   - **Sensitivity**: Less sensitive to outliers compared to MSE.
   - **Application**: Useful in real-world applications where the presence of outliers is common, such as house price predictions, and you want to treat all errors linearly.

### 3. **Root Mean Squared Error (RMSE)**
   - **Formula**: $$RMSE = \sqrt{ \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2 }$$
   - **Use case**:
     - Similar to MSE, RMSE penalizes larger errors more than smaller ones, but the square root makes RMSE comparable to the original units of the data (as opposed to MSE, which is in squared units).
     - RMSE is useful when you want to **penalize large errors**, but you also want to express the result in the same units as the target variable.
   - **Sensitivity**: High sensitivity to outliers, like MSE.
   - **Application**: Commonly used in applications where large errors are particularly undesirable and where interpretability in the same units as the target variable is important (e.g., model evaluation in competition datasets).

### Key Takeaways:
- **Use MSE** when you want to emphasize large errors and are not concerned about outliers.
- **Use MAE** when you want to treat all errors equally and want a more robust metric to outliers.
- **Use RMSE** when you want to penalize large errors but still have a metric that’s interpretable in the same units as the target variable.

### General Rule of Thumb:
- If **outliers are rare** and large errors should be avoided, use **MSE or RMSE**.
- If **outliers are frequent** and you want to minimize their influence, use **MAE**.

