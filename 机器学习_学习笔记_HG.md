# 机器学习

## 一. 基础导入

### 1. 基础概念

**机器学习**: 很多算法的集合, 统称为机器学习;

**神经网络**: 机器学习中的一种算法, 模拟人脑, 分输入层/隐藏层/输出层;

**深度学习**: 本质上就是神经网络, 其区别点在于网络层数变多, 提取的特征越深层;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210312150550020.png" alt="image-20210312150550020" style="zoom:50%;" />

### 2. 知识框架

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210312151124239.png" alt="image-20210312151124239" style="zoom: 50%;" />

### 3. 机器学习基本概念

机器学习是数据挖掘的主要工具;

#### 数据集

- 训练集(Training data): 用来训练构建模型;
- 验证集(Validation data): 用来在模型训练阶段测试模型的好坏;
- 测试集(Test data): 模型训练好后,用来评估模型的好坏;

#### 学习方式

- 监督学习(带有标签,分类)
- 无监督学习(无标签, 聚类)
- 半监督学习(少量带有标签,大量无标签)

#### 应用类别

- 回归: 预测
- 分类: 图像识别,垃圾邮件分类等
- 聚类

## 二. 回归(Regression)

特征值(feature)

标签/结果(target)

### 1. 一元线性回归

- 被预测的值叫因变量, 用来预测的值叫自变量, 一元线性回归包含一个自变量和一个因变量;
- 方程如下, 其中𝜃0为回归线的截距,𝜃1为回归线的斜率;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210315155310211.png" alt="image-20210315155310211" style="zoom: 50%;" />

#### 1) 代价函数(Cost Function)

最小二乘法, 线性回归的代价函数为凸函数

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316090503129.png" alt="image-20210316090503129" style="zoom: 50%;" />

**相关系数**: 衡量线性相关性的强弱, 相关系数越接近1, 则样本点越线性: 

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316092755206.png" alt="image-20210316092755206" style="zoom: 50%;" />

**决定系数: **用于描述非线性或由两个及以上变量的相关关系, 可以用来评价模型的效果

#### 2) 梯度下降法

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316094723030.png" alt="image-20210316094723030" style="zoom: 50%;" />

- 学习率为0-1之间, 不能太大也不能太小, 需要不断调试(相当于每次下降的幅度);
- 两个参数需要同步更新;
- 初始参数选择会影响最终结果, 可能陷入局部最小值; (针对线性回归, 代价函数为凸函数, 没有局部最小值, 利用梯度下降法必得全局最小值) 

**优点:** 当特征值非常多的时候也可以很好的工作

**缺点: **需要选择合适的学习率, 需要迭代很多个周期, 只能得到最优解的近似值

#### 3)  标准方程法

sklearn封装的就是标准方程法;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316142619572.png" alt="image-20210316142619572" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316142635365.png" alt="image-20210316142635365" style="zoom:50%;" />

**优点: **不需要学习率, 不需要迭代, 可得到全局最优解

**缺点:** 需要计算(𝑋𝑇𝑋)−1, 时间复杂度大约是O(𝑛3), n是特征数量; 当特征值存在线性关系或特征值数量n大于样本数m时, 矩阵不可逆, 无法计算;

### 2. 多元线性回归

多元线性回归公式如下, 具备多特征:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316104626569.png" alt="image-20210316104626569" style="zoom: 50%;" />

### 3. 多项式回归

假如我们不是要找直线（或者超平面），而是需要找到一个用多项式所表示的曲线（或者超曲面):

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316113139009.png" alt="image-20210316113139009" style="zoom: 50%;" />

### 4. 特征缩放

#### 1) 数据归一化:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316144218531.png" alt="image-20210316144218531" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316144235740.png" alt="image-20210316144235740" style="zoom:50%;" />

#### 2) 均值标准化

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316144409453.png" alt="image-20210316144409453" style="zoom: 50%;" />

### 5. 交叉验证法

把所有样本切10份, 进行多次循环, 每次取不同的一份数据作为测试集, 其余作为训练集, 得到对应的误差, 并取平均作为最后误差输出;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316144842961.png" alt="image-20210316144842961" style="zoom: 33%;" />

### 6. 过拟合与正则化

**过拟合**: 训练集表现好, 测试集表现不好;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316145252819.png" alt="image-20210316145252819" style="zoom: 33%;" />

**防止过拟合(机器学习):**

