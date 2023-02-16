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





























