title: LeetCode - Word Ladder
date: 2015-08-28 00:06:50
categories :
    - LeetCode
tags:
    - BFS
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/word-ladder/ %}

Given two words (beginWord and endWord), and a dictionary, find the length of shortest transformation sequence from beginWord to endWord, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

{% endblockquote %}

Solution in Java
----------------

{% codeblock %}
class Node{
    String word;
    int distance;
    Node(String word, int distance){
        this.word = word;
        this.distance = distance;
    }
}

public int ladderLength(String beginWord, String endWord, Set<String> wordDict) {

    LinkedList<Node> nodes = new LinkedList<>();
    nodes.add(new Node(beginWord, 1));
    wordDict.add(endWord);

    while(!nodes.isEmpty()){

        Node node = nodes.removeFirst();

        if(node.word.equals(endWord)){
            return node.distance;
        }

        char[] chars = node.word.toCharArray();
        for(int i=0;i<chars.length;i++){
            char temp = chars[i];
            for(char c='a';c<='z';c++){
                if(temp != c){
                    chars[i] = c;

                    String word = String.valueOf(chars);
                    if(wordDict.contains(word)){
                        wordDict.remove(word);
                        nodes.add(new Node(word, node.distance+1));
                    }
                }
            }
            chars[i] = temp;
        }
    }
    return 0;
}
{% endcodeblock %}
