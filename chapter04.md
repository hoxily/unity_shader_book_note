# 第4章 数学基础

计算机图形学是建立在虚拟世界上的数学模型。经常需要使用到矢量与矩阵（线性代数）。

## 笛卡尔坐标系

Cartesian coordinate system.

### 4.2.1 二维笛卡尔坐标系

图4.3 显示了一个二维笛卡尔坐标系。

![一个二维笛卡尔坐标系](images/chapter04_2d_cartesian_coordinate_system.png)

一个二维笛卡尔坐标系包含了两部分信息：

- 一个特殊的位置——原点，它是整个坐标系的中心。
- 两条过原点并且互相垂直的矢量，即x轴和y轴。这些坐标轴也被称为该坐标系的基矢量。

### 4.2.2 三维直角坐标系

![三维直角坐标系](images/chapter04_3d_cartesian_coordinate_system.png)

3个坐标轴之间互相垂直，且长度为1.

根据坐标轴方向，可以划分出两种坐标系：左手坐标系和右手坐标系。

### 4.2.3 左手坐标系与右手坐标系

如果两个坐标系具有相同的旋向性（handedness），那么我们就可以通过旋转的方法来让它们的坐标轴指向重合。但是，如果它们具有不同的旋向性，那么就无法达到重合的目的。

![左手坐标系](images/chapter04_left_handedness_coordinate_system.png)

左手坐标系

![右手坐标系](images/chapter04_right_handedness_coordinate_system.png)

右手坐标系

除了坐标轴朝向不同，两坐标系对于正向旋转的定义也不同。分别对应左手法则和右手法则。

### 4.2.4 Unity使用的坐标系

对于模型空间和世界空间，Unity使了左手坐标系。这意味着，在模型空间中，一个物体的右侧、上侧、前向分别对应了x轴、y轴、z轴的正方向。

对于观察空间来说，Unity使用的是右手坐标系。观察空间，一般是指以摄像机为原点的坐标系。

### 4.2.5 练习题

1. 在3DS max中，默认的坐标轴方向是：x轴正方向指向右方，y轴正方向指向前方，z轴正方向指向上方。问它是左手坐标系还是右手坐标系？（答：右手坐标系）
2. 在左手坐标系中，有一个点的坐标是(0, 0, 1)。如果把该点绕y轴旋转+90°，旋转后的坐标是多少？如果是在右手坐标系中，有一个点的坐标是（0, 0, 1)。如果把该点绕y轴旋转+90°，旋转后的坐标是多少？（答：在左手坐标系中，结果为(1, 0, 0)。在右手坐标系中，结果为(1, 0, 0)）
3. 在Unity中，新建的场景中主摄像机的位置位于世界空间中的(0, 1, -10)位置，保持Rotation为（0, 0, 0)，Scale为(1, 1, 1)。再在世界空间的(0, 1, 0)位置新建一个球体。问：在相机的观察空间下，球的坐标的z值是多少？在相机的模型空间下，球的坐标的z值是多少？（答：-10, 10）

## 4.3 点和矢量

点（point）是n维空间中的一个位置，它没有大小、宽度这类概念。在游戏中主要使用二维和三维空间。使用 $P=(x, y)$ 表示二维空间的点。使用 $P=(x, y, z)$ 表示三维空间中的点。

矢量（vector），也被称为向量，它的定义更复杂一些。矢量是指n维空间中一种包含了模（magnitude）和方向（direction）的有向线段。

矢量的模是指这个矢量的长度。矢量的模总是大于或等于0.

矢量的方向描述了这个矢量在空间中的指向。

使用 $\vec v = (x, y)$ 来表示二维矢量。

使用 $\vec v = (x, y, z)$ 来表示三维矢量。

使用 $\vec v = (x, y, z, w)$ 来表示四维矢量。

### 4.3.1 点和矢量的区别

![矢量以及它的头与尾](images/chapter04_head_and_tail_of_a_vector.svg)

图4.15 矢量以及它的头与尾

![图4.16 点和矢量之间的关系](images/chapter04_relation_between_point_and_vector.svg)

图4.16 点和矢量之间的关系

如果把矢量的尾固定在坐标系原点，那么这个矢量的表示就和点的表示重合了。如上图所示。

任何一个点都可以表示成一个从原点出发的矢量。

### 4.3.2 矢量运算

