---
title: leetcode 总结1
layout: post
date: 2019-06-06 02:21:00
comments: true
tags: 
    - 算法
categories: 
    - 算法
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/leetcode1.JPG
---

刷了几道leetcode。  
84-Largest Rectangle in Histogram    
91-Decode Ways  
322-Coin_Change  
152-Maximum Product Subarray  
<!--more-->  
84 Largest Rectangle in Histogram      
首先这个问题，非常好理解，就是给你一串数组比如
[2,1,5,6,2,3]来表达  
![](/gallery/leetcode-1/Rect.jpg)
然后找到最大的矩形面积   
![](/gallery/leetcode-1/Rect_max.JPG)    
然后就通过两个数组，leftarray,rightarray,和一个循环来解决这个问题。 先初始化leftarray, righarray，然后选定好一个矩形，然后开始向左延申，如果遇到更矮的则停下，如果遇到比自己高的则继续，最终确定左边界为止，然后右边同理，确定左右边界之后，就是右边界-左边界=宽。 面积=高 X 宽。 然后选择一个最大的面积就完成了。  

91 Decode Ways   
首先这个问题，就是解码的意思，1-A， 2-B，3-C ...etc   
然后如果给数字226，则是（1）2-2-6（BBF） （2）22-6  （3)2-26 这三个可能性。   
这里用的是递归，这里的递归有点像一棵树。  
![](/gallery/leetcode-1/tree1.JPG)  
然后得到我们要的答案，当到达每个叶子节点的时候都给答案加1。  
还有遇到0的情况，则马上把这个树的分支cut掉。  
![](/gallery/leetcode-1/tree2.JPG)  
这就可以完成了。 基本上代码就是，确定cut的情况，还有达到叶子节点的情况，然后检查递归调用的条件，符合则调用，不符合则不调用。 这里数字需要低于等于26才可以调用递归函数。
  

322 Coin_Change  
这个问题，给一个amount=11, coins=[1,2,5] 结果是3，11=5+5+1, 就是用最少的硬币凑amount。 3个硬币，这是用最少的硬币凑够amount，这里用的就是dynamic program, 而这种还叫last step dynamic program。
有一个数组array, array[0],array[1],array[2].....array[11]它代表着当amount等于11的时候最少用多少个硬币，然后确定一下array[0]=0，array[负数]=-1也就没有答案。 然后array[11] 有三个来源[1,2,5], 也就是[10,9,6],也就是凑够10再来一个1元硬币，凑够9元再来一个2元硬币，凑够6元再来个5元硬币，那么array[11]能多小，取决与array[10],array[9],array[6]的大小了。而array[10],array[9],array[6]你又已经知道了。    
所以会变成一个树状的dp问题，从array[11]往下走到array[0]开始返回。
  
代码结构：1 首先先确定这个递归调用树在哪些叶子结束，在0 负数 以及array[i]（i是叶子) 已经存在的情况下。 2开始循环访问Coin[1,2,5], 让Amount-Coin[i], 然后递归调用F(Amount-Coin[i]), 然后得到返回值进行比较，找到合法的最小的。    
![](/gallery/leetcode-1/tree3.JPG)   

152-Maximum Product Subarray  

这里有个例子，[1,3,4,-1,2,10,-5], 求最大的相乘数字。 有两种情况，第一种就是就是全部都是正数，或者有偶数个负数，负数乘以负数得正。 还有当有奇数个负数的时候，负数相当于一个分割点，把连续正数分割。 比如[1,3,4,-1,2,10] 这里答案只能是[1,3,4]或者[2,10]。 也就是12和20。 
所以需要两个用DP，而且需要两个数组Up,Down数组来存储，然后保存连续, 或者是分割相乘。  
[1, 3, 4, -1, 2, 10, -5]  
[1, 3, 12, -1]
[1, 3, 4, -12]
显示连乘，然后Up Down互换，然后X过于大，则X替换UP。 X过于小，则X替换Down。 最后比较Up和MAX，然后替换。 这里的思想在于，维护多个数组。 然后从底部开始，动态规划。