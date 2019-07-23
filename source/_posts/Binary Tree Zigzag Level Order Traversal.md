---
title: 二叉树Z顺序遍历
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
 ![](/gallery/leetcode-1/zigzagLevelOrderResult.JPG)  
<!--more-->  
    
	  public List<List<Integer>> zigzagLevelOrder(TreeNode root){
        List<List<Integer>> res=new ArrayList<>();
        travel(root,res,0);
        return res;
    }
    private void travel(TreeNode node,List<List<Integer>>list,int level){
        if(node==null)return ;
        if(list.size()<=level){
            List<Integer> newLevel= new ArrayList<>();
            list.add(newLevel);
        }
        List<Integer> tmplist=list.get(level);
        if(level%2==0) tmplist.add(node.val);
        else tmplist.add(0,node.val);

        travel(node.left,list,level+1);
        travel(node.right,list,level+1);
    }

maybe a queue ?
