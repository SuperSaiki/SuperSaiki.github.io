## 1.构造一些实验样本，包括已经切分词条的文档集合，并且已经分类（带有侮辱性言论，和正常言论）。为了获取方便，在bayes.py中构造一个loadDataSet函数来生成
实验样本。
```python
def loadDataSet():
    postingList=[['my', 'dog', 'has', 'flea', 'problems', 'help', 'please'],
                 ['maybe', 'not', 'take', 'him', 'to', 'dog', 'park', 'stupid'],
                 ['my', 'dalmation', 'is', 'so', 'cute', 'I', 'love', 'him'],
                 ['stop', 'posting', 'stupid', 'worthless', 'garbage'],
                 ['mr', 'licks', 'ate', 'my', 'steak', 'how', 'to', 'stop', 'him'],
                 ['quit', 'buying', 'worthless', 'dog', 'food', 'stupid']]
    classVec=[0,1,0,1,0,1] #1表示侮辱性言论，0表示正常言论,侮辱性言论就是垃圾邮件
    return postingList,classVec
```

## 2.根据文档词汇表构建词向量
### 2.1 准备数据
#### (1)构建词汇表生成函数creatVocabList：
```python
def createVocabList(dataSet):
    vocabSet=set([])
    for document in dataSet:
        vocabSet=vocabSet|set(document) #取两个集合的并集
    return list(vocabSet)
```
#### (2)对输入的词汇表构建词向量
```python
#词集模型
def setOfWords2Vec(vocabList,inputSet):
    returnVec=zeros(len(vocabList)) #生成零向量的array
    for word in inputSet:
        if word in vocabList:
            returnVec[vocabList.index(word)]=1 #有单词，则该位置填充0
        else: print('the word:%s is not in my Vocabulary!'% word)
    return returnVec #返回全为0和1的向量
```
我们将每个词的出现与否作为一个特征，这可以被描述为词集模型(set-of-words model)。
如果一个词在文档中出现不止一次，这可能意味着包含该词是否出现在文档中所不能表达的某种信息,
这种方法被称为词袋模型(bag-of-words model)。
在词袋中，每个单词可以出现多次，而在词集中，每个词只能出现一次。
为适应词袋模型，需要对函数setOfWords2Vec稍加修改，修改后的函数称为bagOfWords2VecMN
 ```python
def bagOfWords2VecMN(vocabList, inputSet):
     returnVec = [0]*len(vocabList)
     for word in inputSet:
             returnVec[vocabList.index(word)] += 1
    return returnVec
 ```
 ### 2.2 训练算法：从词向量计算概率
 ```python
 def trainNB0(trainMatrix,trainCategory):
      '''
      朴素贝叶斯分类器训练函数(此处仅处理两类分类问题)
      trainMatrix:文档矩阵
      trainCategory:每篇文档类别标签
      '''
      numTrainDocs = len(trainMatrix)
      numWords = len(trainMatrix[0])
      pAbusive = sum(trainCategory)/float(numTrainDocs)
      #初始化所有词出现数为1，并将分母初始化为2，避免某一个概率值为0，初始化所有词出现次数为1是拉普帕斯平滑，因为纳姆达为1，分母取值为2是因为纳姆达为1，而每个特征的取值只有两个，即1和0，该词出现和没有出现，故S(j)=2，p0num的每一个分量代表一个特征，但如果是词袋模型，每个特征的取值可能性就是就是所有文章的总词数，（因为可能所有文章全是这个词）垃圾邮件中出现某词的概率实质上是多个指示函数相加，即该词出现1次，2次，3次...的概率相加。
      p0Num = ones(numWords); p1Num = ones(numWords)#
      p0Denom = 2.0; p1Denom = 2.0 #
      for i in range(numTrainDocs):
          #是1的话是侮辱性言论，就是垃圾邮件
          #因为每个特征只有两个取值，所以只需要计算在Y确定的条件下特征取1的概率就可以了，特征取0的概率就是1减去前面那个概率
          if trainCategory[i] == 1:
              p1Num += trainMatrix[i]
              p1Denom += sum(trainMatrix[i]) #个人认为这里应该是p1Denon += 1，每出现一个垃圾邮件垃圾邮件的个数就加1，但如果前面是用的词袋模型而不是词集模型的话这样就是对的，词袋模型的话，每个词语最多可能出现m次，(m为一篇文章的总词数)，所以除的时候要用每个词语出现的次数/该词语最多可能出现的次数，及所有文章次数的总和。这时候公式就不是用的指示函数了。
          else:
              p0Num += trainMatrix[i]
              p0Denom += sum(trainMatrix[i])
      #将结果取自然对数，避免下溢出，即太多很小的数相乘造成的影响
      p1Vect = log(p1Num/p1Denom)#change to log()
      p0Vect = log(p0Num/p0Denom)#change to log()
      return p0Vect,p1Vect,pAbusive
```
