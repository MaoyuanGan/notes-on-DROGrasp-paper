#### [《D(R,O) Grasp: A Unified Representation of Robot and Object Interaction for Cross-Embodiment Dexterous Grasping》](https://arxiv.org/abs/2410.01702) 论文精读与思考笔记
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
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\boldsymbol{\phi}^\mathcal{R}&=f_{\theta_\mathcal{R}}\left(\mathbf{P}^\mathcal{R}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}D},\\{}\boldsymbol{\phi}^\mathcal{O}&=f_{\theta_\mathcal{O}}\left(\mathbf{P}^\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{O}\times{}D},\end{align*}">
</p>

其中，![](https://latex.codecogs.com/svg.latex?\boldsymbol{\phi}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}D}) 对机器人手点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 的语义信息进行了编码（提取了每个点的特征向量），![](https://latex.codecogs.com/svg.latex?\boldsymbol{\phi}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}D}) 对物体点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 的语义信息进行了编码。<br>
然而，这两个点级特征矩阵目前是孤立的，并未编码自己与对方的关系信息。为了让点级特征矩阵所编码的信息更加完备，从而支撑起手-物交互场景下的可靠预测，遂引入机器人-物体交叉注意力函数 ![](https://latex.codecogs.com/svg.latex?g_{\theta_\mathcal{R}}\left(\cdot,\cdot\right)) 以及物体-机器人交叉注意力函数 ![](https://latex.codecogs.com/svg.latex?g_{\theta_\mathcal{O}}\left(\cdot,\cdot\right)) 。

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\boldsymbol{\psi}^\mathcal{R}&=g_{\theta_\mathcal{R}}\left(\boldsymbol{\phi}^\mathcal{R},\boldsymbol{\phi}^\mathcal{O}\right)+\boldsymbol{\phi}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}D},\\{}\boldsymbol{\psi}^\mathcal{O}&=g_{\theta_\mathcal{O}}\left(\boldsymbol{\phi}^\mathcal{O},\boldsymbol{\phi}^\mathcal{R}\right)+\boldsymbol{\phi}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}D},\end{align*}">
</p>

其中，![](https://latex.codecogs.com/svg.latex?\boldsymbol{\psi}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}D}) 在充分保留机器人手点云语义信息的基础上，额外编码了机器人手相对于物体的关系信息。例如，“机器人手在物体正上方”这种相对方位信息。<br>
而 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{\psi}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}D}) 在充分保留物体点云语义信息的基础上，额外编码了物体相对于机器人手的关系信息。例如，“物体近似球形，需要用手充分包络才能抓取”这种可供性信息。<br>

## 2、抓取方式潜变量 ![](https://latex.codecogs.com/svg.latex?z\in\mathbb{R}^d)

