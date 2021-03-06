---
title: 离散小波变换
date: 2015-01-01 10:00:00
categories: fbImgT
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

<!--<img src="http://latex.codecogs.com/gif.latex? a^{i}"/>
<center><img src="{{ site.baseurl }}/images/pdBase/svm_smo1.png"></center>-->

### 一维离散小波变换

   从滤波器的观点来看，对于一维小波变换就是把信号分别通过低通滤波器和高通滤波器把原始信号分解为原信号的近似系数和原信号的细节系数两个部分，其物理思想就是去除信号在空间尺度的关联关系，而用数值来表达原始信号。

   通过离散小波变换提取我们感兴趣的特征。离散小波变换可以将原信号分解为信号本身特征（低频部分）和其细节特征（高频部分）两种，而且可以对于分解后得到的信号本身特征（低频部分）也可以通过小波变换再得到该信号的本身特征和该信号的细节特征，这就为我们提供了多分辨率，我们可以根据自己的应用来决定对信号进行几层变换。小波变换的实现算法是 mallat 算法。

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_diswav1.png"></center>

### 二维离散小波变换

   与一维小波变换相似，对于二维小波变换需要通过滤波器来消除信号中的关联，但是二维信号有着横向和纵向两个方向上的联系，故我们在使用滤波器时需要在横向和纵向每个方向上使用两次滤波器，才能消除信号在那两个方向上的联系。通常实现二维DWT采用分离计算方法。分离计算方法基于二维张量积多分辨分析中的二维尺度函数与小波函数的可分离的特点,用一维小波滤波器分别对二维信号(图象)在水平和竖直进行卷积,最后得到所要的结果。

   1988年，S.Mallat利用多分辨率分析的概念,提出了离散小波变换的快速分解与重构算法,即Mallat算法，解决了小波变换计算比较复杂的问题.在实际应用中，对于N×N的图像,需要应用二维离散小波变换。二维离散小波变换的快速算法如下：

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_diswav2.png"></center>

   式中g和h为分解低通和高通小波滤波器； <img src="http://latex.codecogs.com/gif.latex? X_{LL}^j (n_1 ,n_2 )"/>表示能量集中的低频子带，体现灰度变化； <img src="http://latex.codecogs.com/gif.latex? X_{LH}^j (n_1 ,n_2 )"/>为水平低频垂直高频子带,具有水平边缘信息； <img src="http://latex.codecogs.com/gif.latex? X_{HL}^j (n_1 ,n_2 )"/>为水平高频垂直低频子带,具有垂直边缘信息;  <img src="http://latex.codecogs.com/gif.latex? X_{HH}^j (n_1 ,n_2 )"/>为水平高频垂直高频子带,具有对角边缘信息；原始输入信号为 <img src="http://latex.codecogs.com/gif.latex? X_{LL}^j (n_1 ,n_2 )"/>，j为变换级数，<img src="http://latex.codecogs.com/gif.latex? J \le \log _2 N"/> 。

   图中<img src="http://latex.codecogs.com/gif.latex? \downarrow 2"/>表示下2采样,即输出只剩下输入样本数的一半。首先,原始图像N×N经过水平滤波器组分解成低频和高频分量;然后,数据再通过垂直滤波器组,最终分解成为4个子带图像数据(N/2)×(N/2),完成一级变换.变换后的3个高频分量LH,HL和HH直接输出，而低频分量LL，送到下一级滤波器组中进行第二级变换，依次类推，最终完成小波变换。

---

变换举例：若使用haar小波基，按下面图4.2，

1. 先列采样，得到两幅图，一幅保留偶数列，一幅保留奇数列，行不变；   

2. 然后两幅图像做差分，结果保留在其中一幅A，则A为高频（H），另一幅B只参与运算，不改变结果，作为低频（L）；  

3. 对图A，做行采样，得奇偶，差分得高频图像（HH）和低频图像（HL）。   

4. 对图B，做行采样，差分得高频图像（LH）和低频图像（LL），完成一遍变换；  

5. 对低频（LL）作为一幅新的图像，重复上面的变换。

---

PS：

1. 其中LL得到的过程中只是只参与的运算，未改变过自身，即LL只是原图的一个采样图，如下图3左上角所示。
2. 通过采样进行多尺度多分辨率处理，提取的是特征点。如一个三角形，它的角是弯的，直接检测不会被认为是角点，但采样过后，这个弯的角点可能就变成尖的，就会被采集到。（多尺度的作用参考SIFT算法）
3. 用奇偶行（或列）进行差分得到高频特征，是由Haar小波基的定义而来（一上一下，采样后相当于小波基宽度加大，而其他小波基则对应卷积得到高频，不处理为低频？）。对于其他的小波基则不一定一样。
4. 与傅立叶变换不同，小波变换能提取高频分量及其对应的位置信息。

---

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_diswav3.png"></center>

<center><img src="{{ site.baseurl }}/images/pdBase/ImgTrans_diswav4.png"></center>


