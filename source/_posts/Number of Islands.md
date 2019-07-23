---
title: 岛的数目
layout: post
date: 2019-07-15 12:40:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
 ![](/gallery/leetcode-1/NumberOfIslandResult.JPG)  
<!--more-->  
    private  int n;
    private  int m;

    public  int numIslands(char[][] grid) {
        int count=0;
        n=grid.length;
        if(n==0)return 0;
        m=grid[0].length;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(grid[i][j]=='1'){
                    DFSMarking(grid,i,j);
                    count++;
                }
            }
        }return count;
    }
    public  void DFSMarking(char[][] grid,int i,int j){
        if(i<0||j<0||i>=n||j>=m||grid[i][j]!='1')return ;
        grid[i][j]='0';
        DFSMarking(grid,i+1,j);
        DFSMarking(grid,i-1,j);
        DFSMarking(grid,i,j+1);
        DFSMarking(grid,i,j-1);
    }

问题：
这个问题相当的简单，[1,1,0,0] 1 1 代表陆地 0 0 代表水。 这说明左边是个小岛。 给定一个地图把岛的数目数出来。

算法：
就是用DFS，循环访问每个地图点，然后以它为中心，拓展开来，遇到陆地则标为水，遇到水跳过，遇到边界跳过。所以当搞定一座岛之后，它就不存在变成水域了，然后开始下一个。 

模板：
   
	 //显示判断地图
	if (grid == null || grid.length == 0 || grid[0].length == 0) return 0;
    	int cnt = 0, m = grid.length, n = grid[0].length;
    //然后循环访问每个地图点
		for (int[] dir : dirs)  DFS(grid, i + dir[0], j + dir[1], m, n); 
	
	//递归
	void DFSMarking(char[][] grid,int i,int j){
		//决定触底的条件
        if(i<0||j<0||i>=n||j>=m||grid[i][j]!='1')return ;
        //数据处理 代码    
		grid[i][j]='0';
	 	//扩展开来
        DFSMarking(grid,i+1,j);
        DFSMarking(grid,i-1,j);
        DFSMarking(grid,i,j+1);
        DFSMarking(grid,i,j-1);
    }
时间复杂：O（mn)  
空间复杂度: O(1) 改变input地图集合，不需要空间。  
流数据：不可以处理流数据  
