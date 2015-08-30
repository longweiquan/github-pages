title: LeetCode - Add and Search Word
date: 2015-08-30 13:19:13
categories:
    - LeetCode
tags:
    - Data Structure
    - Trie
    - DFS
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/add-and-search-word-data-structure-design/ %}

Design a data structure that supports the following two operations:

- void addWord(word)
- bool search(word)

search(word) can search a literal word or a regular expression string containing only letters a-z or '.'. 
A '.' means it can represent any one letter.

{% endblockquote %}

Solution in Java
----------------

{% codeblock lang:java %}
public class WordDictionary {

    TrieNode root = new TrieNode();

    public void addWord(String word) {
        TrieNode current = root;
        for(char c: word.toCharArray()){
            if(!current.children.containsKey(c)){
                TrieNode newNode = new TrieNode();
                newNode.value = c;
                current.children.put(c, newNode);
            }
            current = current.children.get(c);
        }
        current.isWord = true;
    }

    public boolean search(String word) {
        return dfs(word, root);
    }

    private boolean dfs(String word, TrieNode node){
        if(word.isEmpty()){
            return node.isWord;
        }

        char c = word.charAt(0);
        if (c == '.') {
            for (TrieNode child : node.children.values()) {
                if (dfs(word.substring(1), child)) {
                    return true;
                }
            }
            return false;
        } else {
            return node.children.containsKey(c) && dfs(word.substring(1), node.children.get(c));
        }
    }

    class TrieNode {
        char value;
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isWord = false;
    }
}
{% endcodeblock %}