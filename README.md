# 我要炒美股
对2020-2024年美股数据的分析<br>

<strong> 数据集来源 </strong>
<a href="https://www.kaggle.com/datasets/dhavalpatel555/us-stock-market-2020-to-2024/" title="kaggle数据">Kaggle</a>
kaggle：<br>


<strong> 数据预处理 </strong><br>

<strong> 数据相关性分析 </strong><br>

<strong> 几种机器学习模型的拟合尝试 </strong><br>

<strong> 分别使用arima模型和多元线性回归模型对未来股价进行预测 </strong><br>

<strong> 结合机器学习模型对多元线性回归进行改进，R2提升近20% </strong><br>


# 数据预处理
## 数据预览

**概述**  
数据预览是在深入分析之前对数据进行初步查看的过程。这一步骤对于理解数据的基本情况、发现数据的潜在问题以及规划后续的数据处理策略至关重要。

### 重要性

- **理解数据结构**：识别数据集中的变量和记录，了解它们的类型和组织方式。
- **评估数据质量**：发现和识别缺失值、异常值、重复记录等数据质量问题。
- **初步分析趋势**：通过简单的统计分析和可视化，对数据集的基本趋势和模式有一个初步认识。

### 数据预览的常见步骤

#### 1. 查看数据维度
- 确定数据集包含的行数和列数。

#### 2. 检查列名称和数据类型
- 查看每个列的名称和数据类型，确保它们与数据集的描述相符。

#### 3. 浏览前几行数据
- 查看数据集的前几行，以获取数据内容的直观感受。

#### 4. 统计描述
- 使用描述性统计分析来了解数据的中心趋势、分散程度和分布形态。

#### 5. 缺失值和异常值检查
- 统计每列的缺失值数量，识别可能的异常值。

#### 6. 初步可视化
- 利用图表（如直方图、散点图等）来初步探索数据的分布和关系。

## 数据格式的统一

**概述**  
在数据分析和挖掘项目中，来自不同来源的数据往往存在格式不一致的问题，这可能包括日期格式、文本编码、数字表示等方面的差异。数据格式的统一是将这些不同格式的数据转换成一种标准格式，以便于后续的数据处理、分析和建模。

### 重要性

- **便于数据整合**：统一的数据格式使得来自不同来源的数据可以轻松合并。
- **提高数据质量**：统一处理过程中，可以识别并纠正数据中的错误和异常值。
- **简化数据处理流程**：一旦数据格式被统一，数据的进一步处理和分析将变得更加直接和高效。

### 数据格式统一的常见方面

#### 1. 日期和时间格式
- 日期和时间是数据分析中常见的变量，不同的数据源可能采用不同的表示方式（如`DD/MM/YYYY`、`MM-DD-YYYY`等）。统一日期格式是确保时间序列分析正确性的关键。

#### 2. 文本和编码
- 文本数据可能因为语言、编码（如UTF-8、ASCII）的不同而呈现多样性。统一文本和编码格式有助于避免乱码，确保文本数据的正确解析和显示。

#### 3. 数值格式
- 数值数据可能因为小数点的使用、千位分隔符的存在、单位的不同（如公里与英里）等因素而需要统一处理。

#### 4. 分类数据的表示
- 对于分类变量，不同的数据集可能使用不同的标签来表示同一类别（如，“是/否”与“1/0”）。统一这些表示有助于后续的分析和建模。<br>

## 数据缺失值的填补
### 缺失值

数据准备过程中约70%的工作量是由于需要解决数据质量问题，其中缺失值和特殊值问题尤为突出。处理这些问题不仅对于保持数据的完整性至关重要，而且对于确保后续数据分析的准确性也非常关键。

#### 缺失值的概念

- **数据库中的NULL值**：直接表示数据缺失。
- **特殊数值**：如系统中使用-999表示数值不存在，需要特别注意这些代表缺失的特殊数值。

#### 缺失值产生的原因

