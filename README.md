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

一个基本的考虑在于，机器人手上的同一点所对应的特征向量的方向（排除光照强度等无关因素），在机器人手的不同构型下应该保持基本一致。否则，机器人对自己的手的感知就是虚假的，机器人其实根本不清楚自己的手是长什么样的。此时机器人手对机器人神经网络而言只是一个属性未知的外部实体，机器人根本不知道如何使用自己的手，机器人运用自己的手进行物体抓取的任务也就失去了实现的基础。<br>
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
