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

### 3.4 Normalisation.

To remove the dimention influence. We need to normalise the data.

![normalisation](https://pic1.zhimg.com/80/v2-be30691d37ac93b2237217cadca2e967_hd.jpg)

### 3.5 Construct a kd-tree and make nearest neighbor search using kd-tree

Every leaf note contains: 1)characteristic coordinate. 2)Segregate Shaft. 3)Indicator direct to the left. 4)Indicator direct to the right.
For detailed describtion see LiHang's book "Statistic Learning Method" or check the references.

### 3.6 Code (Note that class is also an object，so it can also be assigned to another variable. Also there is something called metaclass(元类))

#### 1) Without using sklearn
```markdown
class kdtree(object):
    # create a kdtree class
    # point_list is a pair of lists. pair[0] is a tuple which contains features. pair[1] is types.
    def __init__(self, point_list, depth=0, root=None):
        # check if there is no point left
        if len(point_list)>0:
            
            # choose axis according to the depth of the tree
            k = len(point_list[0][0])
            axis = depth%k
    
            # seperate data by median plane
            point_list.sort(key=lambda x:x[0][axis])
            median = len(point_list)//2
            
            self.axis = axis
            self.root = root #root indicates whether the point is the root point
            sef.size = len(point_list)
            
            # create node
            self.node = point_list[median]
            
            # create left branch and right branch using recursive method 
            # branch has its son branch, node, axis, root(bool) and size
            if len(point_list[:median])>0:
                self.left = kdtree(point_list[:median],depth+1)
            else:
                self.left = None
            if len(point_list[median+1:])>0:
                self.right = kdtree(point_list[median+1:],depth+1)
            else:
                self.right = None
        else:
            return None
    
    # Add point to the tree
    def insert(self,point):
        self.size+=1
        # judge whether it should be added to the left branch or the right branch
        if point[0][self.axis]<self.node[0][self.axis]:
            if self.left!=None:
                self.left.insert(point)
            else:
                self.left = kdtree([point],self.axis+1)
        else:
            if self.right!=None:
                self.right.insert(point)
            else:
                self.right = kdtree([point],self.axis+1)
    
    # input a number and find the leaf
    def find_leaf(self, point):
        if self.left==None and self.right==None:
            return self
        elif self.left==None:
            return self.right.find_leaf(point)
        elif self.right==None:
            return self.left.find_leaf(point)
        elif point[self.axis]<self.node[0][self.axis]:
            return self.left.find_leaf(point)
        else:
            return self.right.find_leaf(point)
     
    # Find the nearest k points. Time complexity is O(DlogN), D is dimension and k is the size of the tree.
    # Input a point, a distance function. The distance function is by default L2.
    def knearest(self, point, k=1, dist=lambda x,y: sum(list(map(lambda u,v:(u-v) ** 2,x,y)))):
        # look down the find the leaf
        leaf = self.find_leaf(point)
        # Climb up from the leaf
        return leaf.k_down_up(point, k, dist, result=[], stop=self, visited=None)

            
    # Climb up. "Stop" indicates where you go. "Visited" tells where you come from.
    def k_down_up(self, point,k, dist, result=[],stop=None, visited=None):

        # choose the longest distance
        if result==[]:
            max_dist = 0
        else:
            max_dist = max([x[1] for x in result])

        other_result=[]

        # 如果离分界线的距离小于现有最大距离，或者数据点不够，就从另一边的树根开始刨
        if (self.left==visited and self.node[0][self.axis]-point[self.axis]<max_dist and self.right!=None)\
            or (len(result)<k and self.left==visited and self.right!=None):
            other_result=self.right.knearest(point,k, dist)

        if (self.right==visited and point[self.axis]-self.node[0][self.axis]<max_dist and self.left!=None)\
            or (len(result)<k and self.right==visited and self.left!=None):
            other_result=self.left.knearest(point, k, dist)

        # 刨出来的点放一起，选前 k 个
        result.append((self.node, dist(point, self.node[0])))
        result = sorted(result+other_result, key=lambda pair: pair[1])[:k]

        # 到停点就返回结果
        if self==stop:
            return result
        # 没有就带着现有结果接着往上爬
        else:
            return self.root.k_down_up(point,k,  dist, result, stop, self)
    
    # 输入 特征、类别、k、距离函数
    # 返回这个点属于该类别的概率
    def kNN_prob(self, point, label, k, dist=lambda x,y: sum(map(lambda u,v:(u-v) ** 2,x,y))):
        nearests = self.knearest(point,  k, dist)
        return float(len([pair for pair in nearests if pair[0][1]==label]))/float(len(nearests))
    # 输入 特征、k、距离函数
    # 返回该点概率最大的类别以及相对应的概率
    def kNN(self, point, k, dist=lambda x,y: sum(map(lambda u,v:(u-v) ** 2,x,y))):
        nearests = self.knearest(point, k , dist)

        statistics = {}
        for data in nearests:
            label = data[0][1]
            if label not in statistics: 
                statistics[label] = 1
            else:
                statistics[label] += 1

        max_label = max(statistics.iteritems(), key=operator.itemgetter(1))[0]
        return max_label, float(statistics[max_label])/float(len(nearests))
```

#### little test

```markdown
#很有意思，因为类也是一个对象，所以可以自我迭代，可以赋值给一个变量
class test(object):
    def __init__(self,name,n,x=None):
        if n<5:
            print(x.name)
        else:
            print("第一步x是None所以没有name属性")
        if n>0:
            self.name = n
            self.age = test(name,n-1,self) #这里的实例self其实是传给了参数x，而且把实例self的属性也一并传了过去，所以x.name会有值
        else:
            print('fuck it')
    pass
a = test('f',5)

#a.age是一个对象，a.age.age仍然是一个对象，他们都具有name属性，而参数name其实并没有被用到
print(a.name)
print(a.age.name)
print(type(a.age.age.age.age.age))
```
#### 2. Use sklearn