1. **机械原因**：数据收集或保存失败，如存储器损坏。
2. **人为原因**：主观失误、历史局限或有意隐瞒，如调查中的无效回答。

## 缺失值处理方法

一般来说，用一个常数填充缺失值通常不是最佳选择。建立基于数据分布的模型来填充缺失值通常效果更好
下面是常用的一些填补缺失值的方法，这个数据集我们是使用的Knn填补<br>

### 使用0值填补缺失值

**概述**  
在数据预处理时，将缺失值填补为0是一种简单直接的方法。这种方法假设缺失值的实际意义是“无”或“没有发生”，适用于那些可以将缺失解释为“不存在”的场合。

#### 何时使用0值填补

- **适用情境**：数据中的缺失表示没有或为空。
- **计数和频率数据**：在计数或频率数据中，0可以表示“未观察到”事件。
- **需要特别标记**：在某些机器学习模型中，0可以作为一种特别的标记，明确表示信息的缺失。

#### 方法步骤

##### 1. 确定适用性
评估数据集和上下文，确定是否适合使用0值填补缺失值。

##### 2. 填补缺失值
直接将缺失值替换为0。<br>

### 使用上一个数填补缺失值

**概述**  
在处理时间序列数据或具有自然排序的数据集时，一种常见的填补缺失值的方法是使用前一个观测值来替换缺失值。这种方法假设数据具有连续性，且相邻的观测值之间差异不大。

#### 何时使用前一个值填补

- **时间序列数据**：数据点是按时间顺序排列的。
- **自然排序数据**：数据点之间存在逻辑上的顺序关系。
- **连续性假设**：假设数据点之间的值变化平滑，没有突变。

#### 方法步骤

##### 1. 排序数据（如果需要）
确保数据是按照正确的顺序排序的，特别是在处理时间序列数据时，这一点非常重要。

##### 2. 填补缺失值
将每个缺失值替换为其前一个非缺失值。<br>

### 使用平均数填补缺失值

**概述**  
在数据集中，数值型特征的缺失值可以通过多种方法进行填补。使用平均数填补是其中一种简单直接的方法，适用于那些假定数据分布大致呈正态分布且没有极端值（异常值）的情况。

#### 何时使用平均数填补

- **数值型数据**：适用于连续的数值型数据特征。
- **数据缺失随机**：当数据缺失是随机发生的，不是系统性缺失时。
- **无极端值影响**：当数据中没有极端值，或者极端值对整体分布影响不大时。

##### 方法步骤

###### 1. 计算平均数
计算数据集中每个数值型特征的平均数（算术均值）。

##### 2. 替换缺失值
将每个数值型特征中的缺失值替换为该特征的平均数<br>

### 使用众数填补缺失值

**概述**  
在数据预处理过程中，填补缺失值是常见的任务，尤其是当数据集在进行机器学习算法训练之前需要完整无缺时。使用众数（即最常见的值）来填补缺失值是处理分类数据的一种简便方法。

#### 何时使用众数填补

- **分类数据**：当数据是分类的（非连续的），且存在一个或多个高频出现的类别时。
- **无明显中心趋势**：当数据没有明显的平均值或中位数时，众数成为表示中心趋势的一个合理选择。
- **避免影响分布**：使用众数可以在一定程度上避免改变原始数据的分布。

#### 方法步骤

##### 1. 确定众数
计算数据集中每个分类变量的频率分布，并确定每个变量的众数。

##### 2. 替换缺失值
对于含有缺失值的每个分类变量，将缺失值替换为该变量的众数。<br>

### 使用K最近邻（KNN）填补缺失数据
后面有对knn的具体描述,这个数据集是用knn填补的缺失值<br>
**概述**  
K最近邻（KNN）是一种简单而有效的算法，不仅可以用于分类或回归问题，还可以用于数据预处理，比如填补缺失值。利用KNN填补缺失值涉及到找到缺失值数据点的K个最近邻居，并用这些邻居的相应特征值来估计缺失值。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/83ac6dd4-125c-427d-9084-3ede07ebbc01)

