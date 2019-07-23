---
title: 深复制节点 Copy List with Random Pointer.
layout: post
date: 2019-07-15 12:40:00
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
 ![](/gallery/leetcode-1/copyRandomListResult.JPG)  
<!--more-->  

    public Node copyRandomList(Node head) {
          //copy all the node
        Node node=head;
        Map<Node,Node> map=new HashMap<Node,Node>(){};
        while(node!= null){
            map.put(node,new Node(node.val));
            node=node.next;
        }
        node=head;
        while(node!= null){
            map.get(node).next=map.get(node.next);
            map.get(node).random=map.get(node.random);
            node=node.next;
        }
        return map.get(head);
    }

问题:
给定了一个链表，这个链表上的节点还会连向一个随机的节点。 然后让你快速的深拷贝一份链表。

算法：
非常简单，而且很快，就设置一个哈希表，遍历那个链表然后把该链表的节点当key,创建新的节点当它的value。 然后再遍历一遍的时候，把获取的节点当key，找到拷贝节点，然后把属性赋值给拷贝节点。

模板：无

时间复杂度为O(n）
空间复杂度为O(n)
可以处理流数据吗？可以如果复制的时候指定复制多少个节点。 是不需要知道数据全貌的。