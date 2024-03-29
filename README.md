
# Breast-cancer-prediction-ML-Python
Make predictions for breast cancer, malignant or benign using the Breast Cancer data set<br>
Dataset - Breast Cancer Wisconsin (Original) Data Set<br>
This code demonstrates logistic regression on the dataset and also uses gradient descent to lower the BCE(binary cross entropy).
## <strong>Dataset description</strong>

![](/pictures/breast%20cancer%20description.PNG)
<ol>
<li>Sample code number: id number</li>
<li>Clump Thickness: 1 - 10</li>
<li>Uniformity of Cell Size: 1 - 10</li>
<li>Uniformity of Cell Shape: 1 - 10</li>
<li>Marginal Adhesion: 1 - 10</li>
<li>Single Epithelial Cell Size: 1 - 10</li>
<li>Bare Nuclei: 1 - 10</li>
<li>Bland Chromatin: 1 - 10</li>
<li>Normal Nucleoli: 1 - 10</li>
<li>Mitoses: 1 - 10</li>
<li>Class: (2 for benign, 4 for malignant)</li>
</ol>
<h2>Libraries required</h2>
<ol>
  <li>numpy
      <br>
        <code>pip install numpy</code>
    </li>
  <li>pandas
    <br>
    <code>
      pip install pandas
    </code>
  </li>
  <li>
    random
    <br>
    <code>
      pip install random
    </code>
  </li>
  <li>
    seaborn
    <br>
    <code>
      pip install seaborn
    </code>
  </li>
</ol>
<h2> Logistic regression algorithm </h2>

![](/pictures/logistic_regression.gif)

<ul>
<li>
  Use the sigmoid activation function - <img src="https://latex.codecogs.com/gif.latex?$\sigma(z)=&space;1/1&plus;e^{-z}$"            title="$\sigma(z)= 1/1+e^{-z}$" />
  </li>
 <li> 
  Remember the gradient descent formula for liner regression where Mean squared error was used but we cannot use Mean squared error here so replace with some error <img src="https://latex.codecogs.com/gif.latex?$E$" title="$E$" />
  </li>
  <li>
    Gradient Descent - <img src="https://latex.codecogs.com/gif.latex?\theta&space;_{j}=\theta&space;_{j}-&space;\alpha\cdot&space;\partial&space;MSE\partial&space;/&space;\theta&space;_{j}&space;$" title="\theta _{j}=\theta _{j}- \alpha\cdot \partial MSE\partial / \theta _{j} $" />
    Logistic regression - <img src="https://latex.codecogs.com/gif.latex?\theta&space;_{j}=\theta&space;_{j}-&space;\alpha\cdot&space;\partial&space;E\partial&space;/&space;\theta&space;_{j}&space;$" title="\theta _{j}=\theta _{j}- \alpha\cdot \partial E\partial / \theta _{j} $" />
  </li>
  <li>
    Conditions for E:
    <ol>
      <li> Convex or as convex as possible</li>
      <li> Should be function of <img src="https://latex.codecogs.com/gif.latex?$\theta$" title="$\theta$" /></li>
      <li> Should be differentiable</li>
      </ol>
  </li>
  <li>
    So use, Entropy = <img src="https://latex.codecogs.com/gif.latex?$-p&space;\log&space;p$" title="$-p \log p$" />
  </li>
  <li>As we cant use both <img src="https://latex.codecogs.com/gif.latex?\hat{y}" title="\hat{y}" /> and y so use cross entropy
  <img src="https://latex.codecogs.com/gif.latex?-y&space;\log&space;\hat{y}" title="-y \log \hat{y}" /> as
    <img src="https://latex.codecogs.com/gif.latex?\hat{y}\epsilon&space;[0,1]" title="\hat{y}\epsilon [0,1]" />
  </li>
  <li>
    So add 2 cross entropies CE 1 = <img src="https://latex.codecogs.com/gif.latex?-y&space;\log&space;\hat{y}" title="-y \log \hat{y}" /> and CE 2 = <img src="https://latex.codecogs.com/gif.latex?(1-y)&space;\log&space;(1-&space;\hat{y})" title="(1-y) \log (1- \hat{y})" />.
    We get Binary Cross entropy (BCE) = <img src="https://latex.codecogs.com/gif.latex?-y\log&space;\hat{y}-(1-y)&space;\log&space;(1-&space;\hat{y})" title="-y\log \hat{y}-(1-y) \log (1- \hat{y})" />
    <li>
      So now our formula becomes,
      <img src="https://latex.codecogs.com/gif.latex?\theta&space;_{j}=\theta&space;_{j}-&space;\alpha\cdot&space;\partial&space;BCE/&space;\partial&space;\theta&space;_{j}&space;$" title="\theta _{j}=\theta _{j}- \alpha\cdot \partial BCE/ \partial \theta _{j} $" />
      <li>
        Using simple chain rule we obtain,
        <img src="https://latex.codecogs.com/gif.latex?\theta&space;_{j}=\theta&space;_{j}-&space;\frac{\alpha}{m}\cdot&space;(\hat&space;y&space;-&space;y)^{T}\cdot&space;X" title="\theta _{j}=\theta _{j}- \frac{\alpha}{m}\cdot (\hat y - y)^{T}\cdot X" />
        </li>
      </li>
    </li>
    <li>
  Now apply Gradient Descent with this formula
  </li>