#### 关键步骤

#### 1. 确定K值
选择合适的K值是使用KNN填补缺失值的关键。K值过小可能会受到异常值的影响，而K值过大则可能会引入过多的变异性。通常情况下，K值的选择可以通过交叉验证来确定。

#### 2. 选择距离度量
确定衡量数据点间相似度的距离度量。常见的选择包括欧氏距离、曼哈顿距离等。

#### 3. 找到最近邻居
对于含有缺失值的数据点，找到其在已知数据集中的K个最近邻居。只有完整的数据点才能被考虑作为邻居。

#### 4. 计算缺失值
根据找到的邻居来估算缺失值。对于连续特征，可以使用邻居值的平均值或加权平均值；对于分类特征，可以使用模式（最频繁出现的值）。<br>

### 使用BP神经网络填补数据

**概述**  
BP神经网络，即反向传播神经网络，是一种监督学习算法，用于训练多层前馈神经网络。通过训练过程中的错误反向传播，网络能够学习并改进其预测。这种方法也可用于填补数据集中的缺失值，特别是在缺失模式复杂或数据集非线性时。<br>

![image](https://github.com/baibairui/stock_pridict/assets/123094711/112579ab-c04e-4d48-aedd-2ae6d083ae8e)

#### 关键步骤

##### 1. 数据预处理
在训练BP神经网络之前，重要的是首先对数据进行预处理。这包括标准化或归一化数据，以使网络更容易学习。

##### 2. 设计神经网络
选择合适的网络架构，包括输入层、一个或多个隐藏层以及输出层。输入层的神经元数量应与完整数据集中的特征数量相匹配，输出层的神经元数量应与要预测的缺失值数量相同。

##### 3. 确定损失函数和优化器
定义损失函数来量化预测值与真实值之间的差异。常用的损失函数有均方误差（MSE）等。选择一个优化器，如SGD或Adam，来最小化损失函数。

##### 4. 训练网络
使用包含缺失值的数据集进行训练。为了训练网络填补缺失值，可以使用仅包含已知值的特征来预测缺失的特征值。

##### 5. 填补缺失值
一旦网络训练完毕，使用它来预测缺失的数据点。网络的输出可以作为填补缺失值的估计。<br>

### 热卡填充（Hot Deck Imputation）

**概述**  
热卡填充是一种用于处理缺失数据的方法，它通过在数据集中找到一个与含有缺失值的对象最相似的完整对象，并用这个相似对象的值来填补缺失值。这种方法在概念上很简单，能够有效地利用数据间的内在关联性进行缺失值估计。

#### 方法特点

- **利用相似性**：基于相似性寻找最佳替代值，以保持数据的一致性和准确性。
- **主观性**：相似性标准的定义可能包含主观判断，不同问题可能需要不同的相似性标准。
- **灵活性**：可以根据具体问题调整相似性的定义和寻找最近邻的方法。

#### 应用步骤

##### 1. 定义相似性标准
根据数据的特性和分析的需要，定义一个合理的相似性标准。这可以基于某些关键特征，如性别、年龄等，也可以是基于复杂的距离度量，如欧氏距离或余弦相似性。

##### 2. 寻找最相似的对象
对于每个含有缺失值的对象，根据定义的相似性标准，在数据集中找到一个最相似的完整对象。

##### 3. 填补缺失值
使用找到的最相似对象的值来填补缺失值。

#### 缺点和考虑因素

- **定义相似性的难度**：在不同的问题和数据集中，定义一个合适的相似性标准可能很有挑战。
- **计算复杂性**：对于大型数据集，寻找最相似对象可能会很耗时。
- **数据代表性**：所选择的最相似对象可能不完全代表缺失值的真实情况。

#### 示例场景

- 在医疗数据分析中，可以使用热卡填充来填补患者记录中的缺失值，如通过性别、年龄和其他健康指标来寻找最相似的患者记录。
- 在客户调研数据中，对于缺失的调研回应，可以根据客户的人口统计信息和以往的调研回应来找到最相似的客户填补缺失值。

热卡填充提供了一种实用的方法来处理缺失数据，尤其是在缺失数据较少且能够定义出合理的相似性标准时。然而，它的成功应用需要对数据有深入的了解和仔细的考量。

### 多重填补（Multiple Imputation, MI）

**概述**  
多重填补（MI）是一种基于贝叶斯估计理念的缺失数据处理方法。它通过为每个缺失值生成多个可能的填补值来创建几个完整的数据集，然后对这些数据集分别进行分析，并最终综合分析结果以考虑因数据缺失而产生的不确定性。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/f8d05255-a810-4895-be0c-3614c3bfd564)

