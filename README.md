#### [《D(R,O) Grasp: A Unified Representation of Robot and Object Interaction for Cross-Embodiment Dexterous Grasping》](https://arxiv.org/abs/2410.01702) 论文阅读笔记
<br><br><br><br><br>

# 正向运动学

在一般实例中，机器人手的某两段相邻连杆（即指节）的铰接处就是关节。例如，拇指第一段指节与第二段指节的铰接处，就构成了一个关节。<br>
设机器人手的关节数为 ![](https://latex.codecogs.com/svg.latex?N_{\text{joint}}) ，那么各关节的角度 ![](https://latex.codecogs.com/svg.latex?\theta_{\text{joint}_1},\theta_{\text{joint}_2},\cdots,\theta_{\text{joint}_{N_{\text{joint}}}}\in\mathbb{R})（弧度制）所组成的实数向量，可以表示为 ![](https://latex.codecogs.com/svg.latex?q\in\mathbb{R}^{N_{\text{joint}}}) 。<br>
设机器人手的自由度为 ![](https://latex.codecogs.com/svg.latex?N_{\text{DoF}}) 。在一般实例中，机器人手的一个关节拥有一个自由度，此时 ![](https://latex.codecogs.com/svg.latex?N_{\text{joint}}=N_{\text{DoF}}) ，于是 ![](https://latex.codecogs.com/svg.latex?q\in\mathbb{R}^{N_{\text{DoF}}}) 。

## 1、机器人手的抓取构型 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{A}\in\mathbb{R}^{N_{\text{DoF}}})

从成功抓取数据集中随机采样一个 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{A}\in\mathbb{R}^{N_{\text{DoF}}}) ，它表征了在某个成功的机器人手抓取物体场景下，机器人手的关节角度向量，或称抓取构型。<br>
设机器人手的连杆数为 ![](https://latex.codecogs.com/svg.latex?N_\ell) 。在该机器人手的每一段连杆的表面上，均匀地采样一些点。这些点的坐标是相对于连杆本身的某个点（例如连杆某一端的端点，作为连杆相对坐标系的原点）而言的，并非相对于地面的某个固定点而言（即并非绝对坐标）。所有采样点组成的点云，可以表示为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}^\mathcal{A}\}_{i=1}^{N_\ell}.">
</p>

如果在每一段连杆的表面上同时采样 ![](https://latex.codecogs.com/svg.latex?n) 个点，那么采样点云所包含的点的数量就是 ![](https://latex.codecogs.com/svg.latex?n\times{}N_\ell) 。<br>
定义点云正向运动学函数 ![](https://latex.codecogs.com/svg.latex?\text{FK}\left(\cdot,\cdot\right)) ，它接受抓取构型 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{A}\in\mathbb{R}^{N_{\text{DoF}}}) 以及采样点云

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}^\mathcal{A}\}_{i=1}^{N_\ell}">
</p>

作为输入，输出绝对坐标点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{A}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{A}=\text{FK}\left(q_\mathcal{A},\{\mathbf{P}_{\ell_i}^\mathcal{A}\}_{i=1}^{N_\ell}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}3},">
</p>

其中 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{R}) 在原论文中设定为 ![](https://latex.codecogs.com/svg.latex?512)（这意味着 ![](https://latex.codecogs.com/svg.latex?n\times{}N_\ell=512) ）。<br>
绝对坐标点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{A}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 是一个 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{R}) 行 ![](https://latex.codecogs.com/svg.latex?3) 列的矩阵，其每一行表征了在某个成功抓取场景下，机器人手表面某个采样点的 ![](https://latex.codecogs.com/svg.latex?\left(x,y,z\right)) 三维绝对坐标（相对于地面上的某个固定点而言）。

## 2、机器人手的张开构型（标准构型）![](https://latex.codecogs.com/svg.latex?q_\mathcal{B}\in\mathbb{R}^{N_{\text{DoF}}})

计算出与采样抓取构型 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{A}\in\mathbb{R}^{N_{\text{DoF}}}) 对应的张开构型，即 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{B}\in\mathbb{R}^{N_{\text{DoF}}}) 。机器人手在这两种构型下具有相似的腕部位姿。机器人手在张开构型下，展示着机器人手尚未执行物体抓取任务时的标准机械结构，因此张开构型也称标准构型。<br>
与抓取构型同理，在张开构型下，对机器人手表面进行采样。为了让采样点在两种构型下保持同一性，张开构型下机器人手连杆上采样点的相对坐标，应当等同于抓取构型下机器人手同一连杆上采样点的相对坐标。<br>
![](https://latex.codecogs.com/svg.latex?\text{FK}\left(\cdot,\cdot\right)) 函数接受张开构型 ![](https://latex.codecogs.com/svg.latex?q_\mathcal{B}\in\mathbb{R}^{N_{\text{DoF}}}) 以及采样点云

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}^\mathcal{B}\}_{i=1}^{N_\ell}">
</p>

