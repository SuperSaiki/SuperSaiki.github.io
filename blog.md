## Welcome to SuperSaiki's Learning record in machine learning.

Hope we can make progress together. 

### Official use guide for github page:

[userguide](https://github.com/SuperSaiki/SuperSaiki.github.io/blob/master/Official%20direction%20for%20use)


# Machine Learning


### 1.【2018-03-17】Perceptron

[Perceptron code](https://github.com/SuperSaiki/Saiki/blob/master/perceptron.py)

```markdown
#define a  perceptron
clf = Perceptron(fit_intercept = False,n_iter=30,shuffle=False)
clf.fit(x_train,y_train)
print(clf.intercept_)
#validate using test data
acc = clf.score(x_test,y_test)
print(acc)
#show results on pictures
plt.scatter(positive_x1,positive_x2,c='red')
plt.scatter(negative_x1,negative_x2,c='blue')
line_x = np.arange(-4,4)
line_y = line_x*(-clf.coef_[0][0]/clf.coef_[0][1])-clf.intercept_/clf.coef_[0][1]
plt.plot(line_x,line_y)
plt.show()
```

This code in the first link contains two ways to make this algorithm. One is to use numpy to build this algorithm from bottom up. The other way is to use sklearn directly and to be a "调包侠". The theory and mathmatics are in the book "统计学习方法" by doctor "Hang Li".

Two references including code and theory from "知乎":

[Introduction to perceptron,忆臻](https://zhuanlan.zhihu.com/p/25880406)

[Use sklearn to do perceptron, Norlan](https://zhuanlan.zhihu.com/p/27152953)

#### 3. 【2018-03-21】 KNN (k-nearest neighbor)

#### 3.1 Key thoughts

Giving a training data set, for any new point, find its nearest k points. And the new point belongs to the type that most points belong to among the k points.

![knn](https://pic2.zhimg.com/v2-c3f1d2553e7467d7da5f9cd538d2b49a_1200x500.jpg)

#### 3.2 Different types of distance measurements

![distance measurement] (https://pic4.zhimg.com/80/v2-60bb382b0d22ec0ce296ed0e024f31bc_hd.jpg)

#### 3.3 The choose of parameter "k"

If k is too small, the model will be more complex and more likely to over fitting. We are easy to learning noises.

![choose of k](https://pic4.zhimg.com/80/v2-18df63acb37f29bd026e01770ef5c966_hd.jpg)

If k is large, the model will be easy. The estimation error will decrease, but the approximately error will increase. Because the point which is far way from the new point can influence which type it belongs to.

In application, we always choose a small "k" first, then use cross validation to find the best "k".

### 3.4 normalisation.
To remove the dimention influence. We need to normalise the data.

![normalisation](https://pic1.zhimg.com/80/v2-be30691d37ac93b2237217cadca2e967_hd.jpg)





