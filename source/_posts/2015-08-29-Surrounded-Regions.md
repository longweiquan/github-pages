title: LeetCode - Surrounded Regions
date: 2015-08-29 20:00:02
categories :
    - LeetCode
tags:
    - BFS
---

Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/surrounded-regions/ %}

Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

{% endblockquote %}

Analyse
-------
Only the regions on the border can be retained. So we can flip the border 'O' by '#', then flip all 'O' by 'X', then 
flip all '#' by 'O'. The difficulty is how to flip the border 'O' region. DFS can resolve the problem, but will encounter 
"Stack Overflow" problem when the data grid is large. BFS can avoid this problem, so it's a good way to solve the
problem.


Solution in Java
----------------

{% codeblock %}
public void solve(char[][] board) {

    if(board == null || board.length == 0){
        return;
    }

    int m = board.length;
    int n = board[0].length;

    // flag from boarder cases
    for(int i = 0; i<m;i++){
       for(int j = 0; j<n;j++){
           if((i==0||j==0||i==m-1||j==n-1) && board[i][j] == 'O'){
               bfsFlag(board, i, j);
           }
       }
    }

    // replace symbols
    for(int i = 0; i<m; i++){
        for(int j = 0; j<n; j++){
            if(board[i][j] == 'O'){
                board[i][j] = 'X';
            } else if(board[i][j] == '#'){
                board[i][j] = 'O';
            }
        }
    }
}

public void bfsFlag(char[][] board, int nodeX, int nodeY){

    LinkedList<int[]> nodes = new LinkedList<>();
    nodes.add(new int[]{nodeX, nodeY});

    while (!nodes.isEmpty()){

        int[] node = nodes.removeFirst();
        int x = node[0];
        int y = node[1];

        if(x < 0 || x >= board.length || y<0 || y >= board[0].length || board[x][y] != 'O'){
            continue;
        }

        board[x][y] = '#';
        nodes.add(new int[]{x-1, y});
        nodes.add(new int[]{x+1, y});
        nodes.add(new int[]{x, y-1});
        nodes.add(new int[]{x, y+1});
    }
}
{% endcodeblock %}