为了赋予网络跨本体化、多样化抓取操作的控制能力，让网络能够捕捉到手、物体、抓取构型之间无数种组合（可理解为“抓取方式”）所蕴藏的内在规律，防止其沦为只能复刻训练数据场景的确定性映射网络，就有必要在网络中引入随机性成分。<br>
具体而言，仅在训练期间引入条件变分自编码器 ![](https://latex.codecogs.com/svg.latex?f_{\theta_\mathcal{G}}\left(\cdot,\cdot,\cdot\right)) ，它接受抓取全局点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{G}\in\mathbb{R}^{\left(N_\mathcal{R}+N_\mathcal{O}\right)\times{}3})（由 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 与 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 沿行方向拼接而成）以及点级特征矩阵 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{\psi}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}D},\boldsymbol{\psi}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}D}) 作为输入，输出均值向量 ![](https://latex.codecogs.com/svg.latex?\mu\in\mathbb{R}^d) 以及协方差矩阵 ![](https://latex.codecogs.com/svg.latex?\Sigma\in\mathbb{R}^{d\times{}d}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\left(\mu,\Sigma\right)=f_{\theta_\mathcal{G}}\left(\mathbf{P}^\mathcal{G},\boldsymbol{\psi}^\mathcal{R},\boldsymbol{\psi}^\mathcal{O}\right).">
</p>

![](https://latex.codecogs.com/svg.latex?\mu\in\mathbb{R}^d) 与 ![](https://latex.codecogs.com/svg.latex?\Sigma\in\mathbb{R}^{d\times{}d}) 作为参数，构成一个高斯分布。在当前的抓取场景下，![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{G},\boldsymbol{\psi}^\mathcal{R},\boldsymbol{\psi}^\mathcal{O}) 代表了“前提条件”，而 ![](https://latex.codecogs.com/svg.latex?\mu\in\mathbb{R}^d,\Sigma\in\mathbb{R}^{d\times{}d}) 所构成的高斯分布，则对以下这种信息进行了编码：<br>
在 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{G},\boldsymbol{\psi}^\mathcal{R},\boldsymbol{\psi}^\mathcal{O}) 的前提下，所有可能的“抓取方式”，以及它们背后所共同蕴藏的、反映当前特定场景实际情况的“物理约束”。<br>
从该分布中随机采样，得到一个 ![](https://latex.codecogs.com/svg.latex?d) 维的向量，记为潜变量 ![](https://latex.codecogs.com/svg.latex?z\in\mathbb{R}^d) 。潜变量 ![](https://latex.codecogs.com/svg.latex?z\in\mathbb{R}^d) 直接编码了从“抓取方式候选池”中任意选择的某一个抓取方式。<br>
将 ![](https://latex.codecogs.com/svg.latex?z\in\mathbb{R}^d) 拼接到每个点的特征向量（ ![](https://latex.codecogs.com/svg.latex?\boldsymbol{\psi}_i^\mathcal{R}\in\mathbb{R}^D) 或 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{\psi}_j^\mathcal{O}\in\mathbb{R}^D) ）的末尾，得到更长的特征向量（ ![](https://latex.codecogs.com/svg.latex?\widehat{\boldsymbol{\psi}}_i^\mathcal{R}\in\mathbb{R}^{D+d}) 或 ![](https://latex.codecogs.com/svg.latex?\widehat{\boldsymbol{\psi}}_j^\mathcal{O}\in\mathbb{R}^{D+d}) ）。这个更长的特征向量就在完整保留原有信息的基础上，额外携带了抓取方式信息。<br>
在原论文中，![](https://latex.codecogs.com/svg.latex?d) 设定为 ![](https://latex.codecogs.com/svg.latex?64) 。

## 3、点对点预测距离矩阵 ![](https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}})

应注意，在未来的抓取构型下，机器人手点云相较当前已经发生了变化，不再是 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{R}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) ，而是与之不同的 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 。后文将 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 称为机器人手预测点云。与此同时，物体在被实际抓取前保持固定，其点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 也就并不会发生变化。<br>
机器人手预测点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 中的任意一点 ![](https://latex.codecogs.com/svg.latex?p_i^{\mathcal{R}^\prime}\in\mathbb{R}^{3}) ，与物体点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 中的任意一点 ![](https://latex.codecogs.com/svg.latex?p_j^{\mathcal{O}}\in\mathbb{R}^{3}) 的距离，被预测为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?r_{ij}=\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_i^\mathcal{R},\widehat{\boldsymbol{\psi}}_j^\mathcal{O}\right)=\sigma\left(\frac{1}{2}\mathcal{N}_\theta\left(\widehat{\boldsymbol{\psi}}_i^\mathcal{R},\widehat{\boldsymbol{\psi}}_j^\mathcal{O}\right)+\frac{1}{2}\mathcal{N}_\theta\left(\widehat{\boldsymbol{\psi}}_j^\mathcal{O},\widehat{\boldsymbol{\psi}}_i^\mathcal{R}\right)\right)\in\mathbb{R}^+,">
</p>

其中 ![](https://latex.codecogs.com/svg.latex?\sigma(\cdot)) 表示softplus函数，![](https://latex.codecogs.com/svg.latex?\mathcal{N}_\theta) 是一个多层感知机（输出一个正数）。<br>
对所有的 ![](https://latex.codecogs.com/svg.latex?\left(p_i^{\mathcal{R}^\prime},p_j^\mathcal{O}\right)) 点对同时进行距离预测，得到完整的点对点预测距离矩阵，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)=\begin{bmatrix}\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_1^\mathcal{R},\widehat{\boldsymbol{\psi}}_1^\mathcal{O}\right)&\cdots&\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_1^\mathcal{R},\widehat{\boldsymbol{\psi}}_{N_\mathcal{O}}^\mathcal{O}\right)\\{}\vdots&\ddots&\vdots\\{}\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_{N_\mathcal{R}}^\mathcal{R},\widehat{\boldsymbol{\psi}}_1^\mathcal{O}\right)&\cdots&\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_{N_\mathcal{R}}^\mathcal{R},\widehat{\boldsymbol{\psi}}_{N_\mathcal{O}}^\mathcal{O}\right)\end{bmatrix}\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}}.">
</p>

<br><br><br><br><br><br><br><br>

# 将之前预测的点对点距离作为计算依据，反解出可供机器人手直接执行的抓取构型

## 1、机器人手预测点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3})

点对点预测距离矩阵的第 ![](https://latex.codecogs.com/svg.latex?i) 行，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_i^\mathcal{R},\widehat{\boldsymbol{\psi}}_1^\mathcal{O}\right),\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_i^\mathcal{R},\widehat{\boldsymbol{\psi}}_2^\mathcal{O}\right),\cdots,\mathcal{K}\left(\widehat{\boldsymbol{\psi}}_i^\mathcal{R},\widehat{\boldsymbol{\psi}}_{N_\mathcal{O}}^\mathcal{O}\right),">
</p>

表示机器人手预测点云中的第 ![](https://latex.codecogs.com/svg.latex?i) 个点 ![](https://latex.codecogs.com/svg.latex?p_i^{\mathcal{R}^\prime}\in\mathbb{R}^3) 与物体点云中所有 ![](https://latex.codecogs.com/svg.latex?N_\mathcal{O}) 个点 ![](https://latex.codecogs.com/svg.latex?p_1^\mathcal{O},p_2^\mathcal{O},\cdots,p_{N_\mathcal{O}}^\mathcal{O}\in\mathbb{R}^3) 的距离。通过多边定位法可以解算出 ![](https://latex.codecogs.com/svg.latex?p_i^{\mathcal{R}^\prime}\in\mathbb{R}^3) 的值，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?p_i^{\mathcal{R}^\prime}=\underset{p_i^\mathcal{R}}{\text{argmin}}\sum_{j=1}^{N_\mathcal{O}}\left(\left\lVert{}p_i^\mathcal{R}-p_j^\mathcal{O}\right\rVert_2^2-\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)_{ij}^2\right)^2\in\mathbb{R}^3.">
</p>

对 ![](https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}}) 的每一行同时执行这种过程，解算出完整的机器人手预测点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 。由此，机器人手预测点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 由未知量变为已知量。<br>
在后文中，将 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{R}^\prime}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 另记为 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{P}}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 。将所有预测点按各自所在的连杆的序号进行分组。由于连杆有 ![](https://latex.codecogs.com/svg.latex?N_\ell) 段，故分成 ![](https://latex.codecogs.com/svg.latex?N_\ell) 组。于是，![](https://latex.codecogs.com/svg.latex?\mathbf{P}^{\mathcal{P}}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 可等价表示为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}^\mathcal{P}\}_{i=1}^{N_\ell}.">
</p>

应注意，机器人手预测点云采用地面绝对坐标系下的坐标值。

## 2、连杆预测位姿 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{\mathcal{T}_i^*}=\left(\mathbf{x}_i^*,\mathbf{R}_i^*\right))

机器人手在初始张开构型 ![](https://latex.codecogs.com/svg.latex?q_\text{init}\in\mathbb{R}^{N_\text{DoF}}) 下的连杆点云（采用连杆相对坐标系下的坐标值）

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}\}_{i=1}^{N_\ell}">
</p>

已经被存储，在抓取构型下的连杆点云（采用地面绝对坐标系下的坐标值）则在上一步被预测为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\{\mathbf{P}_{\ell_i}^\mathcal{P}\}_{i=1}^{N_\ell}.">
</p>

通过刚体配准技术可以解算出每一段连杆的预测位姿。以第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆为例，其预测位姿即为

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\boldsymbol{\mathcal{T}_i^*}=\left(\mathbf{x}_i^*,\mathbf{R}_i^*\right)=\underset{\left(\mathbf{x}_i,\mathbf{R}_i\right)}{\text{argmin}}\left\lVert\mathbf{P}_{\ell_i}^\mathcal{P}-\mathbf{P}_{\ell_i}\left(\mathbf{x}_i,\mathbf{R}_i\right)\right\rVert_2.">
</p>

假如选定第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆上的某个任意点（例如左右两端某一端的端点）作为第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆相对坐标系的原点，那么：
- 以上公式中的 ![](https://latex.codecogs.com/svg.latex?\mathbf{x}_i\in\mathbb{R}^3) 表示该原点的3D平移向量的候选值，其各个分量分别是该原点 ![](https://latex.codecogs.com/svg.latex?x,y,z) 三坐标（地面绝对坐标）的候选值。
- 以上公式中的 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i\in\mathbb{R}^{3\times{}3}) 表示该相对坐标系的3D旋转矩阵的候选值，其第1、2、3列向量分别是该相对坐标系x轴、y轴、z轴上的单位向量（地面绝对坐标）的候选值。
- 对于第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆，已知其上各点在相对坐标系下的坐标值（存储在 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}_{\ell_i}\in\mathbb{R}^{n\times{}3}) 中），同时已知该相对坐标系的原点绝对坐标（存储在 ![](https://latex.codecogs.com/svg.latex?\mathbf{x}_i\in\mathbb{R}^3) 中）以及各坐标轴单位向量的绝对坐标（存储在 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i\in\mathbb{R}^{3\times{}3}) 中），那么，第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆上各点在绝对坐标系下的坐标值就可以被唯一确定，可用 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}_{\ell_i}\left(\mathbf{x}_i,\mathbf{R}_i\right)\in\mathbb{R}^{n\times{}3}) 表示。它与先前预测的、同样采用绝对坐标值的第 ![](https://latex.codecogs.com/svg.latex?i) 段连杆点云 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}_{\ell_i}^\mathcal{P}\in\mathbb{R}^{n\times{}3}) 之间的偏差值的二范数，就作为以上优化公式的迭代优化目标。
- 以上公式中的 ![](https://latex.codecogs.com/svg.latex?\mathbf{x}_i^*\in\mathbb{R}^3) 以及 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 是经过迭代优化后最终选定的预测值。

## 3、抓取构型 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{q}^*\in\mathbb{R}^{N_\text{DoF}})

从初始张开构型 ![](https://latex.codecogs.com/svg.latex?q_{\text{init}}\in\mathbb{R}^{N_\text{DoF}}) 到抓取构型 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{q}^*\in\mathbb{R}^{N_\text{DoF}}) ，将经过以下多轮迭代：

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\boldsymbol{q}\leftarrow\boldsymbol{q}+\delta\boldsymbol{q},">
</p>

其中

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}&\delta\boldsymbol{q}=\underset{\delta\boldsymbol{q}^\prime}{\text{argmin}}\left(\sum_{i=1}^{N_\ell}\left\lVert\mathbf{x}_i+\frac{\partial\mathbf{x}_i\left(\boldsymbol{q}^\prime\right)}{\partial\boldsymbol{q}^\prime}\delta\boldsymbol{q}^\prime-\mathbf{x}_i^*\right\rVert_2\right),\\&\text{s.t.\space}\boldsymbol{q}+\delta\boldsymbol{q}\in\left[\boldsymbol{q}_{\text{min}},\boldsymbol{q}_{\text{max}}\right],\;\,\left\lvert\delta\boldsymbol{q}\right\rvert\leq\varepsilon_q.\end{align*}">
</p>

![](https://latex.codecogs.com/svg.latex?\left[\boldsymbol{q}_{\text{min}},\boldsymbol{q}_{\text{max}}\right]) 表示关节限位区间，![](https://latex.codecogs.com/svg.latex?\varepsilon_q=0.5) 表示最大允许步长。
<br><br><br><br><br><br><br><br><br><br>

# 损失函数 ![](https://latex.codecogs.com/svg.latex?\mathcal{L})

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\mathcal{L}=&\lambda_\mathcal{D}\mathcal{L}_{\text{L1}}\left(\mathcal{D}\left(\mathcal{R},\mathcal{O}\right),\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)^\text{GT}\right)\\&+\lambda_\mathcal{T}\frac{1}{N_\ell}\sum_{i=1}^{N_\ell}\mathcal{L}_{\ell_i}\\&+\lambda_\mathcal{P}\left\lvert\mathcal{L}_\mathbf{P}\left(\mathbf{P}^\mathcal{T},\mathbf{P}^\mathcal{O}\right)\right\rvert\\&+\lambda_{KL}\mathcal{D}_{KL}\left(f_{\theta_\mathcal{G}}\left(\mathbf{P}^\mathcal{G},\boldsymbol{\psi}^\mathcal{R},\boldsymbol{\psi}^\mathcal{O}\right)\,\|\;\mathcal{N}\left(0,I\right)\right),\end{align*}">
</p>

其中 ![](https://latex.codecogs.com/svg.latex?\lambda_\mathcal{D},\lambda_\mathcal{T},\lambda_\mathcal{P},\lambda_{KL}) 为各损失项的尺度缩放超参数。

## 1、距离预测损失

![](https://latex.codecogs.com/svg.latex?\text{GT})（Ground Truth）意为真实值标注，即训练数据集当中的标签。神经网络预测的抓取构型下点对点距离矩阵 ![](https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}}) ，与训练数据集当中标注的、经过实际测量的抓取构型下点对点距离矩阵 ![](https://latex.codecogs.com/svg.latex?\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)^\text{GT}\in\mathbb{R}^{N_\mathcal{R}\times{}N_\mathcal{O}}) 的偏差值的一范数

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\mathcal{L}_{\text{L1}}\left(\mathcal{D}\left(\mathcal{R},\mathcal{O}\right),\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)^{\text{GT}}\right)=\sum_{i=1}^{N_\mathcal{R}}\sum_{j=1}^{N_\mathcal{O}}\left\lVert\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)_{ij}-\mathcal{D}\left(\mathcal{R},\mathcal{O}\right)_{ij}^\text{GT}\right\rVert_1\end{align*}">
</p>

作为损失项，驱动神经网络预测出准确的点对点距离。

## 2、位姿预测损失

在

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\mathcal{L}_{\ell_i}=\left\lVert\mathbf{x}_i^*-\mathbf{x}_i^\text{GT}\right\rVert_2+\text{arccos}\left(\frac{\text{tr}\left(\mathbf{R}_i^{*^\top}\mathbf{R}_i^\text{GT}\right)-1}{2}\right)">
</p>

当中，3D平移预测损失由 ![](https://latex.codecogs.com/svg.latex?\left\lVert\mathbf{x}_i^*-\mathbf{x}_i^\text{GT}\right\rVert_2) 构成，驱动神经网络对任意连杆均预测出准确的3D平移。3D旋转预测损失则由

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\begin{align*}\text{arccos}\left(\frac{\text{tr}\left(\mathbf{R}_i^{*^\top}\mathbf{R}_i^\text{GT}\right)-1}{2}\right)&=\text{arccos}\left(\frac{\text{tr}\left(\mathbf{R}_{rel_i}\right)-1}{2}\right)\\&=\text{arccos}\left(\frac{\left(1+2\cos\theta_{rel_i}\right)-1}{2}\right)\\&=\text{arccos}\left(\cos\theta_{rel_i}\right)\\&=\theta_{rel_i}\end{align*}">
</p>

构成。<br>
由于 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 与 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^\text{GT}\in\mathbb{R}^{3\times{}3}) 之间存在偏差，所以从 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 到 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^\text{GT}\in\mathbb{R}^{3\times{}3}) 需要经过一定旋转。设相对旋转矩阵为 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}\in\mathbb{R}^{3\times{}3}) ，那么就有 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\cdot\mathbf{R}_{rel_i}=\mathbf{R}_i^\text{GT}) ，从而解得 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}=\mathbf{R}_i^{*^\top}\mathbf{R}_i^\text{GT}) 。<br>
设 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}\in\mathbb{R}^{3\times{}3}) 的旋转角为 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}\in\left[0,\pi\right]) ，则从 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 到 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^\text{GT}\in\mathbb{R}^{3\times{}3}) 所旋转的角度即为 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}) 。<br>
倘若从 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 到 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^\text{GT}\in\mathbb{R}^{3\times{}3}) 需要旋转一个大于 ![](https://latex.codecogs.com/svg.latex?180^\circ) 的角度 ![](https://latex.codecogs.com/svg.latex?\theta) ，则在旋转平面上反方向旋转小于 ![](https://latex.codecogs.com/svg.latex?180^\circ) 的角度 ![](https://latex.codecogs.com/svg.latex?360^\circ-\theta) ，可以达成同样的效果。因此，规定旋转角 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}) 的范围是 ![](https://latex.codecogs.com/svg.latex?\left[0,\pi\right]) 。<br>
由于 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}\in{}SO(3)) ，![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}) 与 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}) 之间其实存在一个恒成立的关系式，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\text{tr}\left(\mathbf{R}_{rel_i}\right)=1+2\cos\theta_{rel_i},">
</p>

