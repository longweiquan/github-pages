title: LeetCode - Word Ladder II
date: 2015-08-28 00:09:52
categories:
    - LeetCode
tags:
    - BFS
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/word-ladder-ii/ %}

Given two words (start and end), and a dictionary, find all shortest transformation sequence(s) from start to end, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary

{% endblockquote %}

Solution in Java
----------------

{% codeblock %}
class Node{
    String word;
    int distance;
    Node prev;
    Node(String word, int distance, Node prev){
        this.word = word;
        this.distance = distance;
        this.prev = prev;
    }
}

public List<List<String>> findLadders(String start, String end, Set<String> dict) {

    LinkedList<Node> nodes = new LinkedList<>();
    nodes.add(new Node(start, 1, null));
    dict.add(end);

    Set<String> visited = new HashSet<>();
    List<List<String>> results = new ArrayList<>();
    int currentDistance = 1;
    int endDistance = Integer.MAX_VALUE;

    while (!nodes.isEmpty()){

        Node node = nodes.removeFirst();
        if(node.word.equals(end)){
            List<String> result = new LinkedList<>();
            Node current = node;
            while (current != null){
                result.add(0, current.word);
                current = current.prev;
            }
            results.add(result);
            endDistance = node.distance;
            continue;
        }

        if(node.distance > endDistance){
            break;
        }

        if(node.distance > currentDistance){
            currentDistance = node.distance;
            dict.removeAll(visited);
            visited.clear();
        }

        char[] chars = node.word.toCharArray();
        for(int i=0; i<chars.length; i++){
            char temp = chars[i];
            for(char c='a'; c<='z'; c++){
                if(temp != c){
                    chars[i] = c;

                    String word = String.valueOf(chars);
                    if(dict.contains(word)){
                        visited.add(word);
                        nodes.add(new Node(word, node.distance+1, node));
                    }
                }
            }
            chars[i] = temp;
        }
    }

    return results;
}
{% endcodeblock %}