### sklearn knn 参数详解
neighbors.KNeighborsClassifier(n_neighbors=5, weights=’uniform’, algorithm=’auto’, leaf_size=30, p=2, metric=’minkowski’, metric_params=None, n-jobs=1)

n_neighbors: k的个数

weights：临近实例的权重，可以取三个值，‘uniform’为权重相同的情况，‘distance’是按照距离的倒数进行加权，也可以自己定义函数来对权值进行设定

algorithm：是分类时采取的算法，有 'brute'、'kd_tree' 和 'ball_tree'。kd_tree 的算法在 kd 树文章中有详细介绍，而 ball_tree 是另一种基于树状结构的 kNN 算法，brute 则是最直接的蛮力计算。根据样本量的大小和特征的维度数量，不同的算法有各自的优势。默认的 'auto' 选项会在学习时自动选择最合适的算法，所以一般来讲选择 auto 就可以。

leaf_size：是 kd_tree 或 ball_tree 生成的树的树叶（树叶就是二叉树中没有分枝的节点）的大小。在 kd 树文章中我们所有的二叉树的叶子中都只有一个数据点，但实际上树叶中可以有多于一个的数据点，算法在达到叶子时在其中执行蛮力计算即可。对于很多使用场景来说，叶子的大小并不是很重要，我们设 leaf_size=1 就好。

metric 和 p，是我们在 kNN 入门文章中介绍过的距离函数的选项，如果 metric ='minkowski' 并且 p=p 的话，计算两点之间的距离就是

d((x1,…,xn),(y1,…,yn))=(∑i=1n|xi−yi|p)1/p

一般来讲，默认的 metric='minkowski'（默认）和 p=2（默认）就可以满足大部分需求。其他的 metric 选项可见说明文档。metric_params 是一些特殊 metric 选项需要的特定参数，默认是 None。

n_jobs 是并行计算的线程数量，默认是 1，输入 -1 则设为 CPU 的内核数。
```markdown
import random
from sklearn import neighbors
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap

#生成随机数
x1 = np.random.normal(50, 6, 200)
y1 = np.random.normal(5, 0.5, 200)

x2 = np.random.normal(30,6,200)
y2 = np.random.normal(4,0.5,200)

x3 = np.random.normal(45,6,200)
y3 = np.random.normal(2.5, 0.5, 200)

'''
x1、x2、x3 作为 x 坐标，y1、y2、y3 作为 y 坐标，两两配对。
(x1,y1) 标为 1 类，(x2, y2) 标为 2 类，(x3, y3)是 3 类。
可它们画出，1 类是蓝色，2 类红色，3 类绿色。
'''
plt.scatter(x1,y1,c='b',marker='s',s=50,alpha=0.8)
plt.scatter(x2,y2,c='r', marker='^', s=50, alpha=0.8)
plt.scatter(x3,y3, c='g', s=50, alpha=0.8)
plt.show()

x_val = np.concatenate((x1,x2,x3))
y_val = np.concatenate((y1,y2,y3))

#归一化
x_diff = max(x_val)-min(x_val)
y_diff = max(y_val)-min(y_val)

x_normalized = [x/(x_diff) for x in x_val]
y_normalized = [y/(y_diff) for y in y_val]
xy_normalized = list(zip(x_normalized,y_normalized))

labels = [1]*200+[2]*200+[3]*200

clf = neighbors.KNeighborsClassifier(30)
#训练完成
clf.fit(xy_normalized, labels)

#首先，我们想知道 (50,5) 和 (30,3) 两个点附近最近的 5 个样本分别都是什么。坐标别忘了除以 x_diff 和 y_diff 来归一化。
nearests = clf.kneighbors([(50/x_diff, 5/y_diff),(30/x_diff, 3/y_diff)], 5, False)
print(nearests)

#类别预测
prediction = clf.predict([(50/x_diff, 5/y_diff),(30/x_diff, 3/y_diff)])
print(prediction)

#概率预测
prediction_proba = clf.predict_proba([(50/x_diff, 5/y_diff),(30/x_diff, 3/y_diff)])
print(prediction_proba)

#准确率打分，生成测试数据集
x1_test = np.random.normal(50, 6, 100)
y1_test = np.random.normal(5, 0.5, 100)

x2_test = np.random.normal(30,6,100)
y2_test = np.random.normal(4,0.5,100)

x3_test = np.random.normal(45,6,100)
y3_test = np.random.normal(2.5, 0.5, 100)

xy_test_normalized = list(zip(np.concatenate((x1_test,x2_test,x3_test))/x_diff,\
                        np.concatenate((y1_test,y2_test,y3_test))/y_diff))

labels_test = [1]*100+[2]*100+[3]*100

score = clf.score(xy_test_normalized, labels_test)
print(score)
```

#### References
[一文搞懂k近邻（k-NN）算法](https://zhuanlan.zhihu.com/p/26029567)

[一只兔子帮你理解 kNN](https://www.joinquant.com/post/2227?f=zh&%3Bm=23028465)

[KNN算法详解](https://www.joinquant.com/post/2843)

[用sklearn实现knn](https://www.joinquant.com/post/3227?f=study&m=math)

[从K近邻算法、距离度量谈到KD树、SIFT+BBF算法](https://blog.csdn.net/v_july_v/article/details/8203674)

[利用sklearn学习统计学习方法](https://zhuanlan.zhihu.com/p/27161407)
