## IPC

<img src="/Volumes/disk2/notebooks/Papers/assets/image-20230109180004801.png" alt="image-20230109180004801" style="zoom:50%;" />

 [A Fast Variational Framework for Accurate Solid-Fluid Coupling.pdf](coupling/A Fast Variational Framework for Accurate Solid-Fluid Coupling.pdf) 

## surface tracking

### surface tracking 

(1) particles  ==> mesh   [only geometry]

LosTopos [Multimaterial Mesh-Based Surface Tracking.pdf](./Surface tracking/Multimaterial Mesh-Based Surface Tracking.pdf)
EI Topo[ROBUST TOPOLOGICAL OPERATIONS FOR DYNAMIC EXPLICIT SURFACES.pdf](./Surface tracking/ROBUST TOPOLOGICAL OPERATIONS FOR DYNAMIC EXPLICIT SURFACES.pdf)

(2) sdf ==> mesh [sdf can be advected,  marching cube]



### codimensional fluid: 

(particles, mesh) ==> (new particles, new mesh)  [physic and geometry]

包括物理运算和后处理阶段

particle附带了余维信息

### 结合

直接结合似乎不太行，但是可以利用surface tracking 实现particle system和codimesh的转化

codimeshj ==> particle system 直接提取粒子就好了

particle system ==> codimesh  感觉可以

1. 使用surface  tracking方法： particle system ==> mesh
2. 计算厚度： mesh ==> codimesh

转化的应用： mpm？



## MPM

mpm是 （particle + grid） 适合做耦合

codimension是 （particle + mesh）适合做特殊形态（薄膜、水丝）

如何确定相互转换的时机？

- 检测到出现solid-fluid碰撞的时候转化为mpm？
- 怎么确定什么时候需要特殊形态（薄膜、水丝）？
- 或者从空间上去分 某些区域用MPM，某些区域用codimesh

<img src="/Volumes/disk2/notebooks/Papers/assets/image-20230110185715970.png" alt="image-20230110185715970" style="zoom:50%;" />

## 水+布料

布料用Codimesh

<img src="/Volumes/disk2/notebooks/Papers/assets/image-20230110211945397.png" alt="image-20230110211945397" style="zoom:50%;" />





## Codimesh和Codimesh耦合

## Codimesh追踪表面优化MPM耦合方法



<img src="/Volumes/disk2/notebooks/Papers/assets/image-20230111202759836.png" alt="image-20230111202759836" style="zoom:50%;" />

## 现象

### 拔丝地瓜

拉丝 然后凝固（结合 MPM相场的工作？）

### 颜料滴入水中

### 耦合中的codimension现象

水流过固体，固体表面残留液滴

水流过small gap，残留水膜

### 吹泡泡的杆的头部（圆孔状）粘到泡泡水（solid-fluid coupling），形成膜,在吹出来



大表面张力流固耦合







 [receeding mpm.pdf](MPM/receeding mpm.pdf) 

​	





纽卡斯尔的那个球

![image-20230521185126485](assets/image-20230521185126485.png)





### 能做的现象

融化（multi phase）+气泡

<img src="assets/image-20230508164034826.png" alt="image-20230508164034826" style="zoom:33%;" />



![QQ20230508-164459](assets/QQ20230508-164459.gif)



![QQ20230508-171731](assets/QQ20230508-171731.gif)



![QQ20230508-174733](assets/QQ20230508-174733.gif)



### 能做这个的原因

1. 目前没有mpm的文章能做气泡
2. 有flip做水中气泡的文章（reduced fluid）



### 参考的一些文章

1. reduced fluid（用Houdini SDK开源，我的系统不太支持，重装了一个linux系统在搭环境）

<img src="assets/image-20230510142630873.png" alt="image-20230510142630873" style="zoom:33%;" />

<img src="assets/image-20230510142738341.png" alt="image-20230510142738341" style="zoom:33%;" />

是基于flip的，如果迁移到mpm中，优点在于mpm对比flip的优点，比如通过不同本构模型，能模拟更多不同材质的耦合

2. ghost sph（C++开源）

<img src="assets/image-20230510142940873.png" alt="image-20230510142940873" style="zoom: 50%;" />

3. 这篇同样没有显示的建模气体，气体起的作用很少，做不了气泡

<img src="assets/image-20230510143126383.png" alt="image-20230510143126383" style="zoom:50%;" />

4. LBM  这个和MPM 连续介质力学的体系不太一样 可能不太好迁移

同时这篇文章的气泡和我想要的气泡不太一样

* 包裹：浪把气体包裹进水
* 生成：黄油内部有水或其他会蒸发的物质，受热蒸发产生水蒸气

![image-20230510143451703](assets/image-20230510143451703.png)

![image-20230510143510365](assets/image-20230510143510365.png)



### 目前想的具体做法

第一步：先借鉴sph、欧拉网格或LBM里的气泡方法，融进mpm里，做到简单的现象比如水中气泡上升

第二步：结合multi phase的方法





<img src="assets/001.jpg" alt="001" style="zoom:33%;" />



<img src="assets/030.jpg" alt="030" style="zoom:33%;" />



<img src="assets/060.jpg" alt="060" style="zoom:33%;" />



1. 物理理论上气泡体积变化、产生、受力分析
2. 表面追踪自适应细分、mesh追踪、和p2g结合

