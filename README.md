恶意加密流量检测研究进展
关键字：恶意加密流量 识别 检测 研究进展

1、	引言
恶意流量检测是当前的热点问题。随着移动互联网的普及和发展，网络流量的规模急遽上升，网民人数也在飞速增长中。随之而来的问题是，网络中加密流量和代理流量的比例越来越高。但恶意攻击利用加密技术隐蔽躲藏，Zscaler在2017年下半年的SSL威胁报告中表示，滥用SSL和TLS的网络钓鱼和恶意软件攻击事件显著增加。如何通过不解密的方法快速地进行网络流量的检测，并并更好的识别网络空间中的代理流量，保持较好的检测能力，成为一大重要课题。
现在大概有四中常用的方法：基于端口号、深度包检测、机器学习和深度学习的相关方法。原有基于IAIA体系为应用和协议进行了固定的端口号的分配，设计了基于端口号的流量分类方法。但现如今，当前的大部分应用都采用了动态端口分配这一方法。另外，一些网络服务，如隧道和匿名，隐藏了真实的端口号信息，后又提出了基于DPI（深度包检测）的流量分类方法，这种方法也无法适用于现如今tls加密下的场景，同时也大大增加了系统的存储开销，背离了网络安全保护的初衷。同时，在网络加密流量占比不多，同时 TLS协议未被提出时，前两种常见流量识别技术基本可以保证网络安全；TLS 被提出后，流量加密技术逐渐成熟，需要进一步使用机器学习和深度学习的相关方法做出检测。所在png【待补充】已有了工作及其关键描述，接下来第二部分对各项工作进行详细介绍。
同时，针对分类模型的更新存在一些问题。例如，只在新的流量上训练新的分类器将导致一些历史知识丢失，从而影响到相应的准确率和精确率；面对数据不平衡的现象，会出现数据偏移的现象，从而使得检测结果进一步偏向多数类……为了解决这些问题，由此提出了增量学习的相关方法，接下来对第三部分进行详细介绍。



2、	加密恶意流量检测方法
由于基于端口号的识别方法已经完全不适用于现今的环境，所以不做具体展开。
DPI（深度包检测）技术是在传统IP数据包检测技术之上增加了对应用层数据的应用协议识别。文献[35]将DPI技术分类为：基于特征字的识别技术、应用层网关识别技术和行为模式识别技术。但DPI 技术依旧存在解析加密流量不完全，网络性能延迟，设备迭代困难，可视化不足等问题。文 献[36]和文献[37]提出对加密流量进行深度包检测(DPI) 而无需解密的技术，但在设置阶段需要大量的计算和 较长的检测时间。文献[38]提出基于 DPI 检查负载随机 性的加密流量识别算法，但涉及的流量仅仅包含 TCP,SFTP,HTTP,SMTP和SSL协议。
而在加密恶意流量发展阶段，主要采用的是机器学习的方式。不同类型的流量具备不同的网络行为模式，这些信息直观地体现在其数据包上。例如，恶意流量与良性流量在进行握手协议时会产生不同的数据包头部信息，而不同类型的恶意流量在其平均包长、包间间隔等方 面也存在差异；这些行为模式的差异是识别恶意流量的重要依据。因此可从采集到的加密流量中提取恶意 流量的行为模式，将其进一步归纳为恶意流量的特征，并利用机器学习模型进行识别。思科Deciphering malware's use of TLS (without decryption)首先提出了可以在不解密的情况下识别加密恶意流量，思科基于tls流特征的方法来进行加密流量识别，在不解密流量的情况下也取得了不错的进展。识别出加密流量中潜藏的安全威胁具有很大挑战，现已存在一些检测方法利用数据流的元数据来进行检测，包括包长度和到达间隔时间等。不需要对加密的恶意流量进行解密，就能检测到采用TLS连接的恶意程序。首先，分析正常流量和恶意流量中TLS流、DNS流和HTTP流的不同之处，具体包括未加密的TLS握手信息、TLS流中与目的IP地址相关的DNS响应信息、相同源IP地址5min窗口内HTTP流的头部信息；然后，选取具有明显区分度的特征集作为分类器（有监督机器学习）的输入来训练检测模型，从而识别加密的恶意流量，具有一定的创新性，也为后面的工作铺平了道路。
Dongeon提出将TLS加密流的元数据进行可视化，以作为恶意流量分析和分类的更好方法。提出了粗粒度的方法，称为TLS加密流可视化。JIAYONG提出了一种基于距离的方法，利用无监督学习算法高斯混合模型（GMM）和排序点来识别聚类结构，用以计算恶意软件之间的distance参数。利用此参数定义名为FClass的新恶意软件类。用新算法XGBoost训练模型与其他的四种方法进行比较评估，证明为最佳。LD Chou利用TensorFlow平台，设计了一个基于TensorFlow的深度学习模型框架来对恶意流量进行分类。文献[60]对机器学习训练过程所需实验各项评估参数做出了归纳，文献[61]介绍了机器学习的详细过程。 但对于高度伪装的指挥控制(C&C)通信，单纯基于统计特征或TLS握手特征的传统分类器检测能力逐渐下降。在这种情况下，探索其他维度的特征进行多维特征融合的方式更具有针对性。
恶意加密流量识别研究主要集中于二分类，由于应用程序和版本的多样性，实现加密恶意流量精细化识别仍存在一定的难度。类似于1D-CNN的诸多深度学习模型致力于拓宽加密流量的种类。难度在于：由于不同类型的流量有不同种类的数据包，格式和特征选取的差异性进一步加大了研究的难度；其次，目前存在的公开数据集不够均衡，会对模型的训练产生一定的差异性影响，检测结果多偏向于多数类。如目前较为新颖的 BERT 模型，在 解决 Transformer 模型需要训练大量的参数基础上，通过上下文全向实现自然语言文本的更精准识别处理。 想要将 BERT 模型应用到本领域，还存在着下面的问题：如何高效准确地将 TLS 加密流量转换成如图像， 自然语言处理文本，甚至语音进行处理。将胶囊神经 网络（Capsule Network），对抗神经网络（Generative Adversarial Networks, GAN）等模型应用到加密恶意流 量识别中，如：在胶囊网络中可以通过将获取的 TLS 数据集（.PCAP 数据包等）转化为图像特征，并作为模型的原始数据输入进行训练，这些低层胶囊对其输入执行一些相当复杂的内部计算，然后将这些计算的结果封装成一个包含丰富信息的小向量；再如设计动机为自动化特征提取的GAN 网络，利用 GAN 网络生成器，可以初步解决因为恶意流量少而导致的数据不平衡问题，并利用判别器迭代优化数据，以此有效提高自学习特征的可解释性和检测效率。

