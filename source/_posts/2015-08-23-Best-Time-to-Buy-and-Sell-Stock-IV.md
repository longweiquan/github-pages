title: LeetCode - Best Time to Buy and Sell Stock IV
date: 2015-08-23 10:49:01
categories: 
    - LeetCode
tags: 
    - Dynamic Programming

---

Leetcode - Best Time to Buy and Sell Stock IV Solution

<!-- more -->

Problem
---
{% blockquote @LeetCode https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/ %}

Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete at most k transactions.

{% endblockquote %}

Analyse
---


This problem can be solved by Dynamic Programming. Note, DP is good at solving best solution problems. We will use two
dimensional DP solution because the prices length and the transaction times will impact the results.

In DP problem, we need to find out the states and the approaches to reach next states. In this problem, an obvious state
is the best profits, note globalMax[][]. An implicit state is the best profits with the last selling on the last day, 
note lastMax[][]. And the best profit is between making the last transaction on the last day or not.
{% codeblock %}
// i represents days and j represents transactions
global[i][j] = Max(global[i-1][j], lastMax[i][j])
{% endcodeblock %}

The problem is now transformed to calculate lastMax[i][j]. We have two approaches. The first one is based on the best 
profits until i-1 day with j-1 transaction and add a new transaction on the last day. The second approach based on the 
best profits with the last transaction on i-1 day, so we replace the last transaction to the i day.
{% codeblock %}
// prices represent the prices of stock
diff = prices[i] - prices[i-1];
lastMax[i][j] = Max(global[i-1][j-1]+diff, lastMax[i-1][j]+diff)
{% endcodeblock %}

Solution in Java
----------------

{% codeblock %}
public int maxProfit(int k, int[] prices) {

    int n = prices.length;
    if (n < 2 || k <= 0){
        return 0;
    }

    // Dynamic Programming Data Array

    // Best profit array with last transaction on the last day
    int[][] lastMax = new int[n][k + 1];

    // Best profit array
    int[][] globalMax = new int[n][k + 1];

    for (int i = 1; i < n; i++) {
        for (int j = 1; j <= k; j++) {
            int diff = prices[i] - prices[i - 1];

            // do a new transaction on the last day
            // or replace the last transaction if it was made yesterday
            lastMax[i][j] = Math.max(globalMax[i - 1][j - 1], lastMax[i - 1][j]) + diff;

            // Make the last transaction on the last day or not
            globalMax[i][j] = Math.max(globalMax[i - 1][j], lastMax[i][j]);
        }
    }

    return globalMax[n - 1][k];
}
{% endcodeblock %}