#### 4.3.2.1 矢量与标量的乘法、矢量与标量的除法

标量与矢量相乘，结果为该标量与该矢量的各个分量相乘：

$$
k \vec v = (kx, ky, kz)
$$


矢量被一个非零的标量除，结果为该矢量与该标量的倒数相乘：

$$
\frac{\vec v}{k} = \frac{1}{k}\vec v = (\frac{x}{k}, \frac{y}{k}, \frac{z}{k}), k \neq 0
$$

对于乘法来说，矢量与标量的位置可以互换。但是对于除法，只能是矢量被标量除。

从几何意义上看，把一个矢量$\vec v$和一个标量$k$相乘，意味着对矢量$\vec v$进行一个大小为$|k|$的缩放。当$k$小于0时，矢量的方向将会取反。

![矢量与标量的乘法与除法](images/chapter04_scale_vector.png)

图4.17 矢量与标量的乘法与除法

#### 4.3.2.2 矢量与矢量的加减法

两个相同维度的矢量相加或相减，其结果是一个相同维度的新矢量，两个矢量对应分量的相加或相减。

$$
\vec a + \vec b = (a_x + b_x, a_y + b_y, a_z + b_z)
$$

$$
\vec a - \vec b = (a_x - b_x, a_y - b_y, a_z - b_z)
$$

从几何意义上看，矢量$\vec a + \vec b$等于把矢量$\vec a$的头连接到矢量$\vec b$的尾，然后画一条从$\vec a$的尾到$\vec b$的头的矢量。这被称为矢量加法的三角形定则。

![图4.18 二维矢量的加法与减法](images/chapter04_add_and_subtract_of_vector.png)

图4.18 二维矢量的加法与减法

#### 4.3.2.3 矢量的模

$$
|\vec v| = \sqrt{x^2 + y^2 + z^2}
$$

#### 4.3.2.4 单位矢量

单位矢量（unit vector）是指模为1的矢量。也被称之为被归一化的矢量（normalized vector）。

对于任何给定的非零矢量，把它转换成单矢量的过程被称为归一化（normalization）。

通过在一个矢量的头上添加戴帽符号来表示单位矢量，例如 $\hat{\vec v}$.

$$
\hat{\vec v} = \frac{\vec v}{|\vec v|}, 其中 \vec v 是非零矢量
$$

#### 4.3.2.5 矢量的点积

$$
\vec a \cdot \vec b = (a_x, a_y, a_z) \cdot (b_x, b_y, b_z) = a_xb_x + a_yb_y + a_zb_z
$$

点积满足交换律，即 $\vec a \cdot \vec b = \vec b \cdot \vec a$

点积的一个几何意义就是投影（projection）。通俗的解释就是，有一个光源，它发出的光线垂直于 $\hat{\vec a}$ 方向，那么$\vec b$在$\hat{\vec a}$方向上的投影就是$\vec b$在$\hat{\vec a}$方向上的影子。如下图所示。

![图4.22 矢量b在单位矢量a方向上的投影](images/chapter04_vector_b_project_on_vector_a.png)

图4.22 矢量$\vec b$在单位矢量$\hat{\vec a}$方向上的投影

投影结果的正负号与两个向量的夹角有关。

- 当夹角大于90°时，结果小于0；
- 当夹角为90°时，结果为0；
- 当夹角小于90°时，结果大于0；

参见图4.23.

![图4.23 点积的符号](images/chapter04_sign_of_dot_product.png)

性质1：点积可结合标量乘法

$$
(k \vec a) \cdot \vec b = \vec a \cdot (k \vec b) = k(\vec a \cdot \vec b)
$$

性质2：点积与矢量的加法和减法结合

$$
\vec a \cdot (\vec b + \vec c) = \vec a \cdot \vec b + \vec a \cdot \vec c
$$

性质3：一个矢量和本身进行点积的结果，是该矢量的模的平方。

$$
\vec v \cdot \vec v = x^2 + y^2 + z^2 = |\vec v|^2
$$

$$
\vec a \cdot \vec b = |\vec a||\vec b|\cos \theta，\theta为两向量的夹角
$$

#### 4.2.3.6 矢量的叉积

矢量叉积的结果仍是一个矢量。

$$
\vec a \times \vec b = (a_yb_z - a_zb_y, a_zb_x - a_xb_z, a_xb_y - a_yb_x)
$$

