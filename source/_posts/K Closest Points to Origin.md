---
title: K个离原点最近的点 
layout: post
date: 2019-07-14 12:18:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
![](/gallery/leetcode-1/KCloestPointResult.JPG)  
<!--more-->  
   
    public static int[][] kClosest(int[][] points, int K) {
       PriorityQueue<int[]> pq = new PriorityQueue<int[]>((p1,p2)->p2[0]*p2[0]+p2[1]*p2[1]-p1[0]*p1[0]-p1[1]*p1[1]);
       for(int[] p: points) {
           pq.offer(p);
           if(pq.size() > K) {
             pq.poll();
           }
       }
        System.out.println("finish pq");
       int [][]res=new int[K][2];
       while(K>0){
           res[--K]=pq.poll();
       }
       return res;
    }

    //print out result
    public static void printp(int[][] points,int K){
        for(int[] p: points){
            System.out.print("("+p[0]+","+p[1]+") ");
        }
    }

这个方法时间复杂度为O(n*logn)，但是它比较稳定，而且可以处理流数据，不需要获得所有数据就可以处理。   

原理就是运营一个大小为K的最小堆heap,里面保存K个距离原点最近的点。  
循环访问每个点，同时把他们放入堆内，一旦堆满了，就推出最小堆的距离原点最大的点。  
循环访问完毕后，就把堆内数据取出输出。  
 
但在leetcode它的性能一般，连80%的对手都打不过。所以需要进行改造，改造的方法是自己重新实现一个简单的堆，不用JAVA内置的堆，也就是PriorityQueue。




