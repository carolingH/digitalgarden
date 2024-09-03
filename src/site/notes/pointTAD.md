---
{"dg-publish":true,"permalink":"/pointTAD/"}
---

PointTAD: Multi-Label Temporal Action Detection with Learnable Query Points
https://arxiv.org/abs/2210.11035
code:[https://github.com/MCG-NJU/PointTAD](https://github.com/MCG-NJU/PointTAD)
zhihu:https://zhuanlan.zhihu.com/p/591495791
source:NeurIPS 2022
nju MCG PCG

现实场景中的视频里，**不同类别的动作往往会同时发生**。因此，我们将研究重心转移到更加复杂的**多类别时序动作检测（Multi-label Temporal Action Detection, Multi-label TAD）**，在包含多种类别的动作标注的视频中检测出所有动作。有两类错误，S1虽然正确的预测了动作类，但因为只捕捉到了动作的语义关键帧而形成了不完整的动作框；S2较好的预测了动作的边界，但由于粗糙的动作表征导致了分类错误
![Pasted image 20240903002944.png](/img/user/picture/Pasted%20image%2020240903002944.png)
PointTAD采用query-based时序动作检测框架，包含两个部分：backbone network 和 action decoder，模型支持端到端训练和测试。将动作query解耦为一组query points和一个query vector，其中query points主要表示动作的时序位置，query vector表征动作特征。在每一个解码层，依据query point特征通过多尺度交互模块更新query vector，更新后的query vector再预测偏移量更新query point，每一层输出的query points和query vector都作为输入传递到下一层。
![](/img/user/picture/PointTAD.png)

三维输入

RGB ，query point  ，query vector  (_​ N_q x D)

**backbone network**
I3D
**Iterative point refinement** :

在训练过程中，query points被初始化在输入视频的中点，而后在每个解码层被对应的query vector所更新, 在第 l层解码层，query vector为 ​个query points预测偏移量 偏移​，**偏移量被参数​** 缩放后更新点的位置。

​ 时序中第j个点

​​

**action decoder**

**_设计pseudo segments_**？？？？？

将每层解码层更新后的query points转化为pseudo segments加入样本标签分配和回归损失函数计算。将query points映射为pseudo segments的函数记为T：​不同转换函数​ ​(Min-max 取点集, Partial Min-max)

多级交互模块

配合稀疏点表示，我们在动作解码器设计了多尺度交互模块，从**点层次**结合局部时序信息生成query point 特征、从**实例层次**对候选框内的时序点的关系建模。点层次和实例层次的可变形参数和动态卷积参数都由动作特征（query vector）通过线性变换得到，使得每个动作预测对其对应的时序点都有各自的变换，形成具有区分性的特征。
![PointTAD1.png](/img/user/picture/PointTAD1.png)
![](file:PointTAD1.png)

**点层次的局部可变形算子** ？？？

**实例层次的自适应动态卷积**
.......

![](file:picture/)