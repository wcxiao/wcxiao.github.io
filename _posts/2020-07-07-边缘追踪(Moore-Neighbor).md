---
layout: post
title: '边缘追踪(Moore-Neighbor Tracing)'
date: 2020-07-07
author: xiaowc
color: rgb(255,210,32)
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-banner.png'
tags: dip
---

使用Moore-Neighbor算法进行边缘追踪;

### Moore Neighborhood

Moore领域：像素点P的八个邻居，如图所示：

![mooreneighbor](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/mooreneighbor/mooreneighbor.png)

### 算法过程

输入二值图，指定需要追踪的边缘上的一个点P作为起始点，按照顺时针或者逆时针顺序遍历P的Moore邻域(整个算法过程中遍历顺序需要保持一致)，每次遇到值为1的点，同样对该点的Moore邻域进行遍历，直到遇到一个值为1的点，重复上述过程。当再次回到起始点后算法终止。

伪代码如图：

![mooretracing](./asset/mooretracing.png)

![](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/mooreneighbor/mooretracing.png)

### Jacob停止条件

上述过程存在问题，如图：

![exception1](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/mooreneighbor/exception1.png)

从图中蓝色线所在的点开始，顺时针遍历，遇到邻域中的点P，再从P点开始遍历：

![execption2](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/mooreneighbor/exception2.png)

顺时针遍历P点的邻域，最后又回到起始点，根据终止条件，算法终止。

![exception3](https://picholder.oss-cn-shanghai.aliyuncs.com/dip/mooreneighbor/exception3.png)

显然最后得到的边缘并不完整，算法在这种条件下不能正确运行，于是更改终止条件，使用Jacob终止条件;

**Jacob's stopping criterion**: the algorithm terminates when it visits the **start** pixel for a second time in the same direction it did the first time around.

当第二次从同一方向进入起始点才停止算法；

定义：

 [ 2 ][ 3 ][ 4 ]
 [ 1 ][ X ][ 5 ]
 [ 8 ][ 7 ][ 6 ]

设上表表示点X的Moore邻域，按照数字顺序进行遍历：1->2->3->...->8

遍历时，若从同一个点遍历到起始点则视为从同一方向进入起始点；即将某一点X的方向定义为到达该点X前的点的位置；

例如按照顺时针顺序遍历，则有：8->1, 1->2, 2->3, 3->4, 4->5, 5->6, 6->7, 7->8, 8->1;

于是对应方向：8在1的邻域中位置7上，1在2的邻域中位置7上，2在3的邻域中位置1上......

所以1~8各位置的前一个点所在位置：7 7 1 1 3 3 5 5

## matlab实现

```matlab
function boundary = mooreneighbortracing(binimg,pos)
%mooreneighbortracing(binimg,pos)
%   binimg: 输入的二值图
%   pos: 给定位置(x,y)作为起始点,寻找该点所在边缘
%   boundary: 输出边缘所有点的坐标集合

initial_entry = pos;

% 点X的Moore邻域，按照数字顺序进行遍历
% [ 2 ][ 3 ][ 4 ]
% [ 1 ][ X ][ 5 ]
% [ 8 ][ 7 ][ 6 ]
% 八个邻居的坐标偏移：
neighborhood = [ 0 -1; -1 -1; -1 0; -1 1; 0 1; 1 1; 1 0; 1 -1 ];
exit_direction = [ 7 7 1 1 3 3 5 5 ];

% 遍历给定点的邻域，找到第一个值为1的点
for n = 1 : 8 % 8-connected neighborhood
	c = initial_entry + neighborhood( n, : );
    if binimg( c( 1 ), c( 2 ) ) == 1
        initial_position = c;
        break;
    end
end

% 基于找到的点的位置设置进入该点的方向
initial_direction = exit_direction( n );

% 将这个点的位置加入边界集合
boundary( 1, : ) = initial_position;

% 初始化循环变量
position = initial_position;
direction = initial_direction;
boundary_size = 1;

% 循环查找
while true

    % 顺时针查找
    for i=1:8
        n = mod(direction + i - 1, 8);
        if n==0
            n = 8;
        end
        c = position + neighborhood(n,:);
        if binimg(c(1),c(2))==1
            position = c;
            break;
        end
    end

    % 根据找到的点的信息进行更新
    direction = exit_direction( n );
    boundary_size = boundary_size + 1;
    boundary( boundary_size, : ) = position;

    % Jacob's stopping criterion
    if all( position == initial_position ) &&...
    ( direction == initial_direction )
    	break;
    end
end

end

```

**具体应用：**https://github.com/wcxiao/DigitalImageProcessing/tree/master/hw3

## reference

Moore-Neighbor tracing

    http://www.imageprocessingplace.com/downloads_V3/root_downloads/tutorials/contour_tracing_Abeer_George_Ghuneim/moore.html
    https://www.mathworks.com/matlabcentral/fileexchange/42144-moore-neighbor-boundary-trace

Jacob's stopping criterion

    http://www.imageprocessingplace.net/downloads_V3/root_downloads/tutorials/contour_tracing_Abeer_George_Ghuneim/raymain.html
