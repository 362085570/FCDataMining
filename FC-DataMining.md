# FC-DataMining

第一步 注册github账户

​	已有github账户

第二步 建立XXDataMining库

​	![image-20240423215514050](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240423215514050.png)

第三步 编辑库的信息

​	![image-20240423215636716](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240423215636716.png)

​	Repository name

​	输入仓的名字。

​	Descriptioin（optional）

​	对创建库的描述，可选填。

​	Public、Private

​	Public即为公众的，选了Public即代表你的库代码为公开的，库内的所有内容都会被公开。Private即为私有的，选了Private即代表你的库为私密的。

​	完成以上步骤，点击Create repository按钮，完成库的创建。

第四步 提交ppt和markdown

​	![image-20240423220145690](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240423220145690.png)

基于OpenCV识别手写数字

​	假设我们要让程序识别图 中上方的数字。识别的方式是，依次计算该数字图像（即写有数字的图像）与下方数字图像的距离，与哪个数字图像的距离最近（此时 k =1），就认为它与哪幅图像最像，从而确定这幅图像中的数字是多少。

​	![image-20240424163138720](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163138720.png)

特征值提取
步骤 1：我们把数字图像划分成很多小块，如图 所示。该图中每个数字被分成 5 行 4列，共计 5×4 = 20 个小块。此时，每个小块是由很多个像素点构成的。
将这些小块表示为 B（Bigger），将 B 内的像素点，记为 S（Smaller）。因此，待识别的数字“8”的图像可以理解为：由 5 行 4 列，共计 5×4=20 个小块 B 构成。
每个小块 B 内其实是由 M×N 个像素（更小块 S）构成的。假设每个小块大小为 10×10 =100 个像素。

​	![image-20240424163302773](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163302773.png)

步骤 2：计算每个小块 B 内有多少个更小块 S 是黑色的。以数字“8”的图像为例，其第 1 行中：

第 1 个小块 B 共有 0 个像素点（更小块 S）是黑色的，记为 0。
第 2 个小块 B 共有 28 个像素点（更小块 S）是黑色的，记为 28。
第 3 个小块 B 共有 10 个像素点（更小块 S）是黑色的，记为 10。
第 4 个小块 B 共有 0 个像素点（更小块 S）是黑色的，记为 0。
以此类推，计算出数字“8”的图像中每一个小块 B 中有多少个像素点是黑色的，如图 所示。不同的数字图像中每个小块 B 内黑色像素点的数量是不一样的。正是这种不同，使我们能用该数量（每个小块 B 内黑色像素点的个数）作为特征来表示每一个数字。

​	![image-20240424163447404](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163447404.png)

步骤 3：为了处理上的方便，我们会把得到的特征值排成一行（写为数组形式），如图所示。

​	![image-20240424163538854](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163538854.png)

​	经过上述处理，数字“8”图像的特征值变为一行数字，如图所示。

​	![image-20240424163634596](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163634596.png)

步骤 4：与数字“8”的图像类似，每个数字图像的特征值都可以用一行数字来表示。从某种意义上来说，这一行数字类似于我们的身份证号码，一般来说，具有唯一性。按照同样的方式，获取每个数字图像的特征值，如图所示。

​	![image-20240424163726040](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163726040.png)

数字识别
数字识别要做的就是比较待识别图像与图像集中的哪个图像最近。这里，最近指的是二者之间的欧氏距离最短。假设要识别的图像为图中上方的数字“8”图像，需要判断该图像到底属于图 20-8 中下方的数字“8” 图像的分类还是数字“7”图像的分类。

​	![image-20240424163927935](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424163927935.png)

步骤 1：提取特征值，分别提取待识别图像的特征值和特征图像的特征值。

每个数字图像只提取 4 个特征值（划分为 2×2 = 4 个子块 B），如图所示。此时，提取到的特征值分别为：

​	待识别的数字“8”图像：[3, 7, 8, 13]
​	数字“8”特征图像：[3, 6, 9, 12]
​	数字“7”特征图像：[8, 1, 2, 98]
​	![image-20240424164056922](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164056922.png)

步骤 2：计算距离。计算待识别图像与特征图像之间的欧式距离。

首先，计算待识别的数字“8”图像与下方的数字“8”特征图像之间的距离，如图所示。计算二者之间的距离：

​	![image-20240424164221058](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164221058.png)

接下来，计算待识别的数字“8”图像与数字“7”特征图像之间的距离，如图所示。二者之间的距离为：

​	![image-20240424164257409](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164257409.png)

步骤 3：识别。根据计算的距离，待识别的数字“8”图像与数字“8”特征图像的距离更近。所以，将待识别的数字“8”图像识别为数字“8”特征图像所代表的数字“8”。

通过一个案例来展示手写数字识别

在本例中，0~9 的每个数字都有 10 个特征值。例如，数字“0”的特征值如图所示。

​	![image-20240424164621963](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164621963.png)

数据初始化：对程序中要用到的数据进行初始化。涉及的数据主要有路径信息、图像大小、特征值数量、用来存储所有特征值的数据等。

​	![image-20240424164752084](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164752084.png)

读取特征图像：本步骤将所有的特征图像读入到 a 中。共有 10 个数字，每个数字有 10 个特征图像，采用嵌套循环语句完成读取。

​	![image-20240424164833237](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164833237.png)

提取特征图像的特征值：在提取特征值时，可以计算每个子块内黑色像素点的个数，也可以计算每个子块内白色像素点的个数。按照上述思路，图像映射到特征值的关系如图所示。

​	![image-20240424164944808](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424164944808.png)

![image-20240424165013622](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424165013622.png)

计算待识别图像的特征值：读取待识别图像，然后计算该图像的特征值。

​	![image-20240424165116065](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424165116065.png)

计算待识别图像与特征图像之间的距离：依次计算待识别图像与特征图像之间的距离。

​	![image-20240424165200798](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424165200798.png)

获取k个最短距离及其索引：从计算得到的所有距离中，选取 k 个最短距离，并计算出这 k 个最短距离对应的索引。具体实现方式是：

每次找出最短的距离（最小值）及其索引（下标），然后将该最小值替换为最大值。
重复上述过程 k 次，得到 k 个最短距离对应的索引。
每次将最小值替换为最大值，是为了确保该最小值在下一次查找最小值的过程中不会再次被找到。
	![image-20240424165240863](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424165240863.png)

识别：根据计算出来的 k 个最小值的索引。

​	![image-20240424165339158](C:\Users\36208\AppData\Roaming\Typora\typora-user-images\image-20240424165339158.png)

