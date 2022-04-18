---
title: Andrew Ng 机器学习笔记(第2周)
date: 2022-04-18 21:54:52
categories: 人工智能
excerpt: 机器学习课程第2周的笔记，本周的内容包括多变量线性回归、解析法计算参数、Octave 语法入门三部分。
index_img: /img/AI/ai_index.jpg
math: true
---

## 前言

这个系列的博客将作为我机器学习课程的笔记，部分用中文较难表述的词句我将使用英文原文。

第 2 周的课程标题是多变量梯度下降，包括多变量线性回归、解析法计算参数、Octave 语法入门三部分。

## 多变量线性回归

多变量线性回归的一些**公式**：

- **待定函数**为：$$H(X)=\Theta^{T}X$$其中，$\Theta$向量为待定系数，$X$向量为自变量

- **梯度下降公式**仍为：$$\Theta:=\Theta-\alpha\frac{\partial}{\partial\Theta}J(\Theta)$$其中，$:=$为赋值符号，$J$为代价函数，$\Theta$向量为$J$的参数，$\Theta$向量也是待定函数$H$的待定系数，$\alpha$为学习率

多变量线性回归的一些 **Trick**：

- 对于一些数据范围较大的特征，可以采用**特征缩放**将其归一化，加速梯度下降的收敛

- 学习率过大可能导致梯度下降不收敛，学习率过小则梯度下降收敛较慢

- 当线性回归拟合数据效果不好时，可以采用多项式回归

## 解析法计算参数

对于多变量线性回归，可以解析地计算出最优的$\Theta$向量，公式为：$$\Theta=(X^{T}X)^{-1}X^{T}y$$其中，$X$矩阵是$m \times (n+1)$的矩阵，每一行代表一个样本数据。$X$矩阵第 $1$ 列的元素均为 $1$，第$(n+1)$列表示第$n$个特征。$y$向量是列向量，$y$向量的第$n$个元素表示第$n$个样本对应的真实输出。

多变量线性回归问题，梯度下降法的时间复杂度是$O(kn^{2})$($k$为迭代次数)，而解析法计算的时间复杂度是$O(n^3)$。

若使用解析法计算参数时，发现矩阵$X^TX$不可逆，则可能发生了以下情况：

1. 特征重复，可能存在两个线性相关的特征，可以考虑消除这种线性相关性

2. 特征太多了($m \le n$)，可以考虑删除一些特征

## Octave/Matlab 入门

### Octave/Matlab 的基础操作

- 算术运算：`+(加)`, `-(减)`, `*(乘)`, `/(除)`, `^(幂)`

- 数值比较：`==(等于)`, `~=(不等于)`, `>(大于)`, `<(小于)`, `>=(大于等于)`, `<=(小于等于)`

- 逻辑运算：`&&(逻辑与)`, `||(逻辑或)`, `xor(逻辑异或)`

- 一段代码示例：

  ```matlab
  a = pi; % 这是注释
  b = 'hi'; % 在语句后加;可以阻止REPL输出
  a % 输出a，也可使用disp(a)
  disp(sprintf('2 decimals: %0.2f', a)) % 输出2位小数
  format long % 设置REPL输出更长的数据
  ```

- 矩阵和向量的表示：

  ```matlab
  A = [1 2;3 4;5 6] % 定义一个3行2列的矩阵
  v = [1;2;3] % 定义一个列向量
  v = 1:0.1:2 % 定义一个行向量，行向量的元素为1, 1.1, 1.2, ..., 2
  v = 1:6 % 定义一个行向量，行向量的元素为1, 2, 3, ..., 6
  C = 2*ones(2,3) % 定义一个2行3列的全2矩阵
  w = zeros(1,3) % 定义一个1行3列的全0矩阵
  w = rand(1,3) % 定义一个1行3列的矩阵，元素均为0到1之间的随机数
  w = -6 + sqrt(10)*(randn(1,10000))
  hist(w,50) % 绘制w的直方图，50为直方图的组数
  I = eye(4) % 定义一个4阶单位矩阵
  ```

- 帮助命令：`help`

### Octave/Matlab 的数据处理

- 查看矩阵/向量的维数：

  ```matlab
  A = [1 2; 3 4; 5 6]
  size(A) % 输出矩阵A的大小，结果为[3 2]
  size(A,1) % 输出矩阵A第一维的大小，结果为3
  v = [1 2 3 4]
  length(v) % 输出向量v的长度，结果为4
  length(A) % 输出矩阵A第一维的大小，结果为3
  ```

- 工作目录操作：

  ```matlab
  pwd % 输出当前目录
  cd 'C:\Users\Administrator\Desktop' % 切换到桌面
  ls % 显示当前目录下的文件
  mkdir 'test' % 在当前目录下创建一个文件夹
  rmdir 'test' % 删除当前目录下的文件夹
  ```

