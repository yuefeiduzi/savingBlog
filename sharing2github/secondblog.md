---
title: 源于疑惑，解于异或
date: 2017-09-14 12:31:14
tags:  
	- ^ 
	- 异或 
	- 数字交换 


---


### 写在前面

>不想划水的学生，不是好学生。<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;--鲁迅

### 正文
&emsp;&emsp;今天看到一条老题目，排序

   > 获取一个包含N个整型数字元素的数组，从小到大排序

&emsp;&emsp;本来也就是平常题目，比较完交换元素，
	
		x[i]=x[i]+x[j];
		x[j]=x[i]-x[j];
		x[i]=x[i]-x[j];

&emsp;&emsp;但是写完交换元素时发现存粹加减交换缺点很明显，整型数据溢出，对于32位字符最大表示数字是2147483647，如果是2147483645和2147483646交换就失败了。这时想起之前看到过用有用 ^ 异或处理的方法，计算一番总算写出了出来

		x[i]=x[j]^x[i];
		x[j]=x[i]^x[j];
		x[i]=x[i]^x[j];

&emsp;&emsp;换个好看点的写法

		x[i]^=x[j];
		x[j]^=x[i];
		x[i]^=x[j];

&emsp;&emsp;本着自己的求知欲，又手算了一下
<br>&emsp;&emsp;先回顾一下异或的运算法则:相同为0，不同为1

A输入|符号|B输入|输出
:-:|:-:|:-:|:-:
1|⊕|0|1
1|⊕|1|0
0|⊕|0|0
0|⊕|1|1

&emsp;&emsp;(⊕为“异或”运算符)<br>
&emsp;&emsp;接下来计算

> &emsp;&emsp;A=0x7EH&emsp;DEC:126&emsp;BIN:0111 1110<br>
 &emsp;&emsp;B=0x81H&emsp;DEC:129&emsp;BIN:1000 0001<br>
 &emsp;&emsp;A^B &emsp;&emsp;&emsp;0111 1110<br>
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;^&ensp;1000 0001<br>
 &emsp;&emsp;&emsp;&emsp;&emsp;&ensp;T=&nbsp;1111 1111<br>
 &emsp;&emsp;T=0xFFH&emsp;DEC:255&emsp;BIN:1111 1111<br>
 &emsp;&emsp;T^A &emsp;&emsp;&emsp;1111 1111<br>
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;^&ensp;0111 1110<br>
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;=&nbsp;1000 0001<br>

&emsp;&emsp;答案get√,交换了，上面式子可以等价为<br>&emsp;&emsp;T=A⊕B;&emsp;&emsp;T⊕A=B;&emsp;→&emsp;A⊕B⊕A=B<br>&emsp;&emsp;这时回顾一下[异或](https://baike.baidu.com/item/%E5%BC%82%E6%88%96)定义，以下皆来自百度百科
><br>&emsp;&emsp;异或（xor）是一个数学运算符。它应用于逻辑运算。异或的数学符号为“⊕”，计算机符号为“xor”。<br>&emsp;&emsp;其运算法则为：
a⊕b = (¬a ∧ b) ∨ (a ∧¬b)<br>&emsp;&emsp;<span style="color:red">如果a、b两个值不相同，则异或结果为1。如果a、b两个值相同，异或结果为0。</span>

&emsp;&emsp;根据上面表格1与1异或得0;1与0得1，1对于计算的二进制值起到取反作用<br>&emsp;&emsp;同理对0,0与1异或为1;0与0异或得0，0对于计算的二进制值起到保留作用<br><br>
&emsp;&emsp;回头看上面的手算，A与B逐位异或，如果相同的话就记0，不同则记1，上面A与B每位都不同，所以就得到了全是1的异或结果。
<br>&emsp;&emsp;这里可以理解为：记了0，含义就是同，就是相同；记了1含义就是异，就是不同，这与上面的1与0单独异或时不同的作用完美吻合；<br><br>这时T记的1或者0意义就全了，

* 这一位A与B相同记0，在T与A再次计算时会保留这一位的值→B对应位也是这个值
* 这一位A与B不同记1，在T与A再次计算时会改变这一位的值→正好变为B对应位的值

所以就得到A⊕B⊕A=B

	//todo

<br>&emsp;&emsp;@2017年9月14日

### 关于我
&emsp;&emsp;新生码农<br>&emsp;&emsp;主修java<br>&emsp;&emsp;18届应届生<br>&emsp;&emsp;对于各种技术都超感兴趣