1. 减少样本特征
2. 增加数据量
3. 正则化(Regularized), 正则后的代价函数如下

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316145945101.png" alt="image-20210316145945101" style="zoom: 50%;" />

### 7. 岭回归

如果数据的特征比样本点还多，则计算(𝑋𝑇𝑋)−1时会出错。因为(𝑋𝑇𝑋)不是满秩矩阵，所以不可逆。对此引入岭回归概念

岭回归公式如下, 其中𝜆为岭系数，𝐼为单位矩阵

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210316150402414.png" alt="image-20210316150402414" style="zoom:50%;" />

岭回归最早是用来处理特征数多于样本的情况，现在也用于在估计中加入偏差，从而得到更好的估计。同时也可以解决**多重共线性**的问题。**岭回归是一种有偏估计**, 其代价函数用的是L2正则化;

**𝜆合理选择, 使:**

1. 各回归系数的岭估计基本稳定;
2. 残差平方和增大不太多;

**Longley数据集:**  因存在严重的多重共线性问题，在早期经常用来检验各种算法或计算机的计算精度;

### 8. LASSO

通过构造一个一阶惩罚函数获得一个精炼的模型；通过最终确定一些指标（变量）的系数为零（岭回归估计系数等于0的机会微乎其微, 造成筛选变量困难），解释力很强;

LASSO也是有偏估计, 其代价函数用的是L1正则化. 岭回归用的是L2正则化.

### 9. 弹性网

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317091248741.png" alt="image-20210317091248741" style="zoom:50%;" />

## 三. 分类

### 1. 逻辑回归

#### 1) 预测函数

预测函数为ℎ<sub>𝜃</sub>(𝑥) = 𝑔(𝜃<sup>𝑇</sup>𝑥) ，其中g(x)函数是sigmoid函数: 

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317091450173.png" alt="image-20210317091450173" style="zoom:50%;" />

#### 2) 代价函数

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317091614310.png" alt="image-20210317091614310" style="zoom:50%;" />

#### 3) 正确率/召回率/F1指标

正确率(Precision)就是检索出来的条目有多少是正确的，召回率(Recall)就是所有正确的条目有多少被检索出来了;

正确率和召回率指标有时会出现矛盾的情况, 常用F-Measure来综合考量:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317092825839.png" alt="image-20210317092825839" style="zoom:50%;" />

当𝛽 =1时，就是常见的F1指标：

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317092848569.png" alt="image-20210317092848569" style="zoom:50%;" />





### 2. 神经网络

**深度学习三次热潮**: 1. 20世纪五六十年代(图灵测试,感知器). 2. 20世纪八九十年代(语音学习). 3. 2006年至今(深度学习).

**深度学习爆发三因素**: 大数据, 计算能力, 算法.

**深度学习三巨头: **Geoffrey Hinton, Yann LeCun, Yoshua Bengio;

#### 1) 单层感知器

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317141632871.png" alt="image-20210317141632871" style="zoom:50%;" />



偏置因子可以统一:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317141802061.png" alt="image-20210317141802061" style="zoom: 50%;" />

##### **学习规则:**

𝜂取值一般取0-1之间; 学习率太大容易造成权值调整不稳定; 学习率太小，权值调整太慢，迭代次数太多

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317142100065.png" alt="image-20210317142100065" style="zoom:50%;" />

##### **模型收敛条件**

1. 误差小于某个预先设定的较小的值
2. 两次迭代之间的权值变化已经很小
3. 设定最大迭代次数，当迭代超过最大次数就停止(常用)

##### 存在问题:

单层感知器无法解决非线性问题(如异或问题);

#### 2) 线性神经网络

与感知器类似, 只是把激活函数改成了purelin函数：y = x

#####  Delta学习规则

δ学习规则是一种利用梯度下降法的一般性的学习规则;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317151615307.png" alt="image-20210317151615307" style="zoom:50%;" />

##### 解决异或问题思路:

1. 添加神经元
2. 添加非线性输入

#### 3) BP神经网络(重要)

BP神经网络是整个人工神经网络体系中的精华，广泛应用于分类识别，逼近，回归，压缩等领域。在实际应用中，大约80%的神经网络模型都采取了BP网络或BP网络的变化形式。

##### 网络结构

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317154851436.png" alt="image-20210317154851436" style="zoom:50%;" />

