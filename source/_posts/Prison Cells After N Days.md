---
title: N天后的格子 Prison Cells After N Days
layout: post
date: 2019-07-16 12:40:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
    - 哈希表
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
 ![](/gallery/leetcode-1/PrisonCellAfterNDaysResult.JPG)  
<!--more-->  
    public int[] prisonAfterNDays(int[] cells, int N) {
        Map<String, Integer> seen = new HashMap<>();
        while (N > 0) {
            int[] cells2 = new int[8];
            seen.put(Arrays.toString(cells), N--);
            for (int i = 1; i < 7; ++i)
                cells2[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
            cells = cells2;
            if (seen.containsKey(Arrays.toString(cells))) {
                N %= seen.get(Arrays.toString(cells)) - N;
            }
        }//lastseen-curStatc=cycle cur=cur%cycle
        return cells;
    }

问题: 这个问题非常简单，就是[0,x,0] [1,x,1]会让x变成1，[0,x,1],[1,x,0]会让x变成0  
然后计算数组，再N天之后的变化，而且N非常大。  

算法:
这里有cell变化有规则，而且还有大神得出了神奇数字14，也就是变化周期，在十几分钟内得到这个14不太现实。 就是设置一个HashTable 来记录已经看到的模式，以及它出现的天数位置。  
当你现在看到的 current 和 lastseen 一致。 那么N=N%(lastseen_day-current_day)。 比如要变化100天，然后开始倒数,到了90天，发现90天出现的和97天出现的一模一样。 那说明90-7,90-7*2,90-7*3 都是你现在看到的模式。

模板：无  

时间复杂度：不好说。
空间复杂度：O(1)，总共八个格子所以，有64种模式。
流数据：我觉得这个数组是固定的所以没有流数据