因此可以通过 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}) 反解出 ![](https://latex.codecogs.com/svg.latex?\cos\theta_{rel_i}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\cos\theta_{rel_i}=\frac{\text{tr}\left(\mathbf{R}_{rel_i}\right)-1}{2},">
</p>

进而反解出 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}) ，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\theta_{rel_i}=\text{arccos}\left(\frac{\text{tr}\left(\mathbf{R}_{rel_i}\right)-1}{2}\right),">
</p>

代回 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_{rel_i}=\mathbf{R}_i^{*^\top}\mathbf{R}_i^\text{GT}) ，即得

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\theta_{rel_i}=\text{arccos}\left(\frac{\text{tr}\left(\mathbf{R}_i^{*^\top}\mathbf{R}_i^\text{GT}\right)-1}{2}\right).">
</p>

正实数弧度值 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}\in\left[0,\pi\right]) 直接度量了 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^*\in\mathbb{R}^{3\times{}3}) 与 ![](https://latex.codecogs.com/svg.latex?\mathbf{R}_i^\text{GT}\in\mathbb{R}^{3\times{}3}) 之间的3D偏差角，是3D旋转矩阵之间最可信的偏差指标。将 ![](https://latex.codecogs.com/svg.latex?\theta_{rel_i}) 作为平均损失项

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\frac{1}{N_\ell}\sum_{i=1}^{N_\ell}\mathcal{L}_{\ell_i}">
</p>

的构成部分，可驱动神经网络对各段连杆同时预测出准确的3D旋转矩阵。

## 3、点云穿透损失

在仿真训练环境里，当机器人手执行完毕抓取构型 ![](https://latex.codecogs.com/svg.latex?\boldsymbol{q}^*\in\mathbb{R}^{N_\text{DoF}}) 之后，对机器人手点云进行测量。测量结果记为 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{T}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 。<br>
在仿真训练环境中，最终抓取状态下的机器人手可能穿透到物体内部。为了对这种违背物理事实的现象施加惩罚，引入 ![](https://latex.codecogs.com/svg.latex?\left\lvert\mathcal{L}_\mathbf{P}\left(\cdot,\cdot\right)\right\rvert) 绝对值函数，它接受 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{T}\in\mathbb{R}^{N_\mathcal{R}\times{}3}) 与 ![](https://latex.codecogs.com/svg.latex?\mathbf{P}^\mathcal{O}\in\mathbb{R}^{N_\mathcal{O}\times{}3}) 作为输入，按照一定计算规则，输出一个正实数，即

<p align="center">
<img src="https://latex.codecogs.com/svg.latex?\left\lvert\mathcal{L}_\mathbf{P}\left(\mathbf{P}^\mathcal{T},\mathbf{P}^\mathcal{O}\right)\right\rvert.">
</p>

这个正实数是机器人手对物体穿透程度的量化结果，将其作为损失项，可防止神经网络预测出导致穿模的抓取构型。