![辅助记忆表格](images/chapter04_cross_product_tip_table.png)

辅助记忆表格，结果位于第三个分量。

![辅助记忆三棱柱](images/chapter04_cross_product_tip_triangular_prism.png)

辅助记忆三棱柱，某个面的计算结果对应于对面棱的分量。

叉积不满足交换律，即 $\vec a \times \vec b \neq \vec b \times \vec a$。
叉积满足反交换律，即 $\vec a \times \vec b = -(\vec b \times \vec a)$。
叉积不满足结合律，即 $(\vec a \times \vec b) \times \vec c \neq \vec a \times (\vec b \times \vec c)$。

对两个矢量进行叉积的结果会得到一个同时垂直于这两个矢量的新矢量。

$|\vec a \times \vec b| = |\vec a||\vec b|\sin \theta , 其中 \theta 为两向量的夹角。$

![图4.26 使用矢量a与矢量b构建一个平行四边形](images/chapter04_area_of_2_vectors.png)

这个平行四边形的面积如下：

$$
A = |\vec b|h = |\vec b|(|vec a|\sin \theta) = |\vec a||\vec b|\sin \theta = |\vec a \times \vec b|
$$

![图4.27 分别在左手坐标系和右手坐标系下的叉积结果](images/chapter04_cross_product_in_left_right_handedness_coordinate_system.png)

![图4.28 使用右手法则判断右手坐标系中两向量的叉积的方向](images/chapter04_use_right_headedness.png)

### 4.3.3 练习题

1. 是非题
    1. 一个矢量的大小不重要，我们只需要在正确的位置把它画出来就可以了。（错误，大小很重要。另外矢量没有位置，可以随意把它放在空间的任何位置。）
    2. 点可以认为是位置矢量，这是通过把矢量的尾固定在原点得到的。（正确）
    3. 选择左手坐标系还是右手坐标系很重要，因为这会影响叉积的计算。（错误。无影响。在把数字转换成视学表现的时候，选择不同的坐标系可能会得到不同的结果。）
2. 计算题
    1. $|(2, 7, 3)|$ （结果=$\sqrt{2^2 + 7^2 + 3^2} = \sqrt{62}$）
    2. $2.5(5, 4, 10)$（结果=$(12.5, 10, 25)$）
    3. $\frac{(3,4)}{2}$（结果=$(1.5, 2)$）
    4. 对(5, 12)进行归一化。（$\frac{(5, 12)}{|(5, 12)|} = \frac{(5, 12)}{13}= (\frac{5}{13}, \frac{12}{13})$）
    5. 对(1,1,1)进行归一化。（$\frac{(1,1,1)}{|(1,1,1)|} = \frac{(1,1,1)}{\sqrt{3}} = (\frac{\sqrt 3}{3},\frac{\sqrt 3}{3},\frac{\sqrt 3}{3})$）
    6. (7,4)+(3,5)（结果=$(10,9)$）
    7. (9,4,13)-(15,3,11)（结果=$(-6,1,2)$）
3. 两点间距离问题。假设，场景中有一个光源，位置在（10, 13, 11)处，还有一个点(2,1,1)，那么光源距离该点的距离是多少？答：$d = |(10,13,11) - (2,1,1)| = |(8,12,10)| = \sqrt{8^2+12^2+10^2} = \sqrt{308} = 2 \sqrt{77}$
4. 计算题
    1. $(4,7)\cdot(3,9)$ （答：$4 \times 3 + 7 \times 9 = 12+63 = 75$）
    2. $(2,5,6)\cdot(3,1,2)-10$ （答：$2 \times 3 + 5 \times 1 + 6 \times 2 - 10 = 6+5+12-10 = 13$）
    3. $0.5(-3,4)\cdot(-2,5)$ （答：$(-\frac{3}{2},2)\cdot(-2,5) = -\frac{3}{2}\times(-2)+2\times 5 = 3+10 = 13$）
    4. $(3,-1,2)\times(-5,4,1)$ （答：$(-1 \times 1 - 2 \times 4, 2 \times (-5) - 3 \times 1, 3 \times 4 - (-1)\times(-5)) = (-1 - 8, -10 - 3, 12-5) = (-9,-13,7)$）
    5. $(-5,4,1)\times(3,-1,2)$ （答：根据反交换律，结果 = $-(3,-1,2)\times(-5,4,1) = (9,13,-7)$）
