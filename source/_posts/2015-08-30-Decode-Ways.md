title: LeetCode - Decode Ways
date: 2015-08-30 19:44:27
categories : 
    - LeetCode
tags:
    - Dynamic Programming
---


Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/decode-ways/ %}

A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1, 'B' -> 2, ..., 'Z' -> 26

Given an encoded message containing digits, determine the total number of ways to decode it.

{% endblockquote %}

Solution in Java
----------------

{% codeblock lang:java %}
public int numDecodings(String s) {

    int n = s.length();
    if(n == 0){
        return 0;
    }

    int[] dp = new int[n+1];
    dp[0] = 1;
    dp[1] = s.charAt(0) == '0' ? 0 : 1;

    for(int i=2;i<=n;i++){

        String p1 = s.substring(i-1, i);
        if(dp[i-1] > 0 && p1.compareTo("1") >= 0 && p1.compareTo("9") <= 0){
            dp[i] += dp[i-1];
        }

        String p2 = s.substring(i-2, i);
        if(dp[i-2] > 0 && p2.compareTo("10") >= 0 && p2.compareTo("26") <= 0){
            dp[i] += dp[i-2];
        }
    }
    return dp[n];
}
{% endcodeblock %}