#### 方法原理

- **思想来源**：MI的核心思想来源于贝叶斯估计，认为待填补的值是随机的，由已观测到的值生成。
- **实践方法**：具体实践中，先估计出待填补的值，然后加上不同的噪声，形成多组可选填补值。通过某种标准选择最合适的填补值。

#### 多重填补步骤

1. **生成填补值**：为每个空值生成一套可能的填补值，这些值反映了无响应模型的不确定性。
2. **创建完整数据集**：使用生成的填补值填补缺失值，产生若干个完整的数据集合。
3. **进行统计分析**：对每个填补的数据集进行统计分析。
4. **综合分析结果**：将各个填补数据集的结果综合起来，产生最终的统计推断，考虑到了由于数据填补而产生的不确定性。

#### 应用实例

假设一组数据包含三个变量Y1，Y2，Y3，它们的联合分布为正态分布。在多值插补时，对不同组别的缺失模式（如B组缺失Y3，C组缺失Y1和Y2）采取不同的填补策略。

#### 考虑因素

- **模型形式**：贝叶斯估计要求模型形式准确，而多重插补基于大样本理论，对先验分布的准确性要求不高。
- **参数间关系**：多重插补利用参数间的相互关系进行估计，与贝叶斯估计相比，它能更好地利用参数之间的联系。

#### 优点

- 弥补了贝叶斯估计的不足，尤其是在大数据场景下，先验分布对结果的影响较小。
- 利用了参数间的相互关系，比贝叶斯估计更能体现参数之间的依赖性。

#### 缺点

- 计算复杂性高，需要生成多个完整数据集并对每个数据集进行分析。
- 定义相似性标准可能包含主观判断，不同问题可能需要不同的相似性标准。

多重插补是一种有效处理缺失数据的方法，尤其适用于缺失数据量大且缺失模式复杂的数据集。通过综合考虑多个可能的填补值，MI能够更准确地反映数据的不确定性，为后续分析提供稳健的基础。<br>

# 数据相关性分析
## 绘制直方图观察数据分布情况<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/74cccf6a-79a1-4093-8da2-091a538babc8)


## 绘制时序数据图并添加布林线和移动平均线来确定波动性<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/0f09907c-94c6-413c-bac0-ae8ed21409d4)


## 绘制相关性热力图<br>
   ![image](https://github.com/baibairui/stock_pridict/assets/123094711/3449ea93-d018-47e5-b66d-f5b73883d4e8)

## 对相关性大的数据进行格兰杰因果检验<br>

## 对数据进行因子分析<br>

## 特征工程<br>


# 机器学习模型拟合
## xgboost
XGBoost（eXtreme Gradient Boosting）是一个高效的、灵活的、可移植的机器学习库，专门针对提升树（boosted trees）算法进行了优化。它在Gradient Boosting框架的基础上进行了改进，旨在提高模型的性能和速度。XGBoost在众多机器学习竞赛中展现了其强大的性能，包括Kaggle竞赛中的多个冠军解决方案。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/bfda4f2a-0e35-4694-9038-35d4a697c851)