##### BP算法

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317155552748.png" alt="image-20210317155552748" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317155151132.png" alt="image-20210317155151132" style="zoom:50%;" />

求出输出层的学习信号, 再依次推导上一层的学习信号;

数据复杂 , 需要多层神经网络以及多个神经元, 简单数据则不需要, 不然会导致过拟合. 过拟合可通过正则化来消除;

#### 4) 常见激活函数

##### 1-Sigmoid函数

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317155738690.png" alt="image-20210317155738690" style="zoom:50%;" />

##### 2-Tanh函数和Softsign函数

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317155816639.png" alt="image-20210317155816639" style="zoom:50%;" />

##### 3-ReLU函数

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210317155900616.png" alt="image-20210317155900616" style="zoom:50%;" />

ReLU函数为BP神经网络常用激活函数,u大于0时导数为1, 学习信号基本不会衰减;



### 3. KNN(最近邻规则分类)

#### 1) 算法流程

选择最近K个(一般为奇数)已知实例, 计算未知实例与所有已知实例的距离, 根据少数服从多数的投票法则(majority-voting)，让未知实例归类为K个最邻近样本中最多数的类别;

距离一般使用欧式距离, 其他距离衡量: 余弦值距离, 相关度,曼哈顿距离;

#### 2) 算法缺点

- 算法复杂度较高, 需要比较所有已知实例与要分类的实例;
- 当其样本分布不平衡时，比如其中一类样本过大（实例数量过多）占主导的时候，新的未知实例容易被归类为这个主导样本，因为这类样本实例的数量过大，但这个新的未知实例实际并没有接近目标样本;



### 4. 决策树

比较适合分析离散数据, 如果是连续数据要先转成离散数据再做分析。

常见算法: ID3/C4.5/CART(常用)

**熵(entropy)**: 熵越大, 不确定性越大, 信息熵公式为:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318102422058.png" alt="image-20210318102422058" style="zoom:50%;" />

#### 1) **ID3算法**

选择根节点会选择最大化信息增益来对结点进行划分:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318103312507.png" alt="image-20210318103312507" style="zoom:50%;" />

更倾向于选择分支比较多的节点

#### 2) C4.5算法

信息增益的改进: 增益率

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318104619272.png" alt="image-20210318104619272" style="zoom:50%;" />

#### 3) CART算法

CART决策树的生成就是递归地构建二叉决策树的过程, 用基尼(Gini)系数最小化准则来进行特征选择，生成二叉树

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318112454121.png" alt="image-20210318112454121" style="zoom:50%;" />

#### 4) 剪枝

预剪枝: 构建决策树**前**去掉一些属性;

后剪枝: 构建决策树**后**去掉一些属性;

#### 5) 优缺点

**优点：**小规模数据集有效;

**缺点：**处理连续变量不好; 类别较多时，错误增加的比较快; 不能处理大量数据; 不设置参数容易过拟合



### 5. 集成学习

集成学习就是组合多个分类器，最后可以得到一个更好的分类器。

#### 1) 集成学习算法：

1. 个体学习器之间不存在强依赖关系，装袋（bagging）
2. 随机森林（Random Forest）
3. 个体学习器之间存在强依赖关系，提升（boosting）
4. Stacking

#### 2) bagging

bagging是一种有放回抽样

#### 3) 随机森林(RF)

RF = 决策树+Bagging+随机属性选择

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318165315958.png" alt="image-20210318165315958" style="zoom:50%;" />

**算法流程:** 

1. 样本的随机：从样本集中用bagging的方式，随机选择n个样本。
2. 特征的随机：从所有属性d中随机选择k个属性(k<d)，然后从k个属性中选择最佳分割属性作为节点建立CART决策树。
3. 重复以上两个步骤m次，建立m棵CART决策树。
4. 这m棵CART决策树形成随机森林，通过投票表决结果，决定数据属于哪一类。

#### 4) boosting

AdaBoost是英文“Adaptive Boosting”（自适应增强）的缩写，它的自适应在于：前一个基本分类器被错误分类的样本的权值会增大，而正确分类的样本的权值会减小，并再次用来训练下一个基本分类器; 同时，在每一轮迭代中，加入一个新的弱分类器，直到达到某个预定的足够小的错误率或达到预先指定的最大迭代次数才确定最终的强分类器。

**算法流程:**

