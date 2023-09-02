# Video Anomaly Detection 属于感知任务，还是认知任务？
***
> Topics: ** Dataset、Taxonomy、Time、Reproduction..... **

## Open Challenges and Opportunities
* 视频异常检测的“实时性”是否经得住推敲？**即时性异常**：跑步、异常物体的出现……**非即时性异常**：游荡……**认知型异常（存在因果）**：人出现在公路上，不能够作为判别异常的根据，若人是从人行道走到公路去开车，或刚从公路上的车里下来，然后走到人行道，那么这就属于正常事件；如果，这人不遵守交通规则横穿了马路，这那么这就属于异常事件——见Street Scene数据集。
* 异常具有场景依赖性，所以模型首先要理解场景，对场景进行建模。
* 对场景的切实理解、语义理解、语义解释；很多方法没有理解场景
* 小物体异常，景深的“干扰”
* 静止异常：帧预测框架一定不能够检测，性能下降显著；帧重构框架一定程度可以缓解，检测性能下降也非常明显。
* 语义规则异常：正常情况为，货车从A地拉货到B地，在B地卸下货物后空车返回A地，如此往复。若有一段时间货车空载从A地到B地，然后返回A地，则称此流程为异常。
* 因为无法收集所有的正常事件，所以仅仅通过将异常归结为未知情形，将导致未知正常被错误地判别为异常。
* 基于前景目标提取的方法，默认“异常事件发生在前景目标，不发生在背景”，这种假设本身就有问题。对于背景异常无法判别。
* 重构或预测方法，完全重构或预测像素，基于像素的特征存在太多冗余信息，这是否有必要？能否分级检测？
* 基于自编码器的方法，会存在同等映射的问题，这是否是所有生成模型的通病呢？
* 两大类：生成模型，判别模型；从弱监督层面看，基于弱监督的VAD在数据集上有更高的AUC性能表现。
* Human-related异常，要素可分解为：位置，动作，速度，姿势，轨迹，方向，蒙面——见IITB-Corridor数据集
* 模型对某些正常事件无法学习，比如帧重构或帧预测法，对于夜间有一辆车打开、关闭车灯，会错误地判别为异常；突然变化的正常物体，比如突然打开的雨伞，突然变色的红绿灯，闪烁的车灯**？？训练阶段是否有见过这种情况呢？见过，也许可以通过对训练集进行集中记忆来解决，如果没见过，这就输入未见正常判异常的情况了**
* 异常的性质，1.场景依赖性 2.稀有性——**现存所有方法**仅仅利用见过和没见过，都没有利用其稀有性质来辅助异常检测
* 异常事件的持续性，发生车祸后，不仅仅发生车祸的瞬间为异常，车祸发生后的时间段也应当为异常。这种需要依靠表观？
* 对于基于Object Detection的方法，与基于Object Detection的目标跟踪异常，十分依赖于目标检测器的检测性能；一方面，已知（Seen）类别漏检会印象性能，另一方面，检测类别受限，无法有效检测未知（Unseen）类别；还有，现有方法仅仅使用了它的bbox和confidence信息，没有利用起它的语义标签信息；
* 异常的稀少和多样，两个**发力点**
* 最复杂的挑战是智能异常（对抗样本），与正常模式相似。
* 视频数据的高维、复杂场景、遮挡、视频内容的高度交互、低像素
* 对正常模式的准确定义；提取有效的具有判别性的时空特征；考虑正常模式与异常模式它们之间或内部的差异性和相似性；考虑环境信息和环境的变化。
* 对于long-term异常，如游荡，估计只能利用RNN？因为不方便先把轨迹点弄出来，然后弄成图片送进CNN，或者，能够嵌入到memory中？建模长时间关系，貌似也可以用LSTM？？？也许，通过将LSTM嵌入到memory来捕获长时依赖
* **自编码器**变分自编码器是基础自编码器的改进，自编码器，仅仅重构低质量的图片，没有捕获高级内容信息，没有捕获类别信息。
* 利用变分自编码器学习分布，然后，构造低概率事件，这就是伪异常。或者，利用模型在特征空间在对特征进行判别，判别是否为正常的特征，以此在特征空间得到伪异常特征。
* 因为，有些images，也许在image-level不同，但在feature-level相同，所以在特征空间一定要使得它们远离。对比loss，紧凑loss
* 在one-class范式下进行训练，测试时，进行迭代精炼；关键是看看CVPR2020那篇自训练怎么搞；然后，这个事情，可以用于无监督，也可以用于半监督one-class；；；太水了！！
* Auto-encoder应该是有搞头，直接在latent space进行分类，但是如何操作呢？参考李宏毅老师的PPT。
* one-vs-rest多类别分类，具有一定的解释性，类似于决策树，每次剔除一个类；能否学习/构造一种特征，该特征每次都判别，若为是，则输出，若为否，则往下进行下一次的判别，并且特征结构继续进行变换，，如此一来，相当于每一个样本特征，都要经过层层筛选，那它才可以被判定为正常，否则，只要有其中一个关卡过不了，那就是异常；还有就是，在学习的过程中对于关卡做引导，使之具有解释性？
* **伪异常+样本不均衡**可以用于无监督视频异常检测，也可以用于弱监督视频异常检测。。。无监督肯定是可以的，，只要引入**不是正常的异常**，那么就可以使正常数据的分布表示更加紧凑
* **双流对抗系列**，伪异常双流+纯正常双流+试验充分+发CCF C绝对没问题！！！
* 