5. 已知矢量$\vec a$和矢量$\vec b$，$\vec a$的模为4，$\vec b$的模为6，它们之间的夹角为60°。计算：
    1. $\vec a \cdot \vec b$ （结果 = $|\vec a||\vec b|\cos60° = 4 \times 6 \times \frac{1}{2} = 12$）
    2. $|\vec a \times \vec b|$ （结果= $|\vec a||\vec b|\sin60° = 4 \times 6 \times \frac{\sqrt3}{2} = 12\sqrt3$）
6. 假设，场景中有一个NPC，它位于点p处，它的前方（forward)可以使用矢量$\vec v$来表示.
    1. 如果现在玩家运动到了点x处，那么如何判断玩家是在NPC的前方还是后方？请使用数学公式来描述你的答案。提示：使用点积。（计算 $(x - p) \cdot \vec v$ 的大小，如果点积的结果大于0说明玩家在NPC的前方，如果等于0，说明玩家在NPC的左侧或者右侧，如果小于0，则说明玩家在NPC的后方。）
    2. 使用你在1中提到的方法，代入 $p=(4,2), v=(-3,4), x=(10,6)$ 来验证你的答案。 （代入计算得-2，所以玩家位于NPC的后方。）
    3. 现在，游戏有了新的需求：NPC只能观察到有限的视角范围，这个视角的角度是$\phi$，也就是说NPC最多只能看到它前方左侧或者右侧$\frac{\phi}{2}$角度内的物体。那么我们如何通过点积来判断NPC是否能看到点x呢？（设$\theta$为$\vec v 与 \vec{PX}$向量的夹角，如果$\theta 小于 \frac{\phi}{2}$, 那么NPC就能看到点x。想要使$\theta 小于 \frac{\phi}{2}$，只要使$\cos\theta > \cos\frac{\phi}{2}$。而其中的$\cos\theta = \frac{\vec{PX}\cdot\vec v}{|\vec{PX}||\vec v|}$。也即$\frac{\vec{PX}\cdot\vec v}{|\vec{PX}||\vec v|} > \cos\frac{\phi}{2}$时，就能看到点x，否则看不到点x。）
    4. 在第3条的基础上，策划又有了新的需求：NPC的观察距离也有了限制，它只能看到固定距离内的对象，现在该如何判断呢？（设NPC的最大观察距离为d，则能看到点x的条件之一为$|\vec{PX}| \le d$，结合第3条的结果，两个条件都满足才能看到，即$|\vec{PX}| \le d 且 \frac{\vec{PX}\cdot\vec v}{|\vec{PX}||\vec v|} > \cos\frac{\phi}{2}$时才能看到点x。）
7. 在渲染中我们常常会需要判断一个三角面片是正面还背面，这可以通过判断三角形的3个顶点在当前空间中是顺时针还是逆时针排列来得到。给定三角形的3个顶点$p_1,p_2,p_3$，如何利用叉积来判断这3个顶点的顺序是顺时针还是逆时针？假设我们使用的是左手坐标系，且$p_1,p_2,p_3$都位于xy平面，人眼位于z轴的负方向上，向z轴正方向观察，如图4.29所示。

![图4.29](images/chapter04_check_triangle_front_or_back_by_cross_product.png)

答：取向量 $\vec a = (\vec p_2 - \vec p_1), \vec b = (\vec p_3 - \vec p_1)$，求$\vec a \times \vec b$的结果。如果叉积结果向量的z分量大于0，说明是顺时针。如果小于0，说明是逆时针。如果为0，说明这三个点共线，无法构成三角形。

## 4.4 矩阵

一个$3 \times 3$的矩阵，可以写成如下形式：

$$
\boldsymbol{M} =
\left[
\begin{matrix}
   m_{11} & m_{12} & m_{13} \\
   m_{21} & m_{22} & m_{23} \\
   m_{31} & m_{32} & m_{33}
\end{matrix}
\right]
$$

$m_{ij}$表明了这个元素在矩阵$\boldsymbol{M}$的第$i$行、第$j$列。
矩阵有行矩阵和列矩阵。

### 4.4.2 矩阵与矢量的关系