1. 首先，是初始化训练数据的权值分布D1。假设有N个训练样本数据，则每一个训练样本最开始时，都被赋予相同的权值：w1=1/N。
2. 然后，训练弱分类器hi。具体训练过程中是：如果某个训练样本点，被弱分类器hi准确地分类，那么在构造下一个训练集中，它对应的权值要减小；相反，如果某个训练样本点被错误分类，那么它的权值就应该增大。权值更新过的样本集被用于训练下一个分类器，整个训练过程如此迭代地进行下去。
3. 最后，将各个训练得到的弱分类器组合成一个强分类器。各个弱分类器的训练过程结束后，加大分类误差率小的弱分类器的权重，使其在最终的分类函数中起着较大的决定作用，而降低分类误差率大的弱分类器的权重，使其在最终的分类函数中起着较小的决定作用。

#### 5) Stacking

使用多个不同的分类器对训练集进预测，把预测得到的结果作为一个次级分类器的输入。次级分类器的输出是整个模型的预测结果

**示例:**

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210318172511832.png" alt="image-20210318172511832" style="zoom:50%;" />



### 6. 贝叶斯分析

应用: 文本相关领域的分类, 如新闻分类/评论分析

#### 贝叶斯定理

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319105819579.png" alt="image-20210319105819579" style="zoom: 50%;" />

举例：
𝑃(𝐻)垃圾邮件的先验概率。
𝑃(𝑋)特定特征的先验概率。
𝑃 (𝑋|𝐻) 在垃圾邮件中，包含特定特征(比如“办证”)邮件的概率。
𝑃 (𝐻|𝑋) 包含特定特征(比如“办证”)的邮件属于垃圾邮件的概率。

#### 朴素贝叶斯

假设: 特征X1，X2，X3……之间都是相互独立的

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319110703731.png" alt="image-20210319110703731" style="zoom: 50%;" />

#### 混合模型

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319111007389.png" alt="image-20210319111007389" style="zoom:50%;" />

#### 词袋模型(Bag of Word)

该模型忽略掉文本的语法和语序等要素，将其仅仅看作是若干个词汇的集合，文档中每个单词的出现都是独立的;

#### TF-IDF

提取词频(Term Frequency，缩写TF), 但是出现最多的词可能是“的，是，在”等对文章分类或搜索没有帮助的停用词(stop words)。

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319142913491.png" alt="image-20210319142913491" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319142931569.png" alt="image-20210319142931569" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319142949272.png" alt="image-20210319142949272" style="zoom:50%;" />

"逆文档频率"（Inverse Document Frequency，缩写为IDF): 对每个词分配一个"重要性"权重。最常见的词（"的"、"是"、"在"）给予最小的权重，较常见的词（"中国"）给予较小的权重，较少见的词（"蜜蜂"、"养殖"）给予较大的权重。这个权重叫做IDF.

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319143009566.png" alt="image-20210319143009566" style="zoom:50%;" />

TF-IDF: 

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319143027861.png" alt="image-20210319143027861" style="zoom:50%;" />

### 7. 支持向量机SVM

被称为深度学习出现前最好的机器学习算法;  但是目前复杂的分类已经被深度学习所取代;

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319161015224.png" alt="image-20210319161015224" style="zoom:50%;" />

#### 算法推导

约束条件:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319163605058.png" alt="image-20210319163605058" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319163657442.png" alt="image-20210319163657442" style="zoom:50%;" />

凸优化问题:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319163817281.png" alt="image-20210319163817281" style="zoom:50%;" />

广义拉格朗日乘子法:

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319163739325.png" alt="image-20210319163739325" style="zoom:50%;" />

进一步简化为对偶问题: 

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319163908431.png" alt="image-20210319163908431" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319164448970.png" alt="image-20210319164448970" style="zoom:50%;" />

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319164520917.png" alt="image-20210319164520917" style="zoom:50%;" />

#### SMO算法

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319164939401.png" alt="image-20210319164939401" style="zoom:50%;" />

#### 松弛变量与惩罚函数

约束条件没有体现错误分类的点要尽量接近分类边界

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319165407460.png" alt="image-20210319165407460" style="zoom:50%;" />

使得分错的点越少越好，距离分类边界越近越好

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319165459241.png" alt="image-20210319165459241" style="zoom:50%;" />







## 四. 聚类(Cluster)

### 1. K-MEANS

#### 1) 基本概念

