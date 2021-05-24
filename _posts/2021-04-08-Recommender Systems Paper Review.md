---
title: 推荐系统论文泛读
layout: single
classes: wide
categories:
  - Theory
tags:
  - Recommender System
toc: true
toc_label: "目录"
toc_icon: "cog"
last_modified_at: 2021-05-24
---





[**Neural Architecture Generator Optimization**](paper地址: https://arxiv.org/pdf/2004.01395v3.pdf)

推荐原因：本文提出了一种基于连续优化的自动神经网络设计的方法（neural architecture optimization(NAO)）。主要有以下三个步骤：

1. 利用了一个编码器（Encoder）将神经网络节奏映射到了一个连续空间。
2. 将一个网络的连续表示作为输入传入预测器（Predictor）并预测这个网络的准确率。
3. 再通过一个解码器（Decoder）将一个网络的连续表示映射回网络的结果。性能预测器和编码器使得我们能够在连续空间上进行梯度优化，从而找到一个拥有更高准确率的新结构的编码。再将编码通过解码器解码成一个网络。本文在CIFAR-10和PTB上做了实验，分别得到了2.11%的测试错误率和56.0的测试集困惑度（test set perplexity），并通过发现的最好的结构进行迁移到CIFAR-100和WikiText-2。

[相关summary](https://blog.csdn.net/favorxin/article/details/90206319) | [代码](code：https://github.com/huawei-noah/vega)



[StyleCLIP: Text-Driven Manipulation of StyleGAN Imagery](paper地址: https://arxiv.org/pdf/2103.17249v1.pdf)



推荐原因：受StyleGAN在各种领域生成高度逼真的图像的能力的启发，许多最新工作集中在理解如何使用StyleGAN的潜在空间来操纵生成的和真实的图像。然而，发现在语义上有意义的潜在操纵通常涉及艰苦的人类对许多自由度的检查，或涉及每个所需操纵的带注释的图像集合。在这项工作中，作者探索利用最新引入的对比语言-图像预训练（CLIP）模型的功能，以便为StyleGAN图像处理开发基于文本的界面，而无需进行此类人工操作。我们首先介绍一种优化方案，该方案利用基于CLIP的损失来修改输入潜在矢量，以响应用户提供的文本提示。接下来，我们描述一个潜在映射器，该映射器针对给定的输入图像推断出文本引导的潜在操作步骤，从而实现更快，更稳定的基于文本的操作。最后，我们提出了一种在StyleGAN样式空间中将文本提示映射到与输入无关的方向的方法，从而实现交互式文本驱动的图像处理。广泛的结果和比较证明了我们方法的有效性。

[相关summary](https://www.linkresearcher.com/theses/e48d45e7-4243-4725-a17f-5ee54c6d2461) | [代码](https://github.com/orpatashnik/StyleCLIP)



[Neural Collaborative Filtering vs. Matrix Factorization Revisited](https://arxiv.org/pdf/2005.09683v2.pdf)



推荐原因：

1. libFM作者Rendle对前些年一篇神经网络做推荐系统的文章NCF的实验复现；
2. 作者用详细的实验结果和分析说明，在只有u v两个向量输入的情况下，直接算点积是效果最好的，比叠加神经网络要好得多



[相关summary](https://www.hegongshan.com/2020/09/30/recommender-systems-ncf-vs-mf/) | [代码](https://github.com/google-research/google-research/tree/master/dot_vs_learned_similarity)





On Sampled Metrics for Item Recommendationhttps://dl.acm.org/doi/pdf/10.1145/3394486.3403226

推荐原因：KDD2020 best paper。这篇论文主要对抽样指标进行了详细的研究。在该项目中是使用依赖于相关项目位置的排名指标算法来进行评估，在任务中需要在给定的上下文情况下来对大量的项目进行排序。结果发现这些抽样指标与精确的度量值不一致，因为它们没有保留相关的语句。而研究者证明了一种可行的方法就是通过应用一个修正项，即最小化不同的标准，如偏差或均方误差，来提高抽样指标的性能。最后通过对原始抽样指标及其修正变量实证评估，研究者建议在度量计算中应避免抽样，但是如果实验研究需要抽样，那么他们所提出的修正项可以提高估计的质量。

对应summary：https://zhuanlan.zhihu.com/p/206823510





GLaRA: Graph-based Labeling Rule Augmentation for Weakly Supervised  Named Entity Recognition(https://arxiv.org/pdf/2104.06230.pdf)



【推荐理由】本文收录于EACL2021，文章提出了一种基于图的标注规则增强框架，该框架可以从未标注的数据中自动学习新的标注规则。
近年来，研究人员建议使用启发式标记规则来训练命名实体识别（NER）系统，而不是使用昂贵的手工注释。然而，但是，设计标签规则具有挑战性，因为它通常需要大量的人工和领域专业知识。为了缓解这个问题，本文提出了GLARA模型，一种基于图的自动标注框架，该框架可以从未标注的数据中学习新的标注规则。具体来说，首先创建一个图，图中的节点表示从未标记数据中抽取到的候选规则。然后，通过探索规则之间的语义关系，设计了一种新的图神经网络来扩展标注规则。最后将增强规则应用到未标记的数据中以产生弱标记，并使用这些弱标记数据训练NER模型。作者在三个NER数据集上评估了提出的方法，发现在给定了少量种子规则的情况下，与最佳基准相比，可以实现+20％的F1分数平均提高。





Planning with Entity Chains for Abstractive Summarization(https://arxiv.org/pdf/2104.07606.pdf)



【推荐理由】预先训练的基于transformer的序列到序列模型已经成为许多文本生成任务(包括摘要)的首选解决方案。然而，这些模型产生的结果往往包含重要的问题，如幻觉和无关的通道。缓解这些问题的一个解决方案是在神经摘要中加入更好的内容规划。我们建议使用实体链(即摘要中提到的实体链)来更好地规划和基础抽象摘要的生成。特别地，我们通过在目标前面加上它的实体链来扩充目标。我们对这个内容规划目标进行了预培训和微调。当在CNN/每日邮报、SAMSum和XSum上进行评估时，用这个目标训练的模型在实体的正确性和总结的简结性上得到了提高，在SAMSum和XSum的ROUGE上取得了最先进的性能。





TOP2VEC: DISTRIBUTED REPRESENTATIONS OF TOPICS(https://arxiv.org/pdf/2008.09470v1.pdf)



推荐原因：主题建模用于发现大量文档中的潜在语义结构，通常称为主题。最广泛使用的方法是潜在狄利克雷分配和概率潜在语义分析。尽管它们很受欢迎，但它们仍然有一些缺点。为了获得最佳结果，他们通常需要知道许多主题，自定义停用词列表，词干和词形限制。另外，这些方法依赖于忽略单词顺序和语义的文档的词袋表示。文档和单词的分布式表示形式由于其捕获单词和文档的语义的能力而受到欢迎。我们介绍了top2vec，它利用联合文档和单词语义嵌入来查找主题向量。此模型不需要停用词列表，词干或词形限制，它会自动查找主题数。生成的主题向量与文档和词向量共同嵌入，它们之间的距离表示语义相似性。作者的实验表明，与概率生成模型相比，top2vec所查找的主题具有更丰富的信息，并代表了受过训练的主体。

[相关tutorial和code地址](https://github.com/ddangelov/Top2Vec)





[Don’t Stop Pretraining: Adapt Language Models to Domains and Tasks](https://arxiv.org/pdf/2004.10964v3.pdf)



推荐原因：这篇论文研究了将预训练的模型定制为目标任务的领域是否仍然有帮助。主要包括两个部分，domain-adaptive pretraining和task-adaptive pretraining，作者在四个领域的八个分类任务上做了实验，证明了无论领域内数据多还是少，domain-adaptive pretraining都能提高性能。而且使用给定任务的未标注数据进行task-adaptive pretraining也能提高性能。

[summary](https://blog.csdn.net/SimonSmile/article/details/109027378) | [code](https://github.com/allenai/dont-stop-pretraining)





[An Unsupervised Neural Attention Model for Aspect Extraction](https://www.aclweb.org/anthology/P17-1036.pdf)



推荐原因：Aspect extraction，现有的方法使用多种主题模型topic models，但是这些方法不能产生一致的aspect(coherent aspect)。本文提出了一个novel neural approach（新颖的神经网络方法），为了发觉连贯的aspect(coherent aspect)。通过神经网络的word embeddings来获取词共现的分布（the distribution of word co-occurrences）。不像主题模型，只使用独立的词，word embedding模型可以使有相似上下文的词在embedding空间中的距离更近。除此之外，使用attention机制在训练的时候来削弱不相关词。本文的方法可以提取出更有意义和相关的aspect。

[summary](https://blog.csdn.net/u013695457/article/details/80390569) | [code](https://github.com/ruidan/Unsupervised-Aspect-Extraction)





[Confident Learning: Estimating Uncertainty in Dataset Labels](https://arxiv.org/pdf/1911.00068v5.pdf)



推荐原因：论文提出的置信学习（confident learning，CL）是一种新兴的、具有原则性的框架，以识别标签错误、表征标签噪声并应用于带噪学习（noisy label learning），优点在于：1.发现 “可能错误的样本” 【注：这个只能说是相对的，因为模型找出的置信度低的样本，并不一定就是错误样本，而只是一种不确定估计的选择方法】；2.可直接估计噪声标签与真实标签的联合分布，具有理论合理性。3.不需要超参数，只需使用交叉验证来获得样本外的预测概率。4.不需要做随机均匀的标签噪声的假设（这种假设在实践中通常不现实）。5.与模型无关，可以使用任意模型，不像众多带噪学习与模型和训练过程强耦合。

[summary](https://l7.curtisnorthcutt.com/cleanlab-python-package) | [code](https://github.com/cgnorthcutt/cleanlab)



[NLP中的深度学习技术概览](https://www.linkresearcher.com/information/6c7a15b5-236a-40f3-879f-af2ac06c2557)



[Transformer Transforms Salient Object Detection and Camouflaged Object Detection](https://arxiv.org/abs/2104.10127)



【推荐理由】:
表现SOTA！性能优于SCWS、JLDCF等网络，

源自机器翻译的Transformer网络特别擅长在长序列中对远程依存关系进行建模。当前，Transformer网络在从高级分类任务到low-level密集预测任务的各种视觉任务中都取得了革命性的进步。在本文中，我们进行了将Transformer网络用于显著性物体检测（SOD）的研究。具体来说，我们采用密集的Transformer主干来进行全面监督的基于RGB图像的SOD，基于RGB-D图像对的SOD，以及通过涂抹监督来进行弱监督的SOD。作为扩展，我们还将完全监督的模型应用于伪装对象分割的伪装对象检测（COD）任务。对于完全监督的模型，我们将密集Transformer主干定义为特征编码器，并设计一个非常简单的解码器以生成一个单通道显著性图（或COD任务的伪装图）。对于弱监督模型，由于在乱写注释中没有结构信息，因此我们首先采用最近提出的Gated-CRF损失对有效的成对关系进行建模，以进行准确的模型预测。然后，我们引入自监督的学习策略来推动模型产生尺度不变的预测，这对于弱监督模型和在小型训练数据集上训练的模型被证明是有效的。在各种SOD和COD任务（基于完全监督的RGB图像的SOD，基于监督的完全监督的RGB-D图像对，通过涂抹监控的弱监督的SOD和基于监督的RGB图像的完全监督的SOD）上的大量实验结果表明，Transformer网络可以转换显著性目标检测和伪装对象检测，从而为每个相关任务提供了新的基准。



[So-ViT: Mind Visual Tokens for Vision Transformer](https://arxiv.org/abs/2104.10935)



推荐理由】：本文提出一种二阶视觉Transformer新网络，表现SOTA！性能优于T2T-ViT、RepVGG等网络，代码刚刚开源！

最近，视觉Transformer（ViT）体系结构（其中的骨干网完全由自注意力机制组成）在视觉分类中已经取得了非常有希望的性能。但是，原始ViT的高性能在很大程度上取决于使用超大规模数据集进行的预训练，如果从头开始训练，它在ImageNet-1K上的性能将大大落后。本文通过仔细考虑视觉tokens的作用，努力解决这个问题。首先，对于分类head，现有的ViT仅利class token，而完全忽略了高级visual token固有的丰富语义信息。因此，我们提出了一种新的分类范式，其中视觉标记的二阶交叉协方差池与class token结合在一起以进行最终分类。同时，提出了一种快速的奇异值功率归一化方法，以改善二阶合并。其次，原始的ViT采用固定大小图像patches的naive embedding，缺乏对平移等方差和局部性进行建模的能力。为缓解此问题，我们开发了一种基于现成卷积的轻量级分层模块，用于visual token embedding。我们在ImageNet-1K上对提出的架构（我们称为So-ViT）进行了全面评估。结果表明，我们的模型经过从头训练后，性能优于竞争的ViT变体，并且与最新的CNN模型相当或更好。



[code](https://github.com/jiangtaoxie/So-ViT)





[数据挖掘与可视化教程](https://www.heywhale.com/mw/project/6078eac3a959c70017e9f765)

[往期推荐](https://shimo.im/sheets/irNQzQVsv9UkraDA/7QYQI)





Multilingual Denoising Pre-training for Neural Machine Translation(https://arxiv.org/pdf/2001.08210.pdf)



推荐原因：Facebook AI发布了mBART（multilingual BART），一种基于多语言seq2seq去噪自动编码器的方法，该方法在大规模单语言语料库上进行了预训练，可用于25种语言的机器翻译。这项工作遵循Lewis等人（BART，2019）的预训练方案，并研究了去噪预训练对韩文，日文和土耳其文等多种语言的影响。输入文本涉及对短语的掩盖和对句子的置换（加噪），学习了一种基于Transformer的模型跨多种语言重建文本。 完整的自回归模型仅训练一次，并且可以在任何语言对上进行微调而无需进行任何特定于任务或特定于语言的修改。 此外，解决了文档级和句子级的翻译问题。 除了表现出性能提升外，作者还声称该方法在低资源机器翻译方面效果很好。

[BART summary](https://wmathor.com/index.php/archives/1505/) | [mBART summary](https://cloud.tencent.com/developer/article/1740267) | [code](https://github.com/pytorch/fairseq/tree/master/examples/mbart)





Pre-training is a Hot Topic: Contextualized Document Embeddings Improve Topic Coherence(https://arxiv.org/pdf/2004.03974v1.pdf)

推荐原因：主题模型从文档中提取有意义的单词组，以便更好地理解数据。 但是，解决方案通常不够连贯，因此难以解释。 通过向模型添加更多上下文知识，可以提高一致性。 最近，神经主题模型已经变得可用，而基于BERT的表示通常进一步推动了神经模型的发展。 作者结合了预训练的表示形式和神经主题模型。 预训练的BERT句子嵌入确实比标准LDA或现有的神经主题模型支持生成更有意义和更连贯的主题。

[code](https://github.com/MilaNLProc/contextualized-topic-models)



[Tensor Graph Convolutional Networks for Text Classification](https://arxiv.org/pdf/2001.05313v1.pdf)

推荐原因：本文提出的TensorGCN首先从三个视角对语料构建了三张图，分别是Semantic-based graph、Syntactic-based graph、Sequential-based graph。这三张图的节点相同，边不同。基于语义的图首先对语料使用LSTM进行编码（使用文本分类标签进行训练），在每句话中对词之间计算余弦相似度，相似度高于阈值的时候两词计数一次；基于句法和序列的图也以类似的方法得到。最终，使用TensorGCN提供了一种有效的方法来协调和集成来自不同类型图的异构信息，提升了文本分类的效果。

[summary](https://blog.csdn.net/qq_36618444/article/details/104918860) | [code](https://github.com/THUMLP/TensorGCN)



[Interactive Path Reasoning on Graph for Conversational Recommendation](https://arxiv.org/pdf/2007.00194.pdf)



推荐原因：传统的推荐系统从过去的交互历史中估计用户对项目的偏好，因此受到获取细粒度和动态用户偏好的限制。会话推荐系统(CRS)使系统能够直接向用户询问他们对物品的偏好属性，从而为这些限制带来了革命性的变化。然而，现有的CRS方法并没有充分利用这一优势-它们只以相当隐含的方式使用属性反馈，例如更新潜在用户表示。在本文中，我们提出了转换路径推理(Conversational Path Reasoning, CPR)，这是一个通用的框架，它将会话推荐建模为图上的交互式路径推理问题。它通过跟随用户反馈遍历属性顶点，显式地利用用户偏好属性。通过利用图结构，CPR能够删除许多不相关的候选属性，从而获得更好的命中用户偏好属性的机会。为了演示CPR的工作原理，我们提出了一个简单而有效的实例化，命名为SCPR(SimpleCPR)。我们对多轮会话推荐场景进行了实证研究，这是迄今为止最现实的CRS场景，它考虑了多轮询问属性和推荐项目。通过在Yelp和LastFM两个数据集上的大量实验，我们验证了我们的SCPR的有效性，它的性能明显优于最先进的CRS方法EAR和CRM。特别地，属性越多，我们的方法就能获得越多的优势。

[summary](https://zhuanlan.zhihu.com/p/323376843)



[Zero-Shot Detection via Vision and Language Knowledge Distillation](https://arxiv.org/abs/2104.13921)



【推荐理由】本文提出用于Zero-shot目标检测的ViLD网络（视觉和语言知识蒸馏，Vision and Language knowledge Distillation），可将知识从预先训练的Zero-shot图像分类模型（例如CLIP）提取到两阶段检测器（例如Mask R-CNN）中！

【论文简介】通过训练对齐的图像和文本编码器，Zero-shot图像分类取得了可喜的进展。这项工作的目标是推进Zero-shot目标检测，该目标旨在检测没有边界框或mask注释的新颖目标。我们提出ViLD，这是一种通过视觉和语言知识蒸馏的训练方法。我们将知识从预先训练的Zero-shot图像分类模型（例如CLIP）提取到两阶段检测器（例如Mask R-CNN）中。我们的方法将检测器中的区域嵌入与预先训练的模型推断出的文本和图像嵌入对齐。我们使用文本嵌入作为检测分类器，这是通过将类别名称输入到预训练的文本编码器中而获得的。然后我们将区域嵌入和图像嵌入之间的距离最小化，该距离是通过将区域提议输入到预训练图像编码器中而获得的。在推理过程中，我们将新颖类别的文本嵌入到检测分类器中，以实现Zero-shot目标检测。我们通过将所有稀有类别视为新颖类别来对LVIS数据集的性能进行基准测试。ViLD使用Mask R-CNN（ResNet-50 FPN）获得16.1个Mask APr，用于Zero-shot检测，其性能比监督者高3.8。该模型可以直接传输到其他数据集，分别在PASCAL VOC，COCO和Objects365上达到72.2 AP50、36.6 AP和11.8 AP。





[Interpreting Graph Neural Networks For NLP With Differentiable Edge Masking](https://openreview.net/pdf?id=WznmQa42ZAx)



图数据的天然优势是为学习算法提供了丰富的结构化信息，节点之间邻接关系的设计成为了重要的先验信息和交互约束。然而，有一部分边上的消息是可以忽略的，论文首先提出方法在不影响模型预测效果的情况下，将图结构中冗余的边drop掉。通过分析剩余边上具有怎样的先验知识，实现对GNN的预测过程加以解释。



GNN 能够将结构归纳偏置（structural inductive biases） 整合到 NLP 模型中。然而，却鲜有工作对于这种结构偏置的原理加以解释，特别是在理解图结构的哪些部分有助于模型的预测方面。因此，本文介绍了一种事后（post-hoc）方法，来对 GNN 的预测加以解释，它能够识别出不必要的边。



给定一个训练过的GNN模型，本文通过学习一个简单的分类器，对于每一层中的每条边，预测那条边是否可以被丢弃。作者证明了这样的分类器的训练可以用完全可微分的方式，使用随机门，并通过L0范数促进稀疏性。此外，作者还进行了非常有意义的实验，将提出的技术作为归因方法，同时分析了两个 NLP 任务中的GNN模型——问题回答和语义角色标注，并提供了对这些模型中信息流的理解。实验结果表明，可以丢弃大量的边却不会影响到模型的性能，同时通过分析剩余的重要边来解释模型的预测过程



[Adversarial Attacks on Neural Networks for Graph Data]( https://arxiv.org/pdf/1805.07984.pdf)



推荐原因：该论文提出一个对属性图进行对抗扰动的原则，旨在欺骗当前最优的图深度学习模型，paper贡献如下：

1. 模型：该研究针对节点分类提出一个基于属性图的对抗攻击模型，引入了新的攻击类型，可明确区分攻击者和目标节点。这些攻击可以操纵图结构和节点特征，同时通过保持重要的数据特征（如度分布、特征共现）来确保改变不被发现。
2. 算法：该研究开发了一种高效算法 Nettack，基于线性化思路计算这些攻击。该方法实现了增量计算，并利用图的稀疏性进行快速执行。

[code](https://github.com/danielzuegner/nettack)



[COLD: Towards the Next Generation of Pre-Ranking System](https://arxiv.org/pdf/2007.16122.pdf)



推荐原因：大多数的推荐系统都遵从一种多阶段的级联结构，最为我们所熟知的就是YoutubeDNN论文中所提出的Match + Rank的两阶段结构，但在阿里的场景下，又增加了Pre-Ranking和ReRanking阶段.Pre-Ranking确实是比较少见的。其模型大小／精度介于Matching和Ranking之间。不过我感觉不需要太过纠结于是哪个阶段，或者当作Matching阶段，主要学习的论文的思路就好。

[summary](https://zhuanlan.zhihu.com/p/186320100)





[Learning Knowledge Bases with Parameters for Task-Oriented Dialogue Systems](https://arxiv.org/pdf/2009.13656v1.pdf)



推荐原因：面向任务的对话系统要么通过单独的对话状态跟踪(DST)和管理步骤实现模块化，要么是端到端可训练。在这两种情况下，知识库(KB)在满足用户请求方面起着至关重要的作用。模块化系统依赖DST与知识库交互，这在注释和推理时间方面是昂贵的。端到端系统直接使用知识库作为输入，但当知识库大于几百个条目时，它们无法进行扩展。

[code](https://github.com/HLTCHKUST/ke-dialogue)





[Advances in Variational Inference](https://arxiv.org/pdf/1711.05597.pdf)



推荐原因：变分推断（Variational Inference, VI）是贝叶斯近似推断方法中的一大类方法，将后验推断问题巧妙地转化为优化问题进行求解，相比另一大类方法马尔可夫链蒙特卡洛方法（Markov Chain Monte Carlo, MCMC），VI 具有更好的收敛性和可扩展性（scalability），更适合求解大规模近似推断问题。

[summary 1](https://zhuanlan.zhihu.com/p/88336614) | [如何简单易懂地理解变分推断(variational inference)](https://juejin.cn/post/6844904128758415373)



[Dynamic Fusion Network for Multi-Domain End-to-end Task-Oriented Dialog](https://arxiv.org/pdf/2004.11019v3.pdf)



推荐原因：对话在众多文本生成任务中属于比较难的一类，因为它需要识别说话人的语义、意图等多个方面，从而需要大量数据支撑模型效果。然而，领域内数据往往十分稀少，利用多领域的数据有望缓解对话系统的数据问题。本文提出DFNet，利用多个领域的数据提升对话系统的效果。DFNet可以根据领域之间的相似度动态调整不同领域数据对当前领域的贡献，进而在多领域对话和领域迁移上取得更好的效果。

[summary](https://zhuanlan.zhihu.com/p/340065169) | [code](https://github.com/LooperXX/DF-Net)



#阿里多兴趣发展思路
1、排序
[DIN: Deep Interest Network](https://blog.csdn.net/suspend2014/article/details/104377681) - 考虑用户行为序列权重
[DIEN: Deep Interest Evolution Network](https://blog.csdn.net/suspend2014/article/details/104396044) - 考虑用户行为序列顺序和权重
[DSIN: Deep Session Interest Network](https://blog.csdn.net/suspend2014/article/details/104417120) - 考虑用户行为序列分组、顺序、权重
[DMIN: Deep Multi-Interest Network](https://www.jianshu.com/p/69929b24bb37) - 考虑用户行为序列attention、多兴趣
2、召回
MIND：考虑用户行为序列多兴趣

[胶囊网络](https://www.sohu.com/a/226611009_633698) | [1](https://www.jianshu.com/p/83309cdb9326) | [11](https://zhuanlan.zhihu.com/p/76495890)





[A Closer Look at Deep Policy Gradients](https://arxiv.org/pdf/1811.02553v4.pdf)



推荐原因：尽管近些年policy gradient这一系列的算法在deep RL上得到广泛的拓展并且也在仿真环境中取得不错的效果，但是这类算法的可复现性很差，训练过程中对超参数的敏感度还是很高的（相比监督训练）。本文通过实验说明在实际的policy gradient算法训练过程中，实际的指标和理论优化的目标指标并不是完全变化一致：比如在训练过程中我们固定每次迭代的sample数用来估计梯度，但是发现在迭代次数增加之后，同样的样本量却会有更大梯度预估误差。这篇文章很好地展示了理论优化和实际训练过程中的gap。

[summary](https://zhuanlan.zhihu.com/p/360344452)





BERT系列

1. [demo](https://www.jianshu.com/p/3d0bb34c488a)
2. [源码解读之模型主体](https://www.jianshu.com/p/d7ce41b58801)
3. [源码解读之Pre-train](https://www.jianshu.com/p/22e462f01d8c)
4. [源码解读之Fine-tune](https://www.jianshu.com/p/116bfdb9119a)
5. [BERT命名实体识别](https://github.com/macanv/BERT-BiLSTM-CRF-NER)
6.  [BERT + DSSM](https://github.com/InsaneLife/dssm)
7.  [ALBERT](https://zhuanlan.zhihu.com/p/105499010)
8. [XLNET](https://blog.csdn.net/u012526436/article/details/93196139)

