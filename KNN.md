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

### 3.6 Code (Note that class is also an object，so it can also be assigned to another variable. Also there is something called meta class(元类))

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

