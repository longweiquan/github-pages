title: LeetCode - Word Break
date: 2015-08-23 15:42:32
categories:
    - LeetCode
tags: 
    - Dynamic Programming
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/word-break/ %}

Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated 
sequence of one or more dictionary words.

{% endblockquote %}

Analyse
-------
The problem can be solved by Dynamic Programming. We use an array dp to stock the information that a substring s[0..i] 
can be broken into words not not. When checking a s[0..j] is breakable or not, we look for i where s[0..i] is breakable
and s[i..j] is an word in dictionary.

Solution in Java
----------------

{% codeblock %}
public boolean wordBreak(String s, Set<String> wordDict) {
    
    int n = s.length();
    boolean[] dp = new boolean[n+1];

    for(int j = 1; j<=n;j++){
        for(int i=j-1; i>=0; i--){
            if((i==0 || dp[i]) && wordDict.contains(s.substring(i,j))){
                dp[j] = true;
                break;
            }
        }
    }
    return dp[n];
}
{% endcodeblock %}