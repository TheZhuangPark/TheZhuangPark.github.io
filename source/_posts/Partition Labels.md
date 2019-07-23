---
title: 字符串分块 Partition Labels
layout: post
date: 2019-07-16 19:21:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
    - string
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
![](/gallery/leetcode-1/PartitionLabelsResult.JPG)  
<!--more-->  
 
		 public List<Integer> partitionLabels(String S) {
        if(S.length()==0||S==null)return null;
         List<Integer>res =new ArrayList<>();
         Map<Character,Integer> map=new HashMap<Character,Integer>(26);
         for(int i=0;i<S.length();i++)map.put(S.charAt(i),i); //record last index
        int start=0; int end=0; int last=0;    //record target substring
         for(int j=0;j<S.length();j++){
             last=Math.max(last,map.get(S.charAt(j)));
             if(last==j){
                 res.add(last-start+1);
                 start=last+1;
             }
         }return res;
     }

问题:给一个String，切割它，达到每个分段，里面出现该种类的字母所有个数。 比如abcscauef  
分割后 abcsca uef

算法： 这里就是先遍历一遍，把该字符串的某个字母的最后位置记录。  
然后后再遍历一遍，判断到目前位置出现过的所有字母，谁的最后位置更大，得到最大最后位置。 
如果恰好此最大最后位置等于遍历位置J，那就是分割的时候了。  
别用hashMap,改用26元素数组会快150%

模板：就是遍历两遍，第一遍记录最后位置，第二遍计算每个位置的最大最后位置，然后分割而已没有什么模板。

时间复杂度：O(n)
空间复杂读: O(n)
这个不能处理流数据  


