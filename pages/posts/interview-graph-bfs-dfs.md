---
title: Interview graph bfs/dfs
date: 2024-03-04
description: Interview graph bfs/dfs
tag: interview, graph, bfs, dfs, algorithm
author: nicolas2lee
---

# Interview graph BFS/DFS
## BFS
### Sliding Puzzle
#### Problem
[773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/description/)

#### Solution

The board is very small, so we can use BFS to search all states. The goal is to find the state with "123450". We can convert the board to a string to represent. The step transition 
is that swapping 0 with it's adjecent. And also we can convert the board to string as state, and add it to a set ```seen```, if the string is already in ```seem```, then we do not 
need to search again.

```java
class Solution {
    static class Node{
        String str;
        int step;
        int x;
        int y;
        public Node(String str, int step, int x, int y){
            this.str= str;
            this.step =step;
            this.x=x; this.y=y;
        }
    }
    int[][] dir = {{0,1}, {0, -1}, {1,0}, {-1, 0}};

    public int slidingPuzzle(int[][] board) {
        var sb = new StringBuilder();
        int startX= -1, startY=-1;
        for(int i=0; i<board.length; i++){
            for (int j=0; j<board[0].length; j++){
                sb.append(board[i][j]);
                if (board[i][j]==0){
                    startX=i; startY=j;
                }
            }
        }
        var initStr = sb.toString();
        var node = new Node(initStr, 0, startX, startY);
        var goal = "123450";
        return bfs(board, goal, node);
    }

    int bfs(int[][] board, String goal, Node initNode){
        Queue<Node> q = new LinkedList<>();
        q.add(initNode);
        var seen = new HashSet<String>();
        seen.add(initNode.str);
        while(!q.isEmpty()){
            var cur = q.poll();
            if (cur.str.equals(goal)){
                return cur.step;
            }
            for(int i=0; i<dir.length; i++){
                var x = cur.x+dir[i][0];
                var y = cur.y+dir[i][1];
                if (x>=0 && x<2 && y>=0 && y<board[0].length){
                    var curBoard = getBoardFromStr(cur.str);
                    swap(curBoard, x,y, cur.x, cur.y);
                    var curStr = getStrFromBoard(curBoard);
                    if (!seen.contains(curStr)){
                        var node = new Node(curStr, cur.step+1, x, y);
                        q.add(node);
                        seen.add(curStr);
                    }
                }
            }
        }
        return -1;
    }

    int[][] getBoardFromStr(String str){
        var board = new int[2][3];
        for(int i=0; i<str.length(); i++){
            //System.out.println(i);
            board[i/3][i%3]=str.charAt(i)-'0';
        }
        return board;
    }

    String getStrFromBoard(int[][] board){
        var sb = new StringBuilder();
        for(int i=0; i<board.length; i++){
            for (int j=0; j<board[0].length; j++){
                sb.append(board[i][j]);
            }
        }
        return sb.toString();
    }

    void swap(int[][] board, int x, int y, int xx, int yy){
        var tmp = board[x][y];
        board[x][y] =board[xx][yy];
        board[xx][yy] = tmp;
    }
}
```

