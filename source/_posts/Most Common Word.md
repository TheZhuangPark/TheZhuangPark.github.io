---
title: 最常见的单词
layout: post
date: 2019-07-15 12:42:00
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
![](/gallery/leetcode-1/MostCommonWordResult.JPG)  
<!--more-->  

    //solution 1
     Set<String> ban = new HashSet<>(Arrays.asList(banned));
     Map<String, Integer> count = new HashMap<>();
     String[] words = p.replaceAll("\\W+" , " ").toLowerCase().split("\\s+");
     for (String w : words) if (!ban.contains(w)) count.put(w, count.getOrDefault(w, 0) + 1);
     return Collections.max(count.entrySet(), Map.Entry.comparingByValue()).getKey();

    //solution 2
    public String res;
    public int maxcnt;
    public String mostCommonWord(String paragraph, String[] banned) {
        TrieNode root = new TrieNode();
        StringBuilder st = new StringBuilder();
        res = "";
        maxcnt = 0;
        boolean isString = false;
        for (char ch : paragraph.toCharArray()) {
            if (ch >= 'a' && ch <= 'z') {
                st.append(ch);
                isString = true;
            } else if (ch >= 'A' && ch <= 'Z') {
                st.append((char)(ch + 'a' - 'A'));
                isString = true;
            } else {
                if (isString) {
                    root.insert(st.toString());
                    st = new StringBuilder();
                    isString = false;
                } 
            }
        }
        if (isString) root.insert(st.toString());        
        for (String s : banned) root.ban(s);
        root.findMax(root);
        return res;
    }
    class TrieNode {
        String word = "";
        int cnt = 0;
        TrieNode[] links = new TrieNode[26];
        
        void insert(String s) {
            TrieNode curr = this;
            char[] chs = s.toCharArray();
            for (int i = 0; i < chs.length; ++i) {
                int index = chs[i] - 'a';
                if (curr.links[index] == null) {
                    curr.links[index] = new TrieNode();
                    curr.links[index].word = curr.word + chs[i];
                }
                curr = curr.links[index];
            }
            curr.cnt += 1;
        }
        void ban(String s) {
            char[] chs = s.toCharArray();
            TrieNode curr = this;
            for (int i = 0; i < chs.length; ++i) {
                int index = chs[i] - 'a';
                if (curr.links[index] == null) return;
                curr = curr.links[index];
            }
            curr.cnt = 0;
        }
        void findMax(TrieNode curr) {
            if (curr == null) return;
            if (curr.cnt > maxcnt) {
                res = curr.word;
                maxcnt = curr.cnt;
            }
            for (int i = 0; i < curr.links.length; ++i) {
                findMax(curr.links[i]);
            }
        }
        
        
    }

问题：这个问题很简单，给定一个字符串，然后还有ban word集合，统计出现次数最多的单词。 比如：paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
Output: "ball"

算法：  
第一种解法非常无脑，就是用哈希表，java的string自带分割功能，遍历每个词组，并统计。  
第二种解法性能高很多很多，利用字典树，但是比较麻烦。
  
模板：无

时间复杂度：O(n) n为单词数目  
空间复杂度：O(n)