矢量可以看成是$n \times 1$的列矩阵（column matrix），或者是 $1 \times n$的行矩阵（row matrix），其中n对应了矢量的维度。例如$\vec v = (x,y,z)$可以写成行矩阵$\left[ \begin{matrix} x & y & z \end{matrix} \right]$，或列矩阵$\left[ \begin{matrix} x \\ y \\ z \end{matrix} \right]$。

通过将矢量表达成矩阵，可以让矢量像一个矩阵一样一起参与距阵运算。

### 4.4.3 矩阵运算

#### 4.4.3.1 矩阵与标量的乘法

矩阵与标量相乘，它的结果仍然是一个相同维度的矩阵，原矩阵的每个元素分别与该标量相乘。

$$
k\boldsymbol{M} = \boldsymbol{M}k = k
\left[
\begin{matrix}
   m_{11} & m_{12} & m_{13} \\
   m_{21} & m_{22} & m_{23} \\
   m_{31} & m_{32} & m_{33}
\end{matrix}
\right] =
\left[
\begin{matrix}
   km_{11} & km_{12} & km_{13} \\
   km_{21} & km_{22} & km_{23} \\
   km_{31} & km_{32} & km_{33}
\end{matrix}
\right]
$$

#### 4.4.3.2 矩阵与矩阵相乘

设有$r \times n$的矩阵$\boldsymbol{A}$和一个$n \times c$的矩阵$\boldsymbol{B}$，它们相乘会得到一个$r \times c$的矩阵$\boldsymbol{C} = \boldsymbol{AB}$。那么$\boldsymbol{C}$中的每一个元素$c_{ij}$等于$\boldsymbol{A}$的第$i$行所对应的矢量和$\boldsymbol{B}$的第$j$列所对应的矢量进行矢量点乘的结果，即

$$
c_{ij} = a_{i1}b_{1j} + a_{i2}b_{2j} + \cdots + a_{in}b_{nj} = \sum\limits_{k=1}^{n}{a_{ik}b_{kj}}
$$

矩阵乘法满足一些性质。

##### 4.4.3.2.1 矩阵乘法不满足交换律

通常情况下，$\boldsymbol{AB}\neq\boldsymbol{BA}$

##### 4.4.3.2.2 矩阵乘法满足结合律

$(\boldsymbol{AB})\boldsymbol{C} = \boldsymbol{A}(\boldsymbol{BC})$

### 4.4.4 特殊的矩阵

#### 4.4.4.1 方块矩阵

方块矩阵（square matrix），简称方阵，是指那些行和列数目相等的矩阵。

方阵的对角元素是指行号和列号相等的元素，例如$m_{11}, m_{22}, m_{33}$等。

如果一个矩阵除了对角元素外的所有元素都为0，那么这个矩阵就叫做对角矩阵（diagonal matrix）。

#### 4.4.4.2 单位矩阵

一个特殊的对角矩阵是单位矩阵（identity matrix），用$\boldsymbol{I}_n$来表示。一个$3 \times 3$的单位矩阵如下：

$$
\boldsymbol{I}_3 =
\left[
\begin{matrix}
   1 & 0 & 0 \\
   0 & 1 & 0 \\
   0 & 0 & 1
\end{matrix}
\right]
$$

任何矩阵与单位矩阵相乘的结果为原来的矩阵，即：

$\boldsymbol{MI} = \boldsymbol{IM} = \boldsymbol{M}$

#### 4.4.4.3 转置矩阵

转置矩阵（transposed matrix）实际是对原矩阵的一种运算，即转置运算。给定一个$r \times c$的矩阵$\boldsymbol{M}$，它的转置表示成$\boldsymbol{M}^T$，这是一个$c \times r$的矩阵。原矩阵的第$i$行变成第$i$列，而第$j$列变成第$j$行。

$\boldsymbol{M}_{ij}^T = \boldsymbol{M}_{ji}$


可以使用转置操作来转换行列矩阵：

$$
\left[
\begin{matrix}
   x & y & z
\end{matrix}
\right]^T = 
\left[
\begin{matrix}
   x \\
   y \\
   z
\end{matrix}
\right]
$$

$$
\left[
\begin{matrix}
   x \\
   y \\
   z
\end{matrix}
\right]^T = 
\left[
\begin{matrix}
   x & y & z
\end{matrix}
\right]
$$

##### 4.4.4.3.1 矩阵的转置的转置等于原矩阵

$(\boldsymbol{M}^T)^T = \boldsymbol{M}$

