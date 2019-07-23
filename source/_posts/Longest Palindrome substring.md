---
title: 最长回文子字符串
layout: post
date: 2019-07-02 19:29:00
comments: true
tags: 
    - 算法
categories: 
    - 算法
    - string 
keywords: algorithms
description: 
thumbnail: /gallery/leetcode-1/algorithm1.JPG
---
![](/gallery/leetcode-1/LongestPalindromicSubstringresult.JPG) 
<!--more-->  
 

    class Solution {
     private int lo, maxLen;
     public String longestPalindrome(String s) {
           int slen=s.length();
           if(slen<2)return s;
           for(int i=0;i<slen-1;i++){
               ExtendPalindrome(s,i,i);
               ExtendPalindrome(s,i,i+1);
           }
           return s.substring(lo,lo+maxLen);
    }
    
     public void ExtendPalindrome(String s,int left,int right){
        while(left >= 0 && right < s.length() && s.charAt(left)==s.charAt(right)){
            left--;
            right++;
        }
        int len=right-left-1;
        if(maxLen<len){
            maxLen=len;
            lo=left+1;
        }
    }
    }

非常简单但是速度优化非常好，简单地说就是以一个字符或字符间为中心拓展出去。  
每个回文子子字符串都有个中心，可以是某个字符，也可以是两个字符中间的空隙。  

ExtendPalindrome(s,i,i)就是以S字符串的I位置为中心扩展，看该子字符串可以为多长的回文子字符串。 比如eabbcbbaf, 的以c为中心扩展可以得到最长回文字符串abbcbba。  
然后在另一种情况，比如eabbaf则是以bb的中间的间隙为中心拓展得到最长回文串abba，这就是ExtendPalindrome(s,i,i+1)的作用，以i,i+1中间的间隙为中心拓展。  
               
    ExtendPalindrome(s,i,i);
    ExtendPalindrome(s,i,i+1);  

然后就是一个循环结果，来循环每个S字符串上的每个中心点。  
       
     for(int i=0;i<slen-1;i++)
       {     ExtendPalindrome(s,i,i);
             ExtendPalindrome(s,i,i+1);
       }
 

  
## Longest Palindromic Substring
![](/gallery/leetcode-1/Longest Palindromic Substring-result.JPG)    
a very simple solution with high performance, bascially it search the center of every possible palindrome of string s, once we get the center then we extend it to get longes palindrome of string s.   
For example, we have eabbcbbaf as string s, we get answer when we extend c as the center and get abbcbba as longest panlindrome.  
But we can't say that center must be the character of string s, center can be in the middle of two character, like we have eabbaf, the answer is abba, where center is between bb, in ExtendPalindrome(s,i,i+1); i represent first b, i+1 represent second b.  
                  
    ExtendPalindrome(s,i,i);
    ExtendPalindrome(s,i,i+1);   

And then, it's kind of no-brainer, just visit every possible center of string s, extend center, get the longest palindrome.

    for(int i=0;i<slen-1;i++)
       {     ExtendPalindrome(s,i,i);
             ExtendPalindrome(s,i,i+1);
       }