作为输入，输出绝对坐标点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{B}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{B}=\text{FK}\left(q_\mathcal{B},\{\mathbf{P}_{\ell_i}^\mathcal{B}\}_{i=1}^{N_\ell}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}3}.">
</p>

绝对坐标点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{B}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 是一个 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{R}) 行 ![](https://latex.codecogs.com/svg.latex?3) 列的矩阵，其每一行表征了在相似腕部位姿的机器人手张开构型下，机器人手表面某个采样点的 ![](https://latex.codecogs.com/svg.latex?\left(x,y,z\right)) 三维绝对坐标。
<br><br><br><br><br><br><br><br><br><br>

# 构型无关化预训练

## 1、点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{R}}(\cdot))

引入点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{R}}(\cdot)) ，它将对两种构型下的绝对坐标点云分别进行特征提取。具体而言，它接受 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{A}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 作为输入，输出点级特征矩阵 ![](https://latex.codecogs.com/svg.latex?\phi^\mathcal{A}\in\mathbb{R}^{N_\mathcal{R}\times{}D}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\phi^\mathcal{A}=f_{\theta_\mathcal{R}}\left(\mathbf{P}^\mathcal{A}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}D},">
</p>

也接受 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{B}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 作为输入，输出点级特征矩阵 ![](https://latex.codecogs.com/svg.latex?\phi^\mathcal{B}\in\mathbb{R}^{N_\mathcal{R}\times{}D}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\phi^\mathcal{B}=f_{\theta_\mathcal{R}}\left(\mathbf{P}^\mathcal{B}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}D},">
</p>

其中 ![](https://latex.codecogs.com/svg.latex?D=512) 为特征维度数。

## 2、机器人并非理所当然清楚自己的手是长什么样的

一个基本的考虑在于，机器人手上的同一点所对应的特征向量的方向（排除光照强度等无关因素干扰），在机器人手的不同构型下应该保持基本一致。否则，机器人对自己的手的感知就是虚假的，机器人其实根本不清楚自己的手是长什么样的。此时机器人手对机器人神经网络而言只是一个属性未知的外部实体，机器人根本不知道如何使用自己的手，机器人运用自己的手进行物体抓取的任务也就失去了实现的基础。<br>
对于机器人手表面的某个采样点，可用 ![](https://latex.codecogs.com/svg.latex?i) 表示它在两个点云中的同一索引。对于同一个或另一个采样点，可用 ![](https://latex.codecogs.com/svg.latex?j) 表示它在两个点云中的同一索引。如果是同一个采样点则 ![](https://latex.codecogs.com/svg.latex?i=j) ，如果是另一个采样点则 ![](https://latex.codecogs.com/svg.latex?i\neq{}j) 。

## 3、对比损失函数 ![](https://latex.codecogs.com/svg.latex?\mathcal{L}_p)

定义对比损失函数

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathcal{L}_p=-\frac{1}{N_\ell}\sum_i\log\left[\frac{\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)}{\sum_j\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)}\right],">
</p>

其中

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\omega_{ij}=\begin{cases}1,&\text{if}\;\,i=j\\{}\frac{\tanh\left(\lambda\left\lVert{}p_i^\mathcal{B}-p_j^\mathcal{B}\right\rVert_2\right)}{\tanh\left(\max\left(\lambda\left\lVert{}p_i^\mathcal{B}-p_j^\mathcal{B}\right\rVert_2\right)\right)},&\text{if}\;\,i\neq{}j\end{cases}.">
</p>

![](https://latex.codecogs.com/svg.latex?\left\langle\cdot,\cdot\right\rangle) 表示两个向量之间的余弦相似度，![](https://latex.codecogs.com/svg.latex?p_i^\mathcal{B}\in\mathbb{R}^3) 表示 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{B}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 中第 ![](https://latex.codecogs.com/svg.latex?i) 个点的坐标。在原论文中，设定 ![](https://latex.codecogs.com/svg.latex?\tau=0.1,\lambda=10) 。

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)">
</p>

计算的是点 ![](https://latex.codecogs.com/svg.latex?i) 在抓取构型下的特征向量 ![](https://latex.codecogs.com/svg.latex?\phi_i^\mathcal{A}\in\mathbb{R}^D) 与点 ![](https://latex.codecogs.com/svg.latex?j) 在张开构型下的特征向量 ![](https://latex.codecogs.com/svg.latex?\phi_j^\mathcal{B}\in\mathbb{R}^D) 的距离加权相似度。

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\frac{\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)}{\sum_j\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)}\in\left(0,1\right)">
</p>

计算的是

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\omega_{ii}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)=\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)">
</p>

