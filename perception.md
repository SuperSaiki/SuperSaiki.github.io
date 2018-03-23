
This code in the first link contains two ways to make this algorithm. One is to use numpy to build this algorithm from bottom up. The other way is to use sklearn directly and to be a "调包侠". The theory and mathmatics are in the book "统计学习方法" by doctor "Hang Li".

Two references including code and theory from "知乎":

[Introduction to perceptron,忆臻](https://zhuanlan.zhihu.com/p/25880406)

[Use sklearn to do perceptron, Norlan](https://zhuanlan.zhihu.com/p/27152953)

# 1. Original form algorithm

```markdown
# input raw data
import numpy as np

train_set = [[[3,3],1],[[4,3],1],[[1,1],-1]]
w = [0,0]           # initialise parameter w
b = 0               # initialise parameter b

def update(item):  # parameters updating function 
    global w,b
    x = np.array(item[0])
    w += x.dot(item[1]*1)
    b += 1*item[1]
    print(w)
    print(b)
    
def judge(item):  # judge whether the classification is correct
    res = np.vdot(w,item[0]) + b # calculate w*x+b
    res = item[1]*res               # calculate y*(w*x+b)
    return res
    
def check(): # check whther all the data is classified correctly, return true or flase
    flag = False
    for item in train_set:
        if judge(item)<=0:
            flag = True
            update(item)
    return flag
    
if __name__ == '__main__': # if this file is executed rather than being invoked, the result of this judgement statement is true
    flag = False
    for i in range(1000): # try at most 1000 times
        if not check():   # if all the data is classified correctly
            flag = True
            break
    if flag:
        print("all the data is classified correctly within 1000 iterations")
    else:
        print("Unluckily, after 1000 iterations the data is still not correctly classified")
```

```markdown
# 2. Original form algorithm using sklearn

from sklearn.datasets import make_classification
from sklearn.linear_model import Perceptron
from matplotlib import pyplot as plt
# create data
x,y = make_classification(n_samples=1000,n_features=2,n_redundant=0,n_informative=1,n_clusters_per_class=1)
#seperate data into training data and test data
x_train = x[:800,:]
x_test = x[800:,:]
y_train = y[:800]
y_test = y[800:]
#seperate positive examples and negative examples
positive_x1 = [x[i,0] for i in range(1000) if y[i]==1]
positive_x2 = [x[i,1] for i in range(1000) if y[i]==1]
negative_x1 = [x[i,0] for i in range(1000) if y[i]==0]
negative_x2 = [x[i,1] for i in range(1000) if y[i]==0]
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

