---
title: 最长无重复字串
layout: post
date: 2019-07-03 19:29:00
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
 ![](/gallery/leetcode-1/LongestSubstringWithoutReapetingResult.JPG)  
<!--more-->  


  
     package com.company;
     import java.util.HashMap;
     import java.util.Map;
     public class Main {

    public static void main(String[] args) {
	// write your code here
       String str="tmmzuxt";
       int result=lengthOfLongestSubstring(str);
       System.out.println("result "+result);
    }
    public static int lengthOfLongestSubstring(String s) {
        int slen=s.length();
        if(slen==0||slen==1)return slen;

        int left=0,right=1,max=1;
        Map<Character,Integer> map= new HashMap<Character,Integer>(slen);
        map.put(s.charAt(0),0);
        for(;right<s.length();right++){
            //System.out.println("IsContain:"+IsContain(map,s.charAt(right)));
            Character cright=s.charAt(right) ;
            if( map.containsKey(cright) ){//Contain
                left=Math.max(map.get(cright)+1,left);
                //System.out.println("left"+left+" right:"+right+"len："+len);
            }
            map.put(cright,right);
            max=Math.max(max,right-left+1);
        }
        return max;
    }
    }

这个方法是在一步一步从暴力破解法慢慢构思出来的，其中涉及到了，包含信息分析，代码优化，和HashMap容量优化。 首先第一步理解题目，题目的意思就是，给定一个字符串 tmmzuxt 它的不重复最长子串为 mzuxt，长度为5。   
    
首先暴力法，暴力法就是，以第一个字符S[0]为起点，开始延长，当发现延长出来的字符串有重复项，就重来，把S[1]开始延长。
这里有两个优化：  
第一就是用HashMap(预设大小）来保存延长的字符，这样寻找重复项很快，时间复杂度O(1）。
第二就是重来的时候，起点重置，不应该是上一个起点S[0]的下一个S[1]，而应该是找到重复项目的位置+1，当作新起点。