在

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\omega_{i1}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_1^\mathcal{B}\right\rangle/\tau\right),\omega_{i2}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_2^\mathcal{B}\right\rangle/\tau\right),\cdots,\omega_{ii}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)=\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right),\cdots,\omega_{iN_\mathcal{R}}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_{N_\mathcal{R}}^\mathcal{B}\right\rangle/\tau\right)">
</p>

当中所占的比例。为了让点 ![](https://latex.codecogs.com/svg.latex?i) 所对应的特征向量的方向在机器人手的抓取、张开构型下保持基本一致，点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{R}}(\cdot)) 需要让该比例（本质上就是模型所估计的似然）尽可能变大。<br>
于是，可以将该比例的负对数（即负对数似然）用作一个损失项，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathcal{L}_{p_i}=-\log\left[\frac{\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)}{\sum_j\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)}\right],">
</p>

迫使点云编码器降低该损失项的值的同时，就实现了原比例的增长。<br>
损失项 ![](https://latex.codecogs.com/svg.latex?\mathcal{L}_{p_i}) 是针对于单个点 ![](https://latex.codecogs.com/svg.latex?i) 而言的。将所有点 ![](https://latex.codecogs.com/svg.latex?1,2,\cdots,N_\mathcal{R}) 各自对应的损失项相加，并除以机器人手连杆数 ![](https://latex.codecogs.com/svg.latex?N_\ell) ，就得到了连杆级平均损失（注意并非除以 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{R}) ，并非点级平均损失），即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\mathcal{L}_p&=\frac{1}{N_\ell}\sum_i\mathcal{L}_{p_i}\\&=\frac{1}{N_\ell}\sum_i\left[-\log\left[\frac{\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)}{\sum_j\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)}\right]\right]\\&=-\frac{1}{N_\ell}\sum_i\log\left[\frac{\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)}{\sum_j\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)}\right].\end{align*}">
</p>

## 4、距离权重 ![](https://latex.codecogs.com/svg.latex?\omega_{ij})

对于两个不同的点 ![](https://latex.codecogs.com/svg.latex?i) 和 ![](https://latex.codecogs.com/svg.latex?j) ，在标准构型下，如果它们距离较近，那么它们会具有相似的几何邻域，进而意味着相似的语义信息。因此，对于相距较近的两点的特征向量，它们的方向本就应该是接近的，它们的余弦相似度本就应该较大。这种“近距近特征”的自然性质不应该遭受严苛的梯度惩罚。然而，余弦相似度 ![](https://latex.codecogs.com/svg.latex?\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle) 出现在似然比例的分母中，这意味着当它增大时，![](https://latex.codecogs.com/svg.latex?\mathcal{L}_p) 会增大，进而招致梯度惩罚压力，限制其增大。<br>
为了适度缓解这种梯度惩罚压力，引入距离权重

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\omega_{ij}=\frac{\tanh\left(\lambda\left\lVert{}p_i^\mathcal{B}-p_j^\mathcal{B}\right\rVert_2\right)}{\tanh\left(\max\left(\lambda\left\lVert{}p_i^\mathcal{B}-p_j^\mathcal{B}\right\rVert_2\right)\right)}\in\left(0,1\right),">
</p>

此处 ![](https://latex.codecogs.com/svg.latex?i\neq{}j) 。当点 ![](https://latex.codecogs.com/svg.latex?i) 与点 ![](https://latex.codecogs.com/svg.latex?j) 相距较近时，![](https://latex.codecogs.com/svg.latex?\omega_{ij}) 较小，与 ![](https://latex.codecogs.com/svg.latex?\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)) 相乘后，乘积结果 ![](https://latex.codecogs.com/svg.latex?\omega_{ij}\exp\left(\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle/\tau\right)) 作为新的分母项，使分母整体减小，进而使 ![](https://latex.codecogs.com/svg.latex?\mathcal{L}_p) 减小，缓解了限制 ![](https://latex.codecogs.com/svg.latex?\left\langle\phi_i^\mathcal{A},\phi_j^\mathcal{B}\right\rangle) 增大的梯度惩罚压力。<br>
![](https://latex.codecogs.com/svg.latex?\exp\left(\left\langle\phi_i^\mathcal{A},\phi_i^\mathcal{B}\right\rangle/\tau\right)) 本身既作为分子，又需要直接作为分母中的项，维持似然比例恒不大于 ![](https://latex.codecogs.com/svg.latex?1) 的数学性质。倘若将其与一个小于 ![](https://latex.codecogs.com/svg.latex?1) 的因子相乘，那么就会存在分母小于分子（比例大于 ![](https://latex.codecogs.com/svg.latex?1) ）的可能，进而存在 ![](https://latex.codecogs.com/svg.latex?\mathcal{L}_p<0) 的可能，整个模型的预训练直接朝着错误方向崩溃。因此，当 ![](https://latex.codecogs.com/svg.latex?i=j) 时，![](https://latex.codecogs.com/svg.latex?\omega_{ij}) 必须设置为 ![](https://latex.codecogs.com/svg.latex?1) 。
<br><br><br><br><br><br><br><br><br><br>

# 未来抓取位姿下的点对点预测距离

给定机器人手腕部的位姿（随机生成或者用户指定），可以唯一确定对应的初始张开构型，记为 ![](https://latex.codecogs.com/svg.latex?q_\text{init}\in\mathbb{R}^{N_\text{DoF}}) 。在机器人手每一段连杆的表面上均匀地采样，得到采样点云

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}\}_{i=1}^{N_\ell}.">
</p>

绝对坐标点云即为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{R}=\text{FK}\left(q_\text{init},\{\mathbf{P}_{\ell_i}\}_{i=1}^{N_\ell}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}3},">
</p>

后文中将其称为机器人手点云。<br>
待抓取物体的点云记为 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) ，其中 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{O}) 在原论文中设定为 ![](https://latex.codecogs.com/svg.latex?512) 。后文中将其称为物体点云。<br>
机器人手点云与物体点云在同一个原点坐标系中。接下来，神经网络需要预测出：在未来的抓取构型下，机器人手点云中任意一点与物体点云中任意一点的距离。<br>
这 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{R}\times{}N_\mathcal{O}) 个距离预测值由点对点预测距离矩阵 ![](https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}}) 表征。

