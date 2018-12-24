## 一、信息论基础（熵 联合熵 条件熵 信息增益 基尼不纯度）
1. 熵（Entropy）  
1.1 一条消息可以携带多少信息（以二分类为例）  
假设数据有正例（pos）和反例（neg）两类，我们从数据集中随机选取一个样例，**“结果显示这个样例是正例”这句话携带了多少信息**？<br/>  
先给出公式：
![](http://latex.codecogs.com/gif.latex?I_%7Bpos%7D%3D-%7B%5Clog_%7B2%7DP_%7Bpos%7D%7D)其中P是该样例是正例pos的概率。  
① 我们可以这样想：如果数据集中所有的样例本来就都是正的（即P=1），那么我们说“随机抽取一个样例是正例”这句话就是一句废话，本来事实就是大家都是正的，还需要你再说一遍干啥，这句话一点信息含量都没有，所以最后I=0.  
② 如果事实是这些数据中99%概率的样例都是正的，那么这时候如果有个人跳出来说“我随机从中抽取一个样例，它结果是正的”，这个时候大家也觉得这句话没有太大信息含量，因为不管怎么抽，结果大概率都是正的。最后结果也表现出来![](http://latex.codecogs.com/gif.latex?I_%7Bpos%7D%3D-%7B%5Clog_%7B2%7D0.99%7D)基本接近于0.  
③ 如果数据集中为正的概率只有50%，这个时候有人跳出来说“我刚才抽取了一个样例，它是正的”，只有当概率值为这个值的时候大家才会觉得，这个人说话有最高的信息量，因为大家无法实现对样例的正负进行估计。<br/>  
1.2 一个离散型变量X所携带的信息量  
一个离散型变量X的熵H(X)定义为：![](https://www.zhihu.com/equation?tex=H%28X%29%3D-%5Csum%5Climits_%7Bx%5Cin%5Cmathcal%7BX%7D%7Dp%28x%29%5Clog+p%28x%29)  
H(X)也就是平均信息量，这里的χ是变量x的取值集合，H(X)其实就是X取各种能取的值所携带的平均信息量。  
2. 联合熵  
以二元随机变量X,Y举例，两个变量X和Y的联合信息熵定义为![](https://gss0.bdstatic.com/94o3dSag_xI4khGkpoWK1HF6hhy/baike/s%3D269/sign=eb3bf749dd58ccbf1fbcb23c20dabcd4/fd039245d688d43fdf71cf09711ed21b0ff43b74.jpg)其中x和y是X和Y的特定值, P(x
,y)是这些值一起出现的联合概率。  
一集变量的联合熵大于或等于这集变量中任一个的独立熵，但少于或等于这集变量的独立熵之和。  
3. 条件熵  
仍以二元变量举例，H（X|Y）代表在已知一个变量Y发生的条件下，另一个变量X发生所新增的不确定性。  
![](https://images2018.cnblogs.com/blog/1361042/201804/1361042-20180403085847838-130152886.png)  
条件熵 H(Y|X)H(Y|X) 相当于联合熵 H(X,Y)减去单独的熵 H(X)，即H(Y|X)=H(X,Y)−H(X)，证明如下：  
![](https://images2018.cnblogs.com/blog/1361042/201804/1361042-20180403094218843-1537760199.png)
4. 信息增益（information gain）以二分类为例  
假设属性at把数据集T划分成了若干个子集Ti，每个Ti里的样例又可以标记为2类：pos和neg。  
①首先，根据上面熵的定义，我们可以求出每个子集Ti的熵H（Ti）。  
②我们可以求出被属性at划分过的数据集T的熵H(T,at)  
![](http://latex.codecogs.com/gif.latex?H%28T%2Cat%29%3D%5Csum%20P_%7Bi%7D%5Ccdot%20H%28T_%7Bi%7D%29)  
其中Pi是每个Ti的概率，实际是用相对频率(Ti中样例的个数除以总数据集T中样例的个数)算出来的，即Pi=|Ti|/|T|。  
③用属性at对数据集T进行划分所得到的**信息增益（information gain）**就是I（T,at）。  
![](http://latex.codecogs.com/gif.latex?I%28T%2Cat%29%3D%20H%28T%29-H%28T%2Cat%29)  
信息增益I实际上是数据集T被at划分后减小的熵，为什么划分后熵会减小？因为在划分过程中我们的信息是愈加明确的，所以熵会减小。  
5. 基尼指数Gini index（基尼不纯度）  
基尼指数（基尼不纯度）= 样本被选中的概率 * 样本被分错的概率  
![](https://images2015.cnblogs.com/blog/1094293/201703/1094293-20170317141739932-1276625834.png)  
Gini指数越小表示集合中被选中的样本被分错的概率越小，也就是说集合的纯度越高，反之，集合越不纯。  
注：当为二分类时，Gini(P) = 2p(1-p)。

## 二、决策树的不同分类算法（ID3算法、C4.5、CART分类树）的原理及应用场景
生成决策树的关键在于每次选取什么样的属性去进行划分。一般而言，随着划分过程的不断进行，我们希望决策树的分支节点所包含的样本尽可能属于同一类别，即节点的纯度（purity）不断提高。  
1. ID3算法（iterative Dichotomiser 3）Quinlan，1986  
1.1 Calculate the entropy of the training set, T, using the
percentages, p(pos) and p(neg), of the positive and negative.  
1.2 For each attribute, at, that divides T into subsets, Ti, with relative sizes Pi, do the following:  
      ①：calculate the entropy of each subset, Ti;  
      ②：calculate the average entropy: H(T, at);  
      ③：calculate information gain: I(T, at);  
1.3 Choose the attribute with the highest value of information gain and Divide T into subsets, Ti;  
1.4 For each Ti:
If all examples in Ti belong to the same class, then create a
leaf labeled with this class; otherwise, apply the same
procedure recursively to each training subset.<br/>  
ID3的缺点：  
① 处理大型数据速度较慢，经常出现内存不足;  
② 不可以处理数值型数据（可以通过将连续值转换为关于某个threshold的布尔问题来解决，问题就变为了如何选择合适的threashold，同样可以用信息增益来得出）<br/><br/>   
2. C4.5决策树算法  
事实上，信息增益对可取值数目较多的属性有所偏好，为减少这种偏好可能带来的不利影响，C4.5算法不直接采用信息增益，而是采用gain ratio（增益率）选择最优分化属性。
增益率定义为![](http://latex.codecogs.com/gif.latex?Gain_ratio%28D%2Ca%29%3D%5Cfrac%7BGain%28D%2Ca%29%7D%7BIV%28a%29%7D)  
其中![](http://latex.codecogs.com/gif.latex?IV%28a%29%3D%20-%5Csum_%7Bv%3D1%7D%5E%7BV%7D%5Cfrac%7B%7CD%5Ev%7C%7D%7B%7CD%7C%7Dlog_%7B2%7D%5Cfrac%7B%7CD%5E%7Bv%7D%7C%7D%7B%7CD%7C%7D)  
它的本质是在信息增益的基础之上乘上一个惩罚参数。特征个数较多时，惩罚参数较小；特征个数较少时，惩罚参数较大。  
通过C4.5算法构造决策树时，信息增益率最大的属性即为当前节点的分裂属性，随着递归计算，被计算的属性的信息增益率会变得越来越小，到后期则选择相对比较大的信息增益率的属性作为分裂属性。
3. CART分类树  
CART假设决策树是二叉树，内部结点特征的取值为“是”和“否”，左分支是取值为“是”的分支，右分支是取值为“否”的分支。这样的决策树等价于递归地二分每个特征，将输入空间即特征空间划分为有限个单元，并在这些单元上确定预测的概率分布，也就是在输入给定的条件下输出的条件概率分布。  
CART算法由以下两步组成：  
①：决策树生成：基于训练数据集生成决策树，生成的决策树要尽量大；  决策树剪枝：用验证数据集对已生成的树进行剪枝并选择最优子树，这时损失函数最小作为剪枝的标准。  
②：CART决策树的生成就是递归地构建二叉决策树的过程。CART决策树既可以用于分类也可以用于回归。本文我们仅讨论用于分类的CART。对分类树而言，CART用Gini系数最小化准则来进行特征选择，生成二叉树。  
根据训练数据集，从根结点开始，递归地对每个结点进行以下操作，构建二叉决策树：  
设结点的训练数据集为D，计算现有特征对该数据集的Gini系数。此时，对每一个特征A，对其可能取的每个值a，根据样本点对A=a的测试为“是”或 “否”将D分割成D1和D2两部分，计算A=a时的Gini系数。  
在所有可能的特征A以及它们所有可能的切分点a中，选择Gini系数最小的特征及其对应的切分点作为最优特征与最优切分点。依最优特征与最优切分点，从现结点生成两个子结点，将训练数据集依特征分配到两个子结点中去。对两个子结点递归地调用步骤1~2，直至满足停止条件。   
生成CART决策树：  
算法停止计算的条件是结点中的样本个数小于预定阈值，或样本集的Gini系数小于预定阈值（样本基本属于同一类），或者没有更多特征。

## 三、回归树原理

## 四、决策树防止过拟合手段

## 五、模型评估

## 六、sklearn参数详解，Python绘制决策树
