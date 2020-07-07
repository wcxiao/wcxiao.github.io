---
layout: post
title: '卷积(convolution)和相关(correlation)'
date: 2020-07-07
author: xiaowc
color: rgb(255,210,32)
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: dip
---

使用matlab实现卷积和相关计算函数

卷积：myconv2(A,B,shape)

相关：mycorr2(A,B,shape)

## 实现

输入：A(m\*n)(图像), B(p\*q)(滤波器：p==q=2k+1, 3\*3, 5\*5,..., (2k+1)\*(2k+1)), shape('full','same','valid')

输出：经过滤波器B滤波的图像C

相关操作过程如图：

![corr](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/correlation.png)

先对图像A进行0填充：对顶部和底部，左侧和右侧各填充(p-1)行/列0；

然后使用B对A进行滑动阵列乘积求和；

参考matlab提供的库函数conv2，使用参数shape定义输出矩阵的大小：

shape=='full'：输出矩阵是拓展后的矩阵，大小比原图像矩阵大；

shape=='same'：输出矩阵与原图像矩阵大小一致；

shape=='valid'：不对图像A进行填充，直接进行滑动乘积求和；输出矩阵大小比原图像矩阵小；

![full](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/conv_full.png)

![same](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/conv_same.png)

![valid](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/conv_valid.png)

例如：

![AB](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/inputmatrix.png)

![output](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/convolution/output.png)

```matlab
E = ones(3,3)
F = ones(3,3)
myconv2(E,F,'valid') = 9
```

## 代码

myconv2.m：

```matlab
function C = myconv2(A,B,shape)
%myconv2 convolution of image-A and kernel-B
% 卷积需要将B旋转180度，之后的步骤与相关操作一样;

%rotate 180
Br = rot90(B,2);
%correlation
C = mycorr2(A,Br,shape);

end
```

mycorr2.m:

```matlab
function Out = mycorr2(A,B,shape)

[m,n] = size(A);
[p,q] = size(B);

%paddedsize:
PM = m+2*(p-1);
PN = n+2*(q-1);

%zero padding
Cpadded = zeros(PM,PN);
C = zeros(PM,PN);
Cpadded(p:p+m-1,q:q+n-1) = A;

%calc loop variables
a = (p-1)/2;
b = (q-1)/2;
rstart = (p+1)/2;
rend = PM-a;
cstart = (q+1)/2;
cend = PN-b;

%calc
for i=rstart:1:rend
    for j=cstart:1:cend
        tmp = Cpadded(i-a:i+a,j-a:j+a).*B;
        C(i,j) = sum(sum(tmp));
    end
end

%cut
if strcmp(shape,'full')
    Out = C(rstart:rend,cstart:cend);
elseif strcmp(shape,'same')
    Out = C(p:PM-p+1,q:PN-q+1);
elseif strcmp(shape,'valid')
    rs = rstart + p-1;
    re = rend - (p-1);
    cs = cstart + q-1;
    ce = cend - (q-1);
    Out = C(rs:re,cs:ce);
end

end
```

## reference

《数字图像处理》第三版 Rafael C. Gonzalez编著

https://www.cnblogs.com/xiaojianliu/p/9076547.html

https://www.cnblogs.com/yibeimingyue/p/10879506.html

https://blog.csdn.net/mrahut/article/details/81539435

https://www.cnblogs.com/alexanderkun/p/8149088.html

https://www.cnblogs.com/hyb221512/p/9370367.html
