---
layout: post
title: OpenFOAM多处理器并行计算
tags: OpenFOAM
math: true
date: 2025-9-25
image:
---
![conda](https://github.com/ht894419944/ht894419944.github.io/raw/master/_posts/image/2025-9-25-OpenFOAM/OpenFOAM.jpg)

**OpenFOAM多处理器并行计算，提升工作效率**

### 1. 问题描述  

当模型复杂或者有限元网格精密时，单处理器计算时间比较长，可以用多处理器并行计算，以充分利用电脑资源来节省计算时间。

### 2. 解决方法

<mark>Step1:</mark> 在**system**文件夹下配置decomposeParDict文件

```
numberOfSubdomains 4;                // 启动4个处理器进行计算
method          simple;              // 求解区域分解方法
simpleCoeffs
{
    n               (2 2 1);         // 将求解区域在x方向划分为2块，y方向2块，z方向1块
}
hierarchicalCoeffs
{
    n               (1 1 1);
    order           xyz;
}
manualCoeffs
{
    dataFile        "";
}
distributed     no;
roots           ( );
```

<mark>Step2:</mark> 划分网格。Linux终端输入：

```
blockMesh
```

<mark>Step3:</mark> 分解区域。Linux终端输入：

```
decomposePar
```

<mark>Step4:</mark> 多处理器并行计算。Linux终端输入：

```
mpirun -np 4 pimpleFoam -parallel
```

我用的是pimpleFoam求解器来求解incompressible问题

<mark>Step5:</mark> 合并多处理器的计算结果。Linux终端输入：

```
 reconstructPar
```

**参考资源**
[OpenFOAM v11 User Guide - 2.2 Breaking of a dam](https://doc.cfd.direct/openfoam/user-guide-v11/dambreak)  
[OpenFOAM v11 User Guide - 3.4 Running applications in parallel](https://doc.cfd.direct/openfoam/user-guide-v11/running-applications-parallel)  
[OpenFOAM: Quickstart](https://doc.openfoam.com/2306/quickstart/)  
[OpenFOAM: pimpleFoam](https://doc.openfoam.com/2306/tools/processing/solvers/rtm/incompressible/pimpleFoam/#)

---

- [ ] 学废了 :/
- [x] 学会了 :)

>作者：韩涛