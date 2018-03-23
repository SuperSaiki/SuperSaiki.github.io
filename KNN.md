# KNN

#### 2.1 Key thoughts

Giving a training data set, for any new point, find its nearest k points. And the new point belongs to the type that most points belong to among the k points.

![knn](https://pic2.zhimg.com/v2-c3f1d2553e7467d7da5f9cd538d2b49a_1200x500.jpg)

#### 2.2 Different types of distance measurements

![distance measurement](https://pic4.zhimg.com/80/v2-60bb382b0d22ec0ce296ed0e024f31bc_hd.jpg)

#### 2.3 The choose of parameter "k"

If k is too small, the model will be more complex and more likely to over fitting. We are easy to learning noises.

![choose of k](https://pic4.zhimg.com/80/v2-18df63acb37f29bd026e01770ef5c966_hd.jpg)

If k is large, the model will be easy. The estimation error will decrease, but the approximately error will increase. Because the point which is far way from the new point can influence which type it belongs to.

In application, we always choose a small "k" first, then use cross validation to find the best "k".

### 3.4 normalisation.
To remove the dimention influence. We need to normalise the data.

![normalisation](https://pic1.zhimg.com/80/v2-be30691d37ac93b2237217cadca2e967_hd.jpg)