**算法思想**：以空间中k个点为中心进行聚类，对最靠近他们的对象归类。通过迭代的方法，逐次更新各聚类中心的值，直至得到最好的聚类结果

**算法接受参数K:** 然后将事先输入的n个数据对象划分为k个聚类以便使得所获得的聚类满足：同一聚类中的对象相似度较高；而不同聚类中的对象相似度较小。

#### 2) Mini Batch K-MEANS:

采用小批量的数据子集减少计算时间(每次迭代都抽取子集形成小批量), 结果一般只是略差与标准算法;

#### 3) 算法分析

- 对k个初始质心的选择比较敏感, 容易陷入局部最小值;

  - 解决方法: 使用多次随机初始化, 计算每次建模得到的代价函数的值, 选取代价函数最小的结果作为聚类结果;

- k值的选择由用户指定, 不同的k得到的结果会有挺大的不同;

  - 解决方法: 使用肘部法则确定k值: 计算不同k值时的代价函数的值, 存在肘部则肘部为聚类个数, 如下图为4:

    <img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210315100935154.png" alt="image-20210315100935154" style="zoom: 67%;" />

    

- 存在局限性, 无法处理非球形的数据; 

  - 解决方法: 使用密度聚类算法;

- 数据量比较大时收敛较慢; 

  - 解决方法: Mini Batch K-MEANS;

### 2. DBSCAN

#### 1) 基本概念

基于密度的聚类算法,  将具有足够高密度的区域划分为簇, 并可以发现任何形状的聚类;

**𝛆邻域:** 给定对象半径𝜀内的区域称为该对象的𝜀邻域;

**核心对象**: 如果给定𝜀 邻域内的样本点数大于等于Minpoints，则该对象为核心对象;

**直接密度可达:** 给定一个对象集合D，如果p在q的𝜀邻域内，且q是一个核心对象，则我们说对象p从出发是直接密度可达的;

**密度可达**：集合D，存在一个对象链p1,p2…pn,p1=q,pn=p,pi+1是从pi关于𝜀和Minpoints直接密度可达，则称点p是从q关于𝜀和Minpoints密度可达的。

**密度相连**：集合D存在点o，使得点p、q是从o关于𝜀和Minpoints密度可达的，那么点p、q是关于𝜀和Minpoints密度相连的。

#### 2) 缺点

- 当数据量增大时，要求较大的内存支持I/O消耗也很大;
- 当空间聚类的密度不均匀、聚类间距差相差很大时，聚类质量较差。

#### 3) 优点

- DBSCAN不需要输入聚类个数;
- 聚类簇的形状没有要求;
- 可以在需要时输入过滤噪声的参数;

## 五. 主成分分析(PCA)

找到数据最重要的方向(方差最大的方向), 第一个主成分就是从数据差异性最大(方差最大)的方向提取出来的，第二个主成分则来自于数据差异性次大的方向，并且要与第一个主成分方向正交。

**PCA不仅可以帮忙减少计算量及模型的复杂程度, 同时也可以帮忙过滤掉不太重要的特征, 有时去掉一些噪声特征可以提升建模的准确度!**

#### 算法流程

1. 数据预处理：中心化𝑋 − 𝑋平均值
2. 求样本的协方差矩阵(1/m)*𝑋𝑋<sup>𝑇</sup>。
3. 对协方差矩阵做特征值分解。
4. 选出最大的k个特征值对应的k个特征向量。
5. 将原始数据投影到选取的特征向量上。
6. 输出投影后的数据集。

#### 协方差

**方差**: 描述一个数据的离散程度

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319150205625.png" alt="image-20210319150205625" style="zoom:50%;" />

**协方差**: 描述两个数据的相关性，接近1就是正相关，接近-1就是负相关，接近0就是不相关。

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319150239566.png" alt="image-20210319150239566" style="zoom:50%;" />

协方差只能处理二维问题，那维数多了自然需要计算多个协方差，我们可以使用矩阵来组织这些数据;

**协方差矩阵**: 是一个对称的矩阵，而且对角线是各个维度的方差。

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319150651699.png" alt="image-20210319150651699" style="zoom:50%;" />

以上推导建立在数据均值标准化(中心化)的基础上, 均值标准化后数据平均值为0

<img src="C:\Users\buer\AppData\Roaming\Typora\typora-user-images\image-20210319150707447.png" alt="image-20210319150707447" style="zoom:50%;" />