##### 4.4.4.3.2 矩阵串接的转置，等于反向串接各个矩阵的转置

$(\boldsymbol{AB})^T = \boldsymbol{B}^T\boldsymbol{A}^T$

#### 4.4.4.4 逆矩阵

只有方阵才有逆矩阵。

给定一个方阵$\boldsymbol{M}$，它的逆矩阵用$\boldsymbol{M}^{-1}$来表示。逆矩阵与原矩阵相乘的结果为单位矩阵。即：

$\boldsymbol{MM}^{-1} = \boldsymbol{M}^{-1}\boldsymbol{M} = \boldsymbol{I}$

并非所有的方阵都有对应的逆矩阵。如果一个矩阵有对应的逆矩阵，我们就说这个矩阵是可逆的（invertible）或者说是非奇异的（nonsingular）；相反的，如果一个矩阵没有对应的逆矩阵，我们就说它是不可逆的（noninvertible）或者说是奇异的（singular）。

##### 4.4.4.4.1 逆矩阵的逆矩阵是原矩阵本身

假设矩阵$\boldsymbol{M}$是可逆的，那么
$$
(\boldsymbol{M}^{-1})^{-1} = \boldsymbol{M}
$$

##### 4.4.4.4.2 单位矩阵的逆矩阵是它本身

$$
\boldsymbol{I}^{-1} = \boldsymbol{I}
$$

##### 4.4.4.4.3 转置矩阵的逆矩阵是逆矩阵的转置

$$
(\boldsymbol{M}^T)^{-1} = (\boldsymbol{M}^{-1})^T
$$

##### 4.4.4.4.4 矩阵串接相乘后的逆矩阵等于反向串接各个矩阵的逆矩阵

$$
(\boldsymbol{AB})^{-1} = \boldsymbol{B}^{-1}\boldsymbol{A}^{-1}
$$

#### 4.4.4.5 正交矩阵

如果一个矩阵$\boldsymbol{M}$和它的转置矩阵的乘积是单位矩阵的话，则称这个矩阵是正交的（orthogonal）。

$$
矩阵\boldsymbol{M}是正交的 \leftrightarrow \boldsymbol{MM}^T = \boldsymbol{M}^T\boldsymbol{M} = \boldsymbol{I} \leftrightarrow \boldsymbol{M}^T = \boldsymbol{M}^{-1}
$$

### 4.4.5 行矩阵还是列矩阵

这里的行矩阵还是列矩阵指的是矢量转换成的矩阵。即$\vec v = (x,y,z)$既可以转换成行矩阵$\left[\begin{matrix} x & y & z \end{matrix}\right]$，也可以转换成列矩阵$\left[\begin{matrix} x \\ y \\ z \end{matrix}\right]$。

设矩阵$\boldsymbol{M}$为

$$
\boldsymbol{M} = 
\left[\begin{matrix}
   m_{11} & m_{12} & m_{13} \\
   m_{21} & m_{22} & m_{23} \\
   m_{31} & m_{32} & m_{33}
\end{matrix}\right]
$$

受矩阵乘法的行列数要求所限，行矩阵与$\boldsymbol{M}$相乘只能是$\vec v\boldsymbol{M}$，矢量在左侧，矩阵在右侧。

$$
\vec v\boldsymbol{M} = \left[\begin{matrix} xm_{11}+ym_{21}+zm_{31} & xm_{12}+ym_{22}+zm_{32} & xm_{13}+ym_{23}+zm_{33} \end{matrix}\right]
$$

而如果和列矩阵相乘，则要求矩阵在左侧，矢量在右侧，其结果为：

$$
\boldsymbol{M}\vec v = 
\left[\begin{matrix} xm_{11}+ym_{12}+zm_{13} \\ xm_{21}+ym_{22}+zm_{23} \\ xm_{31}+ym_{32}+zm_{33} \end{matrix}\right]
$$

上述两种结果，除了行列矩阵的区别外，里面的元素也不一样。这就意味着，在把矢量和矩阵相乘时选择行矩阵还是列矩阵来表示矢量是非常重要的，这决定了矩阵乘法的书写次序和结果值。

在unity中，常规做法是把矢量放在矩阵的右侧，即把矢量转换为列矩阵来进行运算。