**核心特性**

XGBoost拥有一系列引人注目的特性，主要包括：

- **高效实现**：支持多种语言（包括Python、R、Java等），并且能够在单机、Hadoop、Spark、Flink等多种分布式环境中运行。
- **灵活性**：支持回归、分类、排名等多种机器学习任务。
- **可扩展性**：通过近似算法（如分位数倾斜）有效处理大规模数据集。
- **剪枝**：提供深度优先和宽度优先的剪枝策略，从而避免过拟合。
- **正则化**：内置L1和L2正则化项，有助于减少模型的复杂性和提高模型的泛化能力。
- **缺失值处理**：自动处理缺失值。

**使用场景**

XGBoost可以应用于广泛的机器学习任务中，包括但不限于：

- **分类问题**：二分类、多分类问题
- **回归问题**：预测连续值
- **排序问题**：如搜索排序
- **时间序列预测**

## lstm
长短期记忆网络（Long Short-Term Memory, LSTM）是一种特殊类型的循环神经网络（RNN），能够学习长期依赖关系。LSTM由Hochreiter & Schmidhuber（1997）引入，旨在解决传统RNN在处理长序列数据时遇到的梯度消失和梯度爆炸问题。LSTM在多个领域，包括自然语言处理、语音识别和时序数据分析等，都有广泛应用。
![image](https://github.com/baibairui/stock_pridict/assets/123094711/40d8e14b-8769-42b7-a1e3-4ec91784b673)

**核心特性**

LSTM的关键在于其内部结构，尤其是“门”（gate）机制，包括：

- **遗忘门**：决定哪些信息需要被遗忘或丢弃。
- **输入门**：决定哪些新信息被存储在细胞状态中。
- **输出门**：决定下一个隐藏状态和当前细胞状态的关系，这影响了网络下一步的输出。

这些门的结构使LSTM能够有效地添加或移除信息，解决了长期依赖的问题。

**使用场景**

LSTM适用于各种涉及序列数据的任务，例如：

- **自然语言处理**（NLP）：包括文本生成、机器翻译、情感分析等。
- **语音识别**：将语音转换为文本。
- **时间序列预测**：如股票价格预测、天气预测等。

## knn
K最近邻（K-Nearest Neighbors, KNN）是一种基本且广泛使用的监督学习算法，用于分类和回归。其工作原理极其简单：对于一个待预测的数据点，算法会在训练集中找到与之最近的K个邻居，然后根据这些邻居的信息来预测该数据点的标签。

**核心思想**

KNN算法的核心思想可以概括为“物以类聚，人以群分”。其判断依据基于邻居的多数投票（对于分类问题）或平均（对于回归问题）。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/0ac9eeb8-a32d-4c94-871a-ec8ee39fe283)

**关键特性**

- **非参数学习**：KNN是一种非参数学习算法，不假设数据的分布，可以捕捉到数据的复杂关系。
- **简单但有效**：尽管算法结构简单，但在很多情况下仍然非常有效。
- **惰性学习**：KNN属于惰性学习算法，因为它实际上不从训练数据中学习一个判别函数，而是把数据库直接用于预测分类。

**应用场景**

KNN算法可以应用于多种场景，包括但不限于：

- **分类任务**：如文本分类、客户分群等。
- **回归任务**：如房价预测、股票价格等。
- **推荐系统**：根据用户的“邻居”（相似的用户）的喜好来推荐商品。

**算法步骤**

1. **确定K值**：选择一个适当的K值。
2. **计算距离**：计算未知数据点与所有已知数据点的距离。
3. **找到最近的K个邻居**：选择距离最近的K个点。
4. **投票或平均**：对于分类问题，根据最近邻居的多数类别来分类；对于回归问题，取最近邻居的输出变量的平均值作为预测值。

