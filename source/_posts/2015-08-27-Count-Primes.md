title: LeetCode - Count Primes
date: 2015-08-27 08:31:06
categories :
    - LeetCode
tags:
    - Math
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/count-primes/ %}

Count the number of prime numbers less than a non-negative number, n.

{% endblockquote %}

Analyse
-------
Please reference to the [Prime Number Algorithm](https://en.wikipedia.org/wiki/Prime_number)


Solution in Java
----------------

{% codeblock %}
public int countPrimes(int n) {

    boolean[] multiples = new boolean[n];

    for(int i=2;i*i<n;i++){
        if(!multiples[i]){
            for(int j=i+i;j<n;j+=i){
                multiples[j] = true;
            }
        }
    }

    int count = 0;
    for(int i=2;i<n;i++){
        if(!multiples[i]){
            count += 1;
        }
    }
    return count;
}
{% endcodeblock %}
