---
title: 两数之和
layout: post
date: 2019-07-03 19:29:00
comments: true
tags: 
    - 算法
categories:  
    - 算法 
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---

![](/gallery/leetcode-1/twosum-result.JPG)  
<!--more-->
    public static int[] twoSum(int[] numbers, int target) {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < numbers.length; i++) {
            // System.out.println("t:"+target+"n["+i+"]:"+numbers[i]);

            if (map.containsKey(target - numbers[i])) {
                result[1] = i + 1;
                result[0] = map.get(target - numbers[i]);
                return result;
            }

           // System.out.println("put key:"+numbers[i]+"value:"+(i+1));
            map.put(numbers[i], i + 1);
        }
        return result;
    }

这里有一个优化点，就是用HashMap存数组里的数字，每访问一个数字Num就用Target-Num, 然后去HashTable找。 找到了，那就把它的位置提取出来，没找到就把这个数字当key，位置当value存到HashMap里去。 就是这个优化点让这个过程只需要O(n)的时间复杂度。