## cnn
卷积神经网络（Convolutional Neural Networks, CNN）是一种在计算机视觉任务中广泛使用的深度学习架构。CNN通过模拟人类视觉系统的机制，能够有效地识别图像中的模式和特征，如边缘、颜色和纹理等。CNN的强大能力使其在图像分类、对象检测、面部识别和许多其他领域都有广泛应用。<br>

![image](https://github.com/baibairui/stock_pridict/assets/123094711/edcb5d01-84de-4a40-ab32-e602b29885c2)

**核心组件**

CNN主要由以下几种类型的层构成：

- **卷积层（Convolutional Layer）**：通过卷积操作提取输入图像的特征。
- **激活层（Activation Layer）**：通常使用ReLU激活函数增加网络的非线性。
- **池化层（Pooling Layer）**：降低特征图的空间尺寸，减少计算量并提取重要特征。
- **全连接层（Fully Connected Layer）**：将学到的“高级”特征表示映射到样本的类别。

**工作原理**

1. **特征提取**：通过卷积层和池化层自动学习输入图像的特征表示。
2. **分类**：使用一个或多个全连接层对提取的特征进行分类。

**应用场景**

CNN在多个领域都有出色的表现，包括但不限于：

- **图像分类**：识别图像中的对象。
- **对象检测**：在图像中识别对象的位置和类别。
- **图像分割**：将图像分割成多个区域，用于进一步分析。
- **面部识别**：识别或验证个人的身份。

## Rnn
循环神经网络（Recurrent Neural Networks, RNN）是一类用于处理序列数据的深度学习模型。RNN能够处理不同长度的输入序列，并在内部维持一个状态（隐藏状态），该状态能够捕捉到目前为止观察到的序列的信息。RNN特别适合于时间序列数据、自然语言文本、语音等序列化信息的任务。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/7139af85-4605-43b4-b0c6-ad1b224f49a7)

**核心概念**

- **隐藏状态**：RNN的核心，它保存了过去时间点的信息。
- **时间步长**：序列中的每个元素被模型逐一处理的过程。
- **参数共享**：在处理序列的每个时间步时，RNN使用相同的权重，这有助于模型泛化。

**工作原理**

在每个时间步，RNN接收两个输入：当前时间步的输入数据和上一个时间步的隐藏状态。模型将这两个输入结合起来更新其隐藏状态，并可能生成一个输出。更新隐藏状态的过程会在序列的每个时间步重复进行，使得模型能够“记住”和综合过去的信息。

**应用场景**

RNN在许多涉及序列数据的任务中表现出色，包括：

- **自然语言处理**（NLP）：如文本分类、机器翻译、情感分析等。
- **时间序列预测**：如股票价格预测、天气预测等。
- **语音识别**：将语音信号转换为文本。

**RNN的变体**

尽管标准RNN在处理序列数据方面具有潜力，但它们经常遇到梯度消失或爆炸的问题，这限制了它们学习长距离依赖的能力。因此，开发了几种RNN的变体，如：

- **长短期记忆网络**（LSTM）：通过引入三种门控机制来解决梯度消失问题。
- **门控循环单元**（GRU）：比LSTM简单，但同样有效地解决了长距离依赖问题。