{% note secondary %}
.dat 后缀的文件一般为数据文件，该后缀被多种软件所使用，每个软件定义的数据内容很可能不一样。下文中，.dat 文件的内容为以空格分隔的数字，每行代表一个样本，每列代表一个特征。
{% endnote %}

- 数据载入相关操作：

  ```matlab
  A = load('data.dat') % 从data.dat文件中载入数据，结果为一个矩阵
  who % 输出当前会话的所有变量名
  whos % 输出当前会话的所有变量的详细信息
  clear A % 删除变量A
  v = A(1:10,1:2) % 取出矩阵A的第1行到第10行，第1列到第2列的元素
  save hello.mat v % 保存变量v到hello.mat文件中，该文件为二进制文件
  clear % 删除所有变量
  save hello.txt v -ascii % 保存变量v到hello.txt文件中，-ascii表示以ASCII码保存
  ```

- 矩阵/向量切片操作

  ```matlab
  A(3,2) % 输出矩阵A的第3行第2列的元素
  A(2,:) % 输出矩阵A的第2行的所有元素
  A([1 3],:) % 输出矩阵A的第1行和第3行的所有元素
  A(:,2) = [10; 11; 12] % 将矩阵A的第2列赋值为10, 11, 12
  A = [A, [100; 101; 102]] % 将列向量[100; 101; 102]添加到矩阵A的最后一列
  A(:) % 将矩阵A转换为列向量
  A = [1 2; 3 4]
  B = [5 6; 7 8]
  C = [A B] % 将矩阵A和B按行拼接，结果为[1 2 5 6; 3 4 7 8]，等价于[A, B]
  C = [A; B] % 将矩阵A和B按列拼接，结果为[1 2; 3 4; 5 6; 7 8]
  ```

### Octave/Matlab 的矩阵运算

- 矩阵/向量的基本运算(一)：

  ```matlab
  A = [1 2; 3 4; 5 6]
  B = [11 12; 13 14; 15 16]
  C = [1 1; 2 2]
  A * C % 矩阵A和矩阵C的乘积，结果为[5 5; 11 11; 17 17]
  A .* B % 矩阵A和矩阵B对应位置的元素相乘，结果为[11 24; 39 56; 75 96]
  A .^ 2 % 矩阵A每个位置的元素取平方，结果为[1 4; 9 16; 25 36]
  v = [1; 2; 3]
  1 ./ v % 向量v每个位置的元素取倒数，结果为[1.0000; 0.5000; 0.3333]
  log(v) % 向量v每个位置的元素取自然对数，结果为[0.0000; 0.6931; 1.0986]
  exp(v) % 向量v每个位置的元素取指数(以e为底数)，结果为[2.7183; 7.3891; 20.0855]
  abs(v) % 向量v每个位置的元素取绝对值，结果为[1.0000; 2.0000; 3.0000]
  -v % 向量v每个位置的元素取负，结果为[-1.0000; -2.0000; -3.0000]
  v + 1 % 向量v每个位置的元素加1，结果为[2.0000; 3.0000; 4.0000]
  ```

- 矩阵/向量的基本运算(二)：

  ```matlab
  A' % 矩阵A的转置，结果为[1 3 5; 2 4 6]
  a = [1 15 2 0.5]
  val = max(a) % 向量a中的最大值，结果为15
  [val, ind] = max(a) % 向量a中的最大值和最大值的索引，结果为[15, 2]
  max(A) % 矩阵A每一列的最大值，结果为[5 6]
  a < 3 % 向量a中的每个元素是否小于3，结果为[1 0 1 1]
  find(a < 3) % 找出向量a中每个小于3的元素的索引，结果为[1 3 4]
  A = magic(3) % 生成一个3阶幻方，结果为[8 1 6; 3 5 7; 4 9 2]
  [r,c] = find(A >= 7) % 找到矩阵A中每个大于等于7的元素的行和列索引，r(行索引)为[1 3 2]，c(列索引)为[1 2 3]
  sum(a) % 向量a中的元素求和，结果为18.5
  prod(a) % 向量a中的元素求乘积，结果为15
  floor(a) % 向量a中的元素向下取整，结果为[1 15 2 0]
  ceil(a) % 向量a中的元素向上取整，结果为[1 15 2 1]
  ```

- 矩阵/向量的基本运算(三)：

  ```matlab
  rand(3) % 生成一个三阶方阵，方阵中的每一个元素为0到1之间的随机数
  max(rand(3), rand(3)) % 生成两个三阶方阵，并对对应位置的元素求最大值
  max(A,[],1) % 矩阵A每一列的最大值，结果为[8 9 7]
  max(A,[],2) % 矩阵A每一行的最大值，结果为[8; 7; 9]
  max(max(A)) % 矩阵A中的最大值，结果为9
  A = magic(9)
  sum(A,1) % 矩阵A每一列的元素求和，结果为[369 369 369 369 369 369 369 369 369]
  flipud(A) % 以水平方向为轴翻转矩阵A
  pinv(A) % 矩阵A的逆，pinv(A)*A的结果近似等于eye(9)
  ```