$$\boldsymbol{CBA}\vec v = (\boldsymbol{C}(\boldsymbol{B}(\boldsymbol{A}\vec v)))$$

使用列向量的结果是，变换的顺序是从右到左，即先对$\vec v$使用$\boldsymbol{A}$进行变换，再使用$\boldsymbol{B}$进行变换，最后使用$\boldsymbol{C}$进行变换。

上面的计算等价于下面的行矩阵运算：

$$\vec v \boldsymbol{A}^T \boldsymbol{B}^T \boldsymbol{C}^T = (((\vec v \boldsymbol{A}^T)\boldsymbol{B}^T)\boldsymbol{C}^T)$$

根据4.4.4.3.2 矩阵串接的转置，等于反向串接各个矩阵的转置，变换可得。

### 4.4.6 练习题

1\. 判断下面矩阵的乘法是否存在。如果存在，计算它们的乘积。
(1) $\left[\begin{matrix} 1 & 3 \\ 2 & 4 \end{matrix}\right]\left[\begin{matrix} -1 & 5 \\ 0 & 2 \end{matrix}\right]$
答：第一个矩阵的列数等于第二个矩阵的行数，所以它们的乘法存在。其乘积为
$\left[\begin{matrix} -1 & 11 \\ -2 & 18 \end{matrix}\right]$

(2) $\left[\begin{matrix} 2 & 4 & 3 \\ 2 & 1 & 4 \end{matrix}\right]\left[\begin{matrix} -1 & 5 \\ 0 & 2 \\ 3 & 10 \end{matrix}\right]$
答：第一个矩阵的列数等于第二个矩阵的行数，所以它们的乘法存在，其乘积为
$\left[\begin{matrix} 7 & 48 \\ 10 & 52 \end{matrix}\right]$

(3) $\left[\begin{matrix} 1 & -2 & 3 \\ 5 & 1 & 4 \\ 6 & 0 & 3 \end{matrix}\right]\left[\begin{matrix} -5 \\ 4 \\ 8 \end{matrix}\right]$
答：第一个矩阵的列数等于第二个矩阵的行数，所以它们的乘法存在，其乘积为
$\left[\begin{matrix} 11 \\ 11 \\ -6 \end{matrix}\right]$