## Next Direction
* **Pseudo-sample** 伪异常一定要加，负采样会使模型学到的分布更加紧凑
* **聚类** 
* **异常稀有性** 用于彻底无监督和弱监督（这两种问题设置本质相通），利用统计信息做伪标签的初步判定
* **多类别分类** 
* **Energy-based Out-of-distribution Detection** 分布外检测 分类中：softmax->one-vs-rest->EBM->
* 

## Taxonomy
* 从监督信号进行分类
* 在线离线
* 是否关注全局，即是否提取前景目标 
* **point anomalies**通过分析单个数据的值即可识别，例如，观察到不被期望的物体。**contextual(conditional) anomalies**除值之外，还需要环境信息，例如，车在人行道是异常，在公路上是正常。**collective(group)anomalies**大量的数据实例组成异常，例如，银行有几个人是正常，但有几百人是异常。条件异常有更广泛的定义，可以包含其它两种类型，因此，推荐将视频异常视作条件异常。


## Survey
* [A Critical Study on the Recent Deep Learning Based Semi-Supervised Video Anomaly Detection Methods](./Survey/A%20Critical%20Study%20on%20the%20Recent%20Deep%20Learning%20Based%20Semi-Supervised%20Video%20Anomaly%20Detection%20Methods.pdf)`arXiv2021`



## Dataset
* UCSD Ped1
* UCSD Ped2
* CUHK Avenue
* ShanghaiTech
* Street Scene
* UBnormal
* IITB-Corridor
* ADOC
* X-MAN
* UCF-Crime
>Note：半监督与弱监督所用的数据集名称相同，但数据集内容的具体划分不同，所以它们之间没有可比性。

## Annotation
* 像素级标注：
* 帧级标注
* 视频级标注
* Bbox标注
* 异常种类标注

## Evaluation  Criterion
* Micro-AUC
* Macro-AUC
* EER（Equal Error Rate）
* TBDR（Track-Based Detection Rate）
* RBDR（Region-Based Detection Rate）

## Supervised



## Weakly Supervised


## Semi-Supervised
单分类问题，但有多个正常子类  
1.Comprehensive Regularization in a Bi-directional Predictive Network for Video Anomaly Detection`AAAI2022`

## Unsupervised
“级联分类”（两种视角）  
第一种视角：是否承接弱监督，若是，则用去掉视频级标注的训练集进行训练，用测试集进行测试；若否，则没有训练集，直接测试集  
第二种视角：**Partial mode**：直接上测试集，丢弃训练集 **Merge mode**：将训练集与测试集混合之后用于无监督视频异常检测
1. A Discriminative Framework for Anomaly Detection in Large Videos`ECCV2016`
2. Unmasking the Abnormal Events in Video`ICCV2017`
3. Classifier Two-Sample Test for Video Anomaly Detections`BMVC2018`
4. Self-Trained Deep Ordinal Regression for End-to-End Video Anomaly Detection`CVPR2020`
5. Generative Cooperative Learning for Unsupervised Video Anomaly Detection`CVPR2022`
6. Deep Anomaly Discovery from Unlabeled Videos via Normality Advantage and Self-Paced Refinement`CVPR2022`


## 说法
* 由于ShanghaiTech数据集比UCSD数据集拥有更高的像素，所以它更容易通过表观特征来识别异常物体
* Street Scene数据集最具有挑战性，异常高度依赖于环境




## 话术
异常事件包含大量信息


