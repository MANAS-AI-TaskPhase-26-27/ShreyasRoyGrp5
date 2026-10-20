what is machine learning
 - supervised learning:
	 - Linear Regression
		 - feature(x) - target(y) relationship
		 - learning algo: feature -> model function -> prediction 
		 - error/ residuals: (prediction ~ actual y)/actual y 
		 - for the case of linear regression, where w,b are parameters
			$$
						  f(x) = wx + b
			$$
		 - Cost Function (Mean Square Error)
		 - Gradient Descent (learning rate, overshooting, slow)
			 - Near local minimum, for fixed alpha, update steps become smaller
	 - Multiple Linear Regression 
		 - Vectorization
		 - Normal Equation as an alternative to Gradient Descent
		 - Feature Scaling to improve Gradient Descent
			 - Diving by Max
			 - Mean Minmax Normalization
			 - Z-score Normalization
			 - Why is Z-score Normalization better than Mean Minmax Normalization(generally)
		 - Learning Curves
			 - Automatic Convergence Test (using epsilon)
			 - Convergence Test by graphing J_w  vs number of iterations
			 - Overshooting
		 - Feature Engineering (Polynomial)
		 - **Stochastic, Batch, Mini-Batch Gradient Descent**
		 - Regularization 
	 - Locally Weighted Linear Regression
		 - Cost Function has a new "w(i)" weighting function, where "w(i)" = exp(-((x(i)-x)^2)/2)
	 - Classification
		 - Binary Classification
		 - Decision Boundary
			 - Linear and Non-Linear
		 - Logistic Regression
			 - Logistic Loss Function
 - unsupervised ML
	 - Clustering
	 - Anomaly Detection
	 - Dimensionality reduction: Compresses complex datasets with many variables into fewer dimensions while preserving critical information (e.g., Principal Component Analysis)
SVM -> Used to represent information as a infinite dimension vector, given finite data

Relation between vector fields and spaces and gradient descent. (Cost Function is the scalar field)
Evaluation Metrics of ML:
- RMSE: Root Mean Squared Error
- MAE: Mean Absolute Error
- WMAE (Accuracy %) : Weighted Mean Absolute Error
- R2 Score
use of jupyter notebooks

Error vs training set size graphs indicate bias and variance
squared error cost function always ends up with a bowl shaped curve(convex function), with only 1 local minima. This is it an advantage over other cost functions which may have multiple local minima, since gradient descent for different starting values, can lead to it converging towards different local minima.

Some Curious Things About Linear Regression: https://share.gemini.google/RH3GqFrZnC7p

`python -m notebook`
```python
train = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\train.csv")
test = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\test.csv")

train = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\crime_train.csv")
test = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\crime_test.csv") 

train = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\CarPrices\CarPrice_Assignment.csv")

train = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\fishing_logs.csv")
test = pd.read_csv(r"C:\Users\SHREYAS ROY\Desktop\Manipal Work Documents\prediction_logs.csv")
```
Learning rate = 0.3, iterations = 5000, Mean Minmax Normalization
![[Pasted image 20261006133019.png]]![[Pasted image 20261006133027.png]]
above R2 was ~82% and accuracy ~ 34%(mape)

Learning rate = 0.5, iterations = 20,000, Mean Minmax Normalization
![[Pasted image 20261006153625.png]]
![[Pasted image 20261006153615.png]]
Please Note that in the above output, it should be "Mean Absolute Error" instead of "Weighted Mean Absolute Error"

Learning Rate = 0.01, Iterations = 20,000 Z-Score Normalized:
![[Pasted image 20261006174728.png]]
![[Pasted image 20261006174750.png]]


![[Fundamentals of Machine Learning.pdf]]


why do we need to normalize "target"
