## Welcome to SuperSaiki's Learning record in machine learning.

Hope we can make progress together. 

### Official use guide for github page:

[userguide](https://github.com/SuperSaiki/SuperSaiki.github.io/blob/master/Official%20direction%20for%20use)


# Machine Learning


### 1.【2018-03-17】Perceptron Study

[Perceptron](https://github.com/SuperSaiki/Saiki/blob/master/perceptron.py)

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

This code contains two ways to make this algorithm. One is to use numpy to build this algorithm from bottom up. The other way is to use sklearn directly and to be a "调包侠". The theory and mathmatics are in the book "统计学习方法" by doctor "Hang Li".

Two references including code and theory from "知乎":

[Introduction to perceptron,忆臻](https://zhuanlan.zhihu.com/p/25880406)

[Use sklearn to do perceptron, Norlan](https://zhuanlan.zhihu.com/p/27152953)