## 1、提取点级特征

引入机器人手点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{R}}(\cdot)) 以及物体点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{O}}(\cdot)) ，它们架构相同，参数独立。其中，机器人手点云编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{R}}(\cdot)) 用构型无关化预训练参数进行初始化，并在训练过程中保持冻结。<br>

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\phi^\mathcal{R}&=f_{\theta_\mathcal{R}}\left(\mathbf{P}^\mathcal{R}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}D},\\{}\phi^\mathcal{O}&=f_{\theta_\mathcal{O}}\left(\mathbf{P}^\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{O}\times{}D},\end{align*}">
</p>

其中，![](https://latex.codecogs.com/svg.latex?\phi^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}D}) 对机器人手点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 的语义信息进行了编码（提取了每个点的特征向量），![](https://latex.codecogs.com/svg.latex?\phi^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}D}) 对物体点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 的语义信息进行了编码。<br>
然而，这两个点级特征矩阵目前是孤立的，并未编码自己与对方的关系信息。为了让点级特征矩阵所编码的信息更加完备，从而支撑起手-物交互场景下的可靠预测，遂引入机器人-物体交叉注意力函数 ![](https://latex.codecogs.com/svg.latex?g_{\theta_\mathcal{R}}\left(\cdot,\cdot\right)) 以及物体-机器人交叉注意力函数 ![](https://latex.codecogs.com/svg.latex?g_{\theta_\mathcal{O}}\left(\cdot,\cdot\right)) 。
