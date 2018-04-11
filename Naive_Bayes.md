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
    classVec=[0,1,0,1,0,1] #1表示侮辱性言论，0表示正常言论
    return postingList,classVec
```

## 2.根据文档词汇表构建词向量
### 2.1 (1)构建词汇表生成函数creatVocabList：
```python
def createVocabList(dataSet):
    vocabSet=set([])
    for document in dataSet:
        vocabSet=vocabSet|set(document) #取两个集合的并集
    return list(vocabSet)
```
## (2)对输入的词汇表构建词向量
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