</ul>

## Code
<ol>
<li>Data preprocessing<br>Load data, remove empty values. As we are using logistic regression replace 2 and 4 with 0 and 1.
<li> <code>sns.pairplot(df)</code><br>Create pair wisegraphs for the features.
<li>Do Principal component analysis for simplified learning. 
<li><code>full_data=np.matrix(full_data)<br>x0=np.ones((full_data.shape[0],1))
data=np.concatenate((x0,full_data),axis=1)<br>
print(data.shape)<br>
theta=np.zeros((1,data.shape[1]-1))<br>
print(theta.shape)<br>
print(theta)
</code><br>
Convert data to matrix, concatenate a unit matrix with the complete data matrix. Also make a zero matrix, for the initial theta.
<li>
<code>test_size=0.2<br>
X_train=data[:-int(test_size*len(full_data)),:-1]<br>
Y_train=data[:-int(test_size*len(full_data)),-1]<br>
X_test=data[-int(test_size*len(full_data)):,:-1]<br>
Y_test=data[-int(test_size*len(full_data)):,-1]
</code><br>
Create the train-test split
<li>
<code>
def sigmoid(Z):<br>
    &nbsp return 1/(1+np.exp(-Z))<br><br>
def BCE(X,y,theta):<br>
    &nbsp pred=sigmoid(np.dot(X,theta.T))<br>
    &nbsp mcost=-np.array(y)*np.array(np.log(pred))np.array((1y))*np.array(np.log(1pred))<br>
    &nbsp return mcost.mean()
</code><br>
Define the code for sigmoid function as mentioned and the BCE.
<li>
<code>
def grad_descent(X,y,theta,alpha):<br>
   &nbsp h=sigmoid(X.dot(theta.T))<br>
    &nbsp loss=h-y<br>
    &nbsp dj=(loss.T).dot(X)<br>
    &nbsp theta -= (alpha/(len(X))*dj)<br>
    &nbsp return theta <br>      
cost=BCE(X_train,Y_train,theta)<br>
print("cost before: ",cost)    <br>
theta=grad_descent(X_train,Y_train,theta,alpha)   <br>
cost=BCE(X_train,Y_train,theta)<br>
print("cost after: ",cost)
</code><br>
Define gradient descent algorithm and also define the number of epochs. Also test the gradient descent by 1 iteration.
<li>
<code>
def logistic_reg(epoch,X,y,theta,alpha):<br>
   &nbsp for ep in range(epoch):<br>
#update theta <br>
        &nbsp theta=grad_descent(X,y,theta,alpha)<br>
#calculate new loss<br>
        &nbsp if ((ep+1)%1000 == 0):<br>
           &nbsp &nbsp  loss=BCE(X,y,theta)<br>
             &nbsp &nbsp print("Cost function ",loss)<br>
     &nbsp return theta<br><br>
theta=logistic_reg(epoch,X_train,Y_train,theta,alpha)
</code><br>
Define the logistic regression with gradient descent code.
<li>
<code>
print(BCE(X_train,Y_train,theta))<br><br>
print(BCE(X_test,Y_test,theta))
</code><br>
Finally test the code,
</ol>
<br>
Now we are done with the code &#128512;

## Other algorithms for same project by me
1. Random Forest classifier<br>
## About me
<strong>David Nguyen</strong><br>
[LinkedIn](https://www.linkedin.com/in/nguyenhuynhduc/)

