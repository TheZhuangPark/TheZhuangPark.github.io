---
title: 两数相加 
layout: post
date: 2019-07-02 19:29:00
comments: true
tags: 
    - 算法
categories: 
    - 算法 
keywords: 算法
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---

![](/gallery/leetcode-1/addtwonumbers-result.JPG)  
<!--more-->  
  
    class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode Head= null;
        ListNode Tail=null;
        int carry=0,t1=0,t2=0,t3=0;
        ListNode p1=l1;
        ListNode p2=l2;

        while(p1!=null||p2!=null||carry!=0){
            t1=0; t2=0;
            if(p1!=null){ t1=p1.val;}
            if(p2!=null){ t2=p2.val;}
            if((p1==null && p2!=null)||(p2==null && p1!=null)) {
                ListNode tmp = null;
                if (p2 != null) tmp = p2;
                else tmp = p1;
                Tail.next = tmp;
                if (carry != 0) tmp.val += 1;
                while (tmp != null) {
                    if (tmp.val >= 10) {
                        tmp.val = 0;
                        if (tmp.next != null) {
                            tmp.next.val += 1;
                        } else {
                            ListNode tmp1 = new ListNode(1);
                            tmp.next = tmp1;
                        }
                    }
                    tmp = tmp.next;
                }//   printListNode(Head);
                return Head;
            }//if one of the list is null no need for cal

            Boolean flag=true;
            if((t3=t1+t2)<10){ flag=false;}
            else{ t3=t3-10;}
            if(Head==null){ Head=new ListNode(t3); Tail=Head;
                //printListNode(Head);
            }
            else{
                ListNode tmp=new ListNode(t3+carry);
                carry=0;
                if(tmp.val>=10){//when t3=9 carry=1
                    tmp.val-=10;
                    carry=1;
                }
                Tail.next=tmp;
                Tail=tmp;
                //  printListNode(Head);
            }
            if(flag) carry=1;
           if(p1!=null) p1=p1.next;
           if(p2!=null) p2=p2.next;
          //  printListNode(Head);
        }
        return Head;
    }
    }


这是我极其杂乱的方法，但是速度和内存还行。这里我的一开始的代码复用的太多，所以做了一些优化少了很多行。 这个方法很简单，就是最近的，从两个list各取一个数字，然后相加，如果大于10则，sum=sum-10, carry=1。 也就是进位1，和人的口算的方法非常相似。 然后在下一轮的计算中，把这个carry加进去。  

注意如果有一个list已经没有数字了，另一个还有，这个时候不用计算直接把另一个的list头接到我们勾结的结果list的尾巴上。

# Add Two Numbers
![](/gallery/leetcode-1/addtwonumbers-result.JPG)  
My simple solution with a not so bad performance, it's so simple, just a copy solution of how human brain add two number. First get two number for two list, and add them together to get sum. Then if sum > 10, sum=sum-10 carry=1. At next round, we would add carry to sum.

If one of the list such as list A reach its' end, no need for more calculation just link list B's head to our result list's end. 

   class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode Head= null;
        ListNode Tail=null;
        int carry=0,t1=0,t2=0,t3=0;
        ListNode p1=l1;
        ListNode p2=l2;

        while(p1!=null||p2!=null||carry!=0){
            t1=0; t2=0;
            if(p1!=null){ t1=p1.val;}
            if(p2!=null){ t2=p2.val;}
            if((p1==null && p2!=null)||(p2==null && p1!=null)) {
                ListNode tmp = null;
                if (p2 != null) tmp = p2;
                else tmp = p1;
                Tail.next = tmp;
                if (carry != 0) tmp.val += 1;
                while (tmp != null) {
                    if (tmp.val >= 10) {
                        tmp.val = 0;
                        if (tmp.next != null) {
                            tmp.next.val += 1;
                        } else {
                            ListNode tmp1 = new ListNode(1);
                            tmp.next = tmp1;
                        }
                    }
                    tmp = tmp.next;
                }//   printListNode(Head);
                return Head;
            }//if one of the list is null no need for cal

            Boolean flag=true;
            if((t3=t1+t2)<10){ flag=false;}
            else{ t3=t3-10;}
            if(Head==null){ Head=new ListNode(t3); Tail=Head;
                //printListNode(Head);
            }
            else{
                ListNode tmp=new ListNode(t3+carry);
                carry=0;
                if(tmp.val>=10){//when t3=9 carry=1
                    tmp.val-=10;
                    carry=1;
                }
                Tail.next=tmp;
                Tail=tmp;
                //  printListNode(Head);
            }
            if(flag) carry=1;
           if(p1!=null) p1=p1.next;
           if(p2!=null) p2=p2.next;
          //  printListNode(Head);
        }
        return Head;
    }
    }