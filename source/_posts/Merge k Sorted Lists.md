---
title: 融合K个有序数组 Merge K Sorted Lists
layout: post
date: 2019-07-17 12:18:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
    - 递归
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
![](/gallery/leetcode-1/MergeKSortedListResult.JPG)  
<!--more-->  
   
    
    public  ListNode mergeKLists(ListNode[] lists) {
       return partion(lists,0,lists.length-1);
    }
    public  ListNode partion(ListNode[] lists,int s,int e){
        if(s==e) return lists[s];
        if(s<e){
            int q=(s+e)/2;
            ListNode l1=partion(lists,s,q);
            ListNode l2=partion(lists,q+1,e);
            return merge(l1,l2);
        }else
            return null;
    }
    public  ListNode merge(ListNode l1,ListNode l2){
        if(l1==null) return l2;
        if(l2==null) return l1;
        if(l1.val<l2.val){
            l1.next=merge(l1.next,l2);
            return l1;
        }else{
            l2.next=merge(l1,l2.next);
            return l2;
        }
    }

问题：就是给定几条已经排好序的链表，然后融合它们成一条有序的链表。  
[1,3,4] [2,8,9] [3,5,7] -> [1,2,3,3,4,5,7,8,9]    

算法：就是递归，把问题分成若干份。比如上例子，把问题先分成融合[1,3,4] [2,8,9]。  
然后开始融合这两个数组成[1,2,3,4,8,9] 再让该链表和[3,5,7]融合。   
所以这里有个partion函数分割问题，merge两个链表。  
这个方法高效在于：   
对比效率高，对比两条链表可以更快的消耗完其中一条，当其中一条到头了就不用再对比了。
这本质上还是排序，排序算法就是要对比，提高对比的效率。  

模板： 
 	
	public  ListNode mergeKLists(ListNode[] lists) {
       return partion(lists,0,lists.length-1);
    }// 主方法直接调用partion,partion(处理的数据，start, end)其中通过变化start,end来分割问题的规模    

    public  ListNode partion(ListNode[] lists,int s,int e){
        if(s==e) return lists[s];
 		//partion函数要确立分割的尽头，当分割到start,end相等，或满足条件的时候就返回该元素
        if(s<e){ 
 			//确立分割点，一般通过计算start,end然后得出新点q,然后调用两次partion
            int q=(s+e)/2;
            ListNode l1=partion(lists,s,q);
            ListNode l2=partion(lists,q+1,e);

			//确立当分割达到尽头时，处理数据的方式
            return merge(l1,l2);
        }else
            return null;
    }  
	//融合，merge(listNode node1,node2,node3...)一般通过变化node成node.next来缩减
     public  ListNode merge(ListNode l1,ListNode l2){
        if(l1==null) return l2;
        if(l2==null) return l1; 
        //确立分割尽头
        if(l1.val<l2.val){
 			//确立分割点
            l1.next=merge(l1.next,l2);
            return l1;
        }else{
            l2.next=merge(l1,l2.next);
            return l2;
        }
    }

时间复杂度: O(nk)最坏的情况下  
空间复杂度：O(1)  
不可以处理流数据，分割时需要知道数据全貌  