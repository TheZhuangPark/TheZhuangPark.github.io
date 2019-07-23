---
title: 雨水积谭 Trapping Rain Water
layout: post
date: 2019-07-17 12:18:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
    - 双指针
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
![](/gallery/leetcode-1/TrapRainWaterResult.JPG)  
<!--more-->  
   
    public static int trap(int[] A) {
        if (A.length < 3) return 0;

        int ans = 0;
        int l = 0, r = A.length - 1;

        // find the left and right edge which can hold water
        while (l < r && A[l] <= A[l + 1]) l++;
        while (l < r && A[r] <= A[r - 1]) r--;

        while (l < r) {
            int left = A[l];
            int right = A[r];
            if (left <= right) {
                // add volum until an edge larger than the left edge
                while (l < r && left >= A[++l]) {
                    ans += left - A[l];
                   // System.out.println("index:"+l+"left add water"+(left-A[l]));
                }
            } else {
                // add volum until an edge larger than the right volum
                while (l < r && A[--r] <= right) {
                    ans += right - A[r];
                   // System.out.println("index:"+r+"right add water"+(right-A[l]));
                }
            }//System.out.println("finish!");
        }
        return ans;
    }
问题：
就是给定一个数组当砖头，[2,1,2]=[2米高砖，1米高砖，2米高砖] 下完雨之后会有积水，于是变成[2米高砖，1米高砖+1米高水，2米高砖米], 然后给定数组，问下完雨所有积水的数目(本例子[2,1,2]中积水就是1)。    
算法：  
设置一个左指针，一个右指针，让它们分别移动到，最近能乘水的位置。     
然后开始，加水和移动左指针或右指针，直到碰壁为止，接着判断交换指针到左或右。  
这种双指针高效的原因是，左右指针都只需要移动遍历一次，然后在满足条件的array[left],array[right]进行计算。  
一般模板都如下：
 		
	 int l = 0, r = A.length - 1;
	 //移动左右指针到满足条件的位置开始，即初始化，可能不用。
     while (l < r && A[l] <= A[l + 1](条件) ) l++;
     while (l < r && A[r] <= A[r - 1](条件) ) r--;
     while (l < r) { //在交换条件下，交替移动左，或者右指针
            int left = A[l]; int right = A[r];
            if (left <= right(交换条件) ) {
                while (l < r && left >= A[++l](移动条件) ) {
                    ans += left - A[l]; (代码)
                }
            } else {
                while (l < r && A[--r] <= right(移动条件) ) {
                    ans += right - A[r];(代码) 
                }
            }
        }
        return ans;
  
时间复杂度为O(n)：左右指针遍历的和为整个数组   
空间复杂度为O(1)：只需要存左指针，右指针，和int类型 res    
不能处理stream data,流数据，因为它需要提前知道整个数据的全貌。  
   

