title: LeetCode - Word Break II
date: 2015-08-23 15:56:23
categories:
    - LeetCode
tags: 
    - Dynamic Programming
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/word-break-ii/ %}

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word 
is a valid dictionary word. Return all such possible sentences.

{% endblockquote %}

Analyse
-------

It's an upgrade version of {% post_link Word-Break %}. Instead of boolean result, the second version ask for all possible
solutions in string form. This time we need an array of string list as dp array, but it's simpler to use Map in Java.
 
The basic idea is the same as the version 1, so I won't repeat here. But it's not good enough in some special cases. We
need optimizations.

First optimization, we don't need to verify any substring s[i..j]. We can limit i from j-maxWordLength to j-1.
 
Second optimization, verify that a string is breakable before calculating the results. 


Solution in Java
----------------

{% codeblock %}
public List<String> wordBreak(String s, Set<String> wordDict) {

    int maxWordLength = 0;
    for(String word: wordDict){
        maxWordLength = Math.max(maxWordLength, word.length());
    }
    int n = s.length();

    if(n == 0 || maxWordLength == 0 || !isValid(s, wordDict)){
        return new ArrayList<>();
    }

    Map<Integer, List<String>> dp = new HashMap<>();

    for(int j=1;j<=n;j++){
        for(int i=j-1; i>=j-maxWordLength; i--){
            if((i==0 || dp.get(i) != null) && wordDict.contains(s.substring(i,j))){

                // init list
                if(dp.get(j) == null){
                    dp.put(j, new ArrayList<>());
                }

                String word = s.substring(i, j);
                if(i == 0){
                    dp.get(j).add(word);
                } else {
                    for(int k=0; k<dp.get(i).size();k++){
                        dp.get(j).add(dp.get(i).get(k) + " " + word);
                    }
                }
            }
        }
    }

    if(dp.get(n) != null){
        return dp.get(n);
    } else {
        return new ArrayList<>();
    }
}

private boolean isValid(String s, Set<String> wordDict) {

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
