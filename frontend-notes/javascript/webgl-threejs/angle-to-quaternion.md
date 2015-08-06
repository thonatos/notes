
四元数与旋转

#### 四元数（Quaternion）

在3d模型的建立中，Quaternion额外的重要，通常会使用它来表示一个空间旋转。

那么我们首先得明白什么是四元数，以及如何用它来表示一个旋转。

#### 基本表达

    ## w表示旋转角度，( x, y, z ) 表示旋转轴;
    Quaternion = ( x, y, z, w );
    qx = ax * sin(angle/2)
    qy = ay * sin(angle/2)
    qz = az * sin(angle/2)
    qw = cos(angle/2)

#### 推导公式

    ## 三角函数
    cos(angle/2)2 + sin(angle/2)2 = 1; (1)
    ## 向量相乘
    ax*ax + ay*ay + az*az = 1; (2)
    ## 由(1)(2)可得下式 
    cos(angle/2)2 + (ax*ax+ ay*ay + az*az) * sin(angle/2)2 = 1;
    cos(angle/2)2 + ax*ax * sin(angle/2)2 + ay*ay * sin(angle/2)2+ az*az * sin(angle/2)2 = 1
    ## 于是可以得到
    qw2 + qx2 + qy2 +qz2 =1
    ## 则：
    qw = cos(angle/2)
    qx = ax * sin(angle/2)
    qy = ay * sin(angle/2)
    qz = az * sin(angle/2)

#### 应用实例

存在一个向量(1,0,0);绕着轴旋转90度，则可以表示如下：

    angle = 90 degrees;
    axis = ( 1, 0, 0 );
    cos(45 degrees) = 0.7071
    sin(45 degrees) = 0.7071;
    qx= 0.7071;
    qy = 0;
    qz = 0;
    qw = 0.7071;

