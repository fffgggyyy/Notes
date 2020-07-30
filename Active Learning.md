### 0.目的 

尽可能少的标记样本训练模型，同时达到较高的精度

样本数据类别分布不平衡，希望保留尽肯能“有价值”的样本。



### 1.介绍

主动学习算法可以主动地提出一些标注请求，将一些经过筛选的数据提交给专家进行标注。如图所示（图中的样本选择算法以**基于池(pool-based)**为例，后面第2章会具体讲解样本选择方法）

![image-20200730150026203](pic/Active%20Learning.assets/image-20200730150026203.png)

机器学习模型从少量已标记样本L开始训练，通过一定的样本选择算法选择出一个或者一批最有 **”价值“**（更大的信息量或者不确定性） 的样本交给专家oracle进行标记，标记后的样本加入训练集中参与下一轮的训练。



举个例子：下图中当标记样本较少后，通过主动学习，对具有不确定性的样本交给专家标记，能够显著提升逻辑回归的精度。

![image-20200730150040692](pic/Active%20Learning.assets/image-20200730150040692.png)

### 2.不同的咨询方式

![image-20200730150054298](pic/Active%20Learning.assets/image-20200730150054298.png)

三种情况：

- membership query synthesis
- stream-based selective sampling
- pool-based active learning



**membership query synthesis** 由模型生成咨询。如图像处理中给图片加噪声等，在自然语言任务中生成后往往会遇到语义不匹配等后果。

**Stream-Based Selective Sampling** 未标记的样例按先后顺序逐个提交给选择引擎，由选择引擎决定是否标注当前提交的样例，如果不标注，则将其丢弃。通常会设置一个阈值。

**Pool-Based Active Learning**  基于池(pool-based)的主动学习中则维护一个未标注样例的集合，由选择引擎在该集合中选择当前要标注的样例。

	- **基于不确定度缩减的方法** 信息熵
	- **基于版本缩减的方法** 训练后能够最大程度缩减版本空间的样例进行标注，在二值分类问题中，这类方法选择的样例总是差不多平分版本空间。
	- **基于泛化误差缩减的方法**  能够使未来泛化误差最大程度减小的样例
	- 其他方法 COMB算法 多视图主动学习 预聚类主动学习

### 3.咨询策略

评估一个实例是否应该被咨询。

- Uncertainty Sampling 如信息熵、least confident、margin
- Query-By-Committee 在分类问题中体现为让版本空间变小
- Expected Model Change 模型“改变”最大的标记样本为“有价值”的样本，这里对模型“改变”的衡量可以由梯度提升来体现
- 等等



### 4.相关应用、文章、代码展示

读了一篇[ICLR2018]Deep Active Learning for Named Entity Recognition

这篇文章主要是将监督学习融入命名实体识别中，结合主动学习时仅使用25%的训练语料就可接近使用完整数据的效果。

本文在命名实体识别中使用了一个CNN-CNN-LSTM模型，由CNN字符编码器和CNN词编码器以及一个长期短期记忆（LSTM）标签解码器组成。

本文的创新是引入active learning，与程序进行交互，学习过程由多个回合组成：在每个回合开始时，主动学习算法选择句子进行注释，对扩充数据集进行训练来更新模型参数，然后进入下一轮。

本文对active learning其中的三种query策略进行对比效果，分别是LC、MNLP、BALD。效果都比随机取样的baseline好。



### 5. 代码学习

找到了一些样例代码，如谷歌的active learning Playground。https://github.com/google/active-learning 还没有仔细学习。