2\. 判断下面的矩阵是否是正交矩阵。
(1) $\left[\begin{matrix} 1 & 0 & 0 \\ 1 & 0 & 0 \\ 1 & 0 & 0 \end{matrix}\right]$
答：因为$\left[\begin{matrix} 1 & 0 & 0 \\ 1 & 0 & 0 \\ 1 & 0 & 0 \end{matrix}\right]\left[\begin{matrix} 1 & 0 & 0 \\ 1 & 0 & 0 \\ 1 & 0 & 0 \end{matrix}\right]^T = \left[\begin{matrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{matrix}\right] \neq \boldsymbol{I}_3$，所以它不是正交矩阵。

(2) $\left[\begin{matrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{matrix}\right]$
答：因为$\boldsymbol{I}_4\boldsymbol{I}_4^T = \boldsymbol{I}_4^T = \boldsymbol{I}_4$，所以它是正交矩阵。

(3) $\left[\begin{matrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right]$
答：因为$\left[\begin{matrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right]\left[\begin{matrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right]^T = \left[\begin{matrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right]\left[\begin{matrix} \cos\theta & \sin\theta & 0 \\ -\sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right] = \left[\begin{matrix} (\cos\theta)(\cos\theta) + (-\sin\theta)(-\sin\theta) + (0)(0) & (\cos\theta)(\sin\theta) + (-\sin\theta)(\cos\theta) + (0)(0) &  (\cos\theta)(0)+(-\sin\theta)(0)+(0)(1) \\ \sin\theta\cos\theta + \cos\theta(-\sin\theta)+(0)(0) & \sin\theta\sin\theta + \cos\theta\cos\theta + (0)(0) & (\sin\theta)(0)+(\cos\theta)(0)+(0)(1) \\ (0)\cos\theta+(0)(-\sin\theta)+(1)(0) & (0)\sin\theta+(0)\cos\theta+(1)(0) & (0)(0)+(0)(0)+(1)(1) \end{matrix}\right] = \left[\begin{matrix} \cos^2\theta+\sin^2\theta & 0 & 0 \\ 0 & \sin^2\theta+\cos^2\theta & 0 \\ 0 & 0 & 1 \end{matrix}\right] = \left[\begin{matrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{matrix}\right] = \boldsymbol{I}_3$，所以它是正交矩阵。

3\. 给定一个矢量(3,2,6)，分别把它当成行矩阵和列矩阵与下面的矩阵相乘。考虑两种情况下得到的矢量结果是否一样。如果不一样，考虑如何得到相同的结果。

(1) $\left[\begin{matrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{matrix}\right]$
答：因为是单位矩阵，所以给定的矢量与其相乘，将会得到本身，即$(3,2,6)$。

(2) $\left[\begin{matrix} 1 & 0 & 2 \\ 0 & 1 & -3 \\ 0 & 0 & 3 \end{matrix}\right]$
答：
矢量作为行矩阵时，计算结果如下：
$\left[\begin{matrix} 3 & 2 & 6 \end{matrix}\right]\left[\begin{matrix} 1 & 0 & 2 \\ 0 & 1 & -3 \\ 0 & 0 & 3 \end{matrix}\right] = \left[\begin{matrix} 3 & 2 & 18 \end{matrix}\right]$
矢量作为列矩阵时，计算结果如下：
$\left[\begin{matrix} 1 & 0 & 2 \\ 0 & 1 & -3 \\ 0 & 0 & 3 \end{matrix}\right]\left[\begin{matrix} 3 \\ 2 \\ 6 \end{matrix}\right] = \left[\begin{matrix} 15 \\ -16 \\ 18 \end{matrix}\right]$
将其中的$3\times 3$矩阵做一个转置，即可得到相同的结果。

(3) $\left[\begin{matrix} 2 & -1 & 3 \\ -1 & 5 & -3 \\ 3 & -3 & 4 \end{matrix}\right]$

答：记给定的矢量为$\boldsymbol{V} = \left[\begin{matrix} 3 & 2 & 6 \end{matrix}\right]$，记给定的矩阵为$\boldsymbol{M}$，那么有
$(\boldsymbol{VM})^T = \boldsymbol{M}^T\boldsymbol{V}^T$
其中的$\boldsymbol{V}^T$即为列矩阵。

观察得$\boldsymbol{M}^T = \boldsymbol{M}$，则有$(\boldsymbol{VM})^T = \boldsymbol{MV}^T$，所以把给定的矢量当成行矩阵和列矩阵与给定的矩阵相乘，得到的结果矢量相等。

## 4.5 矩阵的几何意义：变换

在三维渲染中矩阵可视化的结果就是变换。

### 4.5.1 什么是变换

变换（transform）指的是我们把一些数据，如点、方向矢量甚至是颜色等，通过某种方式进行转换的过程。

线性变换（linear transform）指的是那些可以保留矢量加和标量乘的变换。用数学公式表达即：

$$
f(x) + f(y) = f(x + y)
$$
$$
kf(x) = f(kx)
$$

线性变换包括：缩放（scale）、旋转（rotation）、错切（shear）、镜像（mirroring或reflection）、正交投影（orthographic projection）等。

仅有线性变换是不够的。考虑平移变换，例如$f(x) = x + (1,2,3)$。这个变换就不是一个线性变换，它满足标量乘法，但不满足矢量加法。令$x=(1,1,1)$，那么：
$f(x)+f(x) = (4,6,8)$
$f(x+x) = (3,4,5)$
可以看到两个运算的结果不一样。

仿射变换（affine transform）是合并线性变换和平移变换的变换类型。仿射变换可以使用一个$4 \times 4$的矩阵来表示。为此，需要把矢量扩展到四维空间下，这就是齐次坐标空间（homogeneous space）。

表4.1 常见的变换种类和它们的特性

变换名称 | 是线性变换吗 | 是仿射变换吗 | 是可逆矩阵吗 | 是正交矩阵吗
:-----: | :---------: | :---------: | :---------: | :----------:
平移    | 否           | 是          | 是          | 否
绕坐标轴旋转 | 是 | 是 | 是 | 是
绕任意轴旋转 | 是 | 是 | 是 | 是
按坐标轴缩放 | 是 | 是 | 是 | 否
错切 | 是 | 是 | 是 | 否
镜像 | 是 | 是 | 是 | 是
正交投影 | 是 | 是 | 否 | 否
透视投影 | 否 | 否 | 否 | 否

### 4.5.2 齐次坐标
