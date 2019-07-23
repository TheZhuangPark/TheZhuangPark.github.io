---
title: LRU缓存
layout: post
date: 2019-07-14 19:21:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
 ![](/gallery/leetcode-1/LeastRecentUseCacheResult.JPG)  
<!--more-->  

  		
	import java.util.Hashtable;

	public class LRUCache {
    class DLinkedNode{
      int key;
      int value;
      DLinkedNode pre;
      DLinkedNode next;
    }
    private void addNode(DLinkedNode node){
        node.pre = head;
        node.next= head.next;
        head.next.pre=node;
        head.next=node;
    }
    private void removeNode(DLinkedNode node){
        DLinkedNode preNode=node.pre;
        DLinkedNode nextNode=node.next;
        preNode.next=nextNode;
        nextNode.pre=preNode;
    }
    private void moveToHead(DLinkedNode node){
        this.removeNode(node);
        this.addNode(node);
    }
    private DLinkedNode popTail(){
        DLinkedNode tailNode=tail.pre;
        this.removeNode(tailNode);
        return tailNode;
    }
    private Hashtable<Integer,DLinkedNode> cache =
            new Hashtable<Integer, DLinkedNode>();
    private int count;
    private int capacity;
    private DLinkedNode head,tail;

    public LRUCache(int capacity){
        this.capacity=capacity;
        this.count=0;
        head=new DLinkedNode();
        head.pre=null;
        tail=new DLinkedNode();
        tail.next=null;
        head.next=tail;
        tail.pre=head;
    }
    public int get(int key){
        DLinkedNode targetNode=cache.get(key);
        if(targetNode==null)return -1;
        this.moveToHead(targetNode);
        return targetNode.value;
    }
    public void put(int key,int value){
        DLinkedNode node=cache.get(key);
        if(node==null){//insert the node, pop out the extract
            node=new DLinkedNode();
            node.key=key;
            node.value=value;
            this.addNode(node);
            this.cache.put(key,node);
            count++;
            if(count>capacity){
                DLinkedNode TailNode=this.popTail();
                this.cache.remove(TailNode.key);
                count--;
            }
        }else{//update the node
            node.value=value
            this.moveToHead(node);
        }
    }
	}

 no need for count, psudoNode, 

 

  
    