## 拟合效果比较
![image](https://github.com/baibairui/stock_pridict/assets/123094711/f64490b8-7160-4ff8-90db-f8687a05280d)

# 对未来数据进行预测
## 自回归积分滑动平均模型（ARIMA）介绍

自回归积分滑动平均模型（Autoregressive Integrated Moving Average, ARIMA）是时间序列分析中广泛使用的一种模型。它结合了自回归（AR）、差分（I）和滑动平均（MA）三个部分，旨在模拟时间序列数据中的一系列不同组件，如趋势、季节性等。<br>
ARIMA模型提供了另一种时间序列预测的方法。指数平滑模型（exponential smoothing）和ARIMA模型是应用最为广泛的两种时间序列预测方法，基于对这两种预测方法的拓展,很多其他的预测方法得以诞生。与指数平滑模型针对于数据中的趋势（trend）和季节性（seasonality）不同，ARIMA模型旨在描绘数据的自回归性（autocorrelations）。<br>
![image](https://github.com/baibairui/stock_pridict/assets/123094711/3c18fb45-d8b5-4c6a-add5-6949e681f6fb)


### ARIMA模型的三个组成部分

- **自回归（AR）**：`p` 部分表示模型中使用的时序数据的滞后数（lags），即过去值的数量。它捕捉了序列中的自回归特性。
- **差分（I）**：`d` 部分代表使序列平稳所需的差分次数，即连续计算前后数据点之间差异的次数。差分帮助去除数据中的趋势和季节性成分。
- **滑动平均（MA）**：`q` 部分是模型中使用的滑动平均项数，它捕捉了误差项的滑动平均特性。

### 模型表示

ARIMA模型通常表示为 ARIMA(p, d, q)，其中：
- `p` 是自回归项的阶数。
- `d` 是差分的阶数。
- `q` 是滑动平均项的阶数。

  其中的差分阶数通常需要通过ADF检验来确定，选择能使时间序列平稳的差分次数<br>
  ![image](https://github.com/baibairui/stock_pridict/assets/123094711/c1c43a66-0c3d-4aa4-add5-e9bdda928eb1)
![image](https://github.com/baibairui/stock_pridict/assets/123094711/0b17557b-4890-4201-92a8-a9dc60d0f23b)


### 工作原理

ARIMA模型通过结合自回归特性、差分以去除数据中的非平稳趋势和季节性成分、以及滑动平均来捕捉时间序列的未来点。模型首先对序列进行必要的差分，使其平稳，然后利用自回归和滑动平均模型来预测未来值。

### 应用场景

ARIMA模型在金融、经济、销售等领域的时间序列数据分析中有广泛应用，特别适用于：

- 销售预测
- 股票价格分析
- 经济指标预测
- 能源消耗预测


## 多元线性回归

### 多元线性回归介绍

多元线性回归（Multiple Linear Regression, MLR）是统计学中的一种线性回归模型，它用来估计两个或多个自变量（解释变量）与一个因变量（响应变量）之间的关系。与简单线性回归不同的是，多元线性回归分析允许你探索多个自变量对因变量的影响。<br>

![image](https://github.com/baibairui/stock_pridict/assets/123094711/8f088e3a-8141-4736-bb85-b1bc99102904)

### 模型定义

多元线性回归模型可以表示为：

![image](https://github.com/baibairui/stock_pridict/assets/123094711/3dfd1374-c090-47f9-af94-9d4a03d80432)


其中：

- \( Y \) 是因变量（目标变量）。
- \( X_1, X_2, \ldots, X_n \) 是自变量（特征变量）。
- \( \beta_0 \) 是截距项，也称为常数项。
- \( \beta_1, \beta_2, \ldots, \beta_n \) 是每个自变量的系数，表示自变量对因变量的影响强度。
- \( \epsilon \) 是误差项，表示模型无法解释的随机变异。

### 应用场景

多元线性回归广泛应用于经济学、社会科学、生物统计和工程等领域，用于：

- 预测分析：例如，预测房价、销售额、股票价格等。
- 风险评估：例如，在金融领域评估信贷风险。
- 优化策略：通过理解不同因素对结果的影响，帮助企业或个人做出更好的决策。

### 假设

多元线性回归分析基于以下几个假设：
- 线性关系：自变量和因变量之间存在线性关系。
- 无多重共线性：模型中的自变量之间应相互独立。
- 同方差性：对于所有的观测值，残差的方差应相同。
- 正态分布的残差：残差（误差项）应呈正态分布。

# 对预测模型进行改进
1. 将Rnn和XGboost作为基层，多元线性回归模型作为元层，使用stack堆叠模型对基本的多元线性回归模型进行改进
2. 将改进前后的效果进行对比
   