### Octave/Matlab 的绘图

- Matlab/Octave 绘图(一)：

  ```matlab
  t=[0:0.01:0.98]
  y1=sin(2*pi*4*t)
  plot(t,y1) % 绘制y1的曲线，颜色默认为蓝色
  hold on % 在当前绘图区域继续绘图，不清除当前绘图区域
  y2=cos(2*pi*4*t)
  plot(t,y2,'r') % 绘制红色的曲线
  xlabel('time') % 设置x轴的标签
  ylabel('value') % 设置y轴的标签
  legend('sin','cos') % 设置图例，legend('sin','cos')表示在图例中显示两条曲线的名称
  title('my plot') % 设置标题
  cd 'C:\Users\Matrix53\Desktop' % 切换到桌面
  print -dpng 'myPlot.png' % 保存当前绘图区域为一个png文件
  close % 关闭当前绘图区域
  ```

  绘制结果如下：
  <img src="/img/AI/plot_1.png" alt="绘制两条曲线" width="50%"/>

- Matlab/Octave 绘图(二)：

  ```matlab
  figure(1) % 切换到第一个绘图区域
  plot(t,y1) % 在第一个绘图区域绘制y1
  figure(2) % 切换到第二个绘图区域
  plot(t,y2) % 在第二个绘图区域绘制y2
  ```

  绘制结果如下：
  <img src="/img/AI/plot_2.png" alt="第一个绘图窗口" width="50%"/>
  <img src="/img/AI/plot_3.png" alt="第二个绘图窗口" width="50%"/>

- Matlab/Octave 绘图(三)：

  ```matlab
  subplot(1,2,1) % 将绘图区域分为 1×2 的矩阵，准备在第一个区域画图
  plot(t,y1) % 在第一个绘图区域绘制y1
  subplot(1,2,2) % 将绘图区域分为 1×2 的矩阵，准备在第二个区域画图
  plot(t,y2) % 在第二个绘图区域绘制y2
  ```

  绘制结果如下：
  ![绘图窗口的切分](/img/AI/plot_4.png)

- Matlab/Octave 绘图(四)：

  ```matlab
  axis([0.5 1 -1 1]) % 将横轴的范围设置为[0.5,1]，将纵轴的范围设置为[-1,1]
  clf % 清空当前绘图区域
  A = magic(5)
  imagesc(A) % 绘制矩阵A的热力图
  colorbar % 显示每个颜色与值的对应关系
  colormap(gray) % 将热力图显示为灰度图，这三条指令也可写为`imagesc(A), colorbar, colormap gray`一行
  ```

  绘制结果如下：
  <img src="/img/AI/plot_5.png" alt="灰度图的显示" width="50%"/>

### Octave/Matlab 的流程控制

- Matlab/Octave 的循环语句：

  ```matlab
  % 初始化 10×1 的零向量
  v = zeros(10,1)
  % for 循环的基础语法
  for i=1:10,
    v(i)=2^i;
  end;
  % while 循环的基础语法
  i = 1;
  while i<= 5,
    v(i) = 100;
    i = i+1;
    break;
  end;
  ```

- Matlab/Octave 的分支语句：

  ```matlab
  % 初始化 a
  a = 3;
  % if 语句的基础语法
  if a==3,
    disp('a is 3') % 末尾可以加;
  elseif a==4,
    disp('a is 4')
  else
    disp('a is not 3 or 4')
  end;
  ```

- Matlab/Octave 的函数定义：

  {% note secondary %}
  Matlab/Octave 一般将函数定义在单独的文件中，暴露出的函数名必须与文件名一致。
  
  Matlab/Octave 的 REPL 存在搜索路径的概念，REPL 可以使用搜索路径下`.m`文件中的函数，搜索路径包括`pwd`。

  另外，还可使用匿名函数、inline命令等来定义函数，下文的函数定义均在`.m`文件中进行。
  {% endnote %}

  ```matlab
  % squareThisNumber.m
  function [y] = squareThisNumber(x)
    y = x^2;
  end
  % REPL
  addpath('path/to/squareThisNumber.m') % 将函数添加到搜索路径
  squareThisNumber(3) % 调用函数，结果为9
  % squareAndCubeThisNumber.m
  function [y1,y2] = squareAndCubeThisNumber(x)
    y1 = x^2;
    y2 = x^3;
  end
  % REPL
  addpath('path/to/squareAndCubeThisNumber.m') % 将函数添加到搜索路径
  [a,b] = squareAndCubeThisNumber(3) % 调用函数，a=9,b=27
  ```

### Tips: 将计算向量化

尚待施工

## 参考资料

- Coursera：Andrew Ng[机器学习课程](https://www.coursera.org/learn/machine-learning)
- 知乎：[常用聚类算法](https://zhuanlan.zhihu.com/p/104355127)

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)
