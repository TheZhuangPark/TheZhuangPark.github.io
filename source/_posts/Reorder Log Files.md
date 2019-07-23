---
title: Log File重排序 
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
![](/gallery/leetcode-1/ReorderLogFilesResult.JPG)  
<!--more-->  

    public String[] reorderLogFiles(String[] logs) {

        Comparator<String> myComp = new Comparator<String>(){
            @Override
            public int compare(String s1,String s2){
                int s1si = s1.indexOf(' ');
                int s2si = s2.indexOf(' ');
                char s1fc = s1.charAt(s1si+1);
                char s2fc = s2.charAt(s2si+1);
	//                char s1fc = s1.charAt(s1.length()-1);
	//                char s2fc = s2.charAt(s2.length()-1);

                if(s1fc<='9'){
                    if(s2fc<='9') return 0;
                    else return 1;
                }
                if(s2fc<='9')return -1;

                int preCompute = s1.substring(s1si+1).compareTo(s2.substring(s2si+1));
                if(preCompute==0) return s1.substring(0,s1si).compareTo(s2.substring(0,s2si));
                return preCompute;
            }

        };
        Arrays.sort(logs,myComp);
        return logs;
    }