3、	加密恶意流量检测中不足的改进方法
在基于加密恶意流量检测的二分类问题中，若要解决数据更新对原有模型的负面影响，即新有的数据集对模型的更新会使得模型对原有数据的检测能力的下降，需要引入增量学习的方法。增量学习的能力就是能够不断地处理现实世界中连续的信息流，在吸收新知识的同时保留甚至整合、优化旧知识的能力。在机器学习领域，增量学习致力于解决模型训练的一个普遍缺陷：灾难性遗忘，也就是说，一般的机器学习模型(尤其是基于反向传播的深度学习方法)在新任务上训练时，在旧任务上的表现通常会显著下降。造成灾难性遗忘的一个主要原因是“传统模型假设数据分布是固定或平稳的，训练样本是独立同分布的”。而在加密恶意流量的二分类任务中，要解决的问题在于如何改进模型泛化性能。可以使用三种方法解决上述问题，分别是基于sklearn、基于lightGBM和基于keras的增量训练方法。目前已经取得不错的检测效果。
而对于更加复杂的类增量学习任务，要解决的问题在于如何让模型认识更多的类。增量学习方法的种类有很多种划分方式，将其划分为以下三种范式：正则化(regularization)、回放(replay)和参数隔离(parameter isolation)。

4、	总结与展望
机器学习领域对抗样本的飞速进展使得流量混淆与伪装的难度进一步降低，攻击者可通过学习流量数据，添加合适的噪声将攻击流量伪装成正常流量，甚至误导模型将正常流量识别为攻击流量；另外，基于机器学习的恶意流量识别能力会随着时间流逝而逐渐下降，同时在训练／测试集中的恶意流量分布必须符合现实分布。因此，在确保数据集有效丰富的前提下，可以预见，融合其他领域技术以及运用深度学习方法将会使检测模型解决流量检测的准确性能实时保持稳定的上升。


---
具体方案、关键技术、解决效果：
1. 该网络将输入数据映射到低维度的空间，使得是被细粒度簇变得容易；
2. 该方法优于现有的半监督和无监督学习的方法，因为原有的方法都会假设标签完整，而本文克服了这一缺点；同时计算性能开销控制在与前述方法一致。
创新点：
1.  遵循的方法论可分为三点，使用无监督学习结果增强标签，输入转换并进行最终聚类。
2.  优点在于，fare结合了现有的标签由此带来了表现更好，其尝试把数据映射到低纬度空间，降低了特征处理的复杂程度，且定义了聚类距离的方法。
不足与借鉴：
1）论文不足：选择广泛使用的聚类算法，如DBSCAN会使得系统计算达到瓶颈而达不到假设的预期效果；未考虑数据极度不平衡且巨大的挑战；
2）思考与借鉴：通过已知的高质量的数据训练模型来加入低质量标记的数据，是半监督学习，对于识别未知类具有很好的借鉴意义。

