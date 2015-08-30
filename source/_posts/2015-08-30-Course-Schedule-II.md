title: LeetCode - Course Schedule II
date: 2015-08-30 18:57:14
courses:
    - LeetCode
tags:
    - BFS
    - Graph
---


Problem
-------

{% blockquote @LeetCode https://leetcode.com/problems/course-schedule-ii/ %}

There are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.


{% endblockquote %}


Solution in Java
----------------

{% codeblock lang:java %}
public int[] findOrder(int numCourses, int[][] prerequisites) {

    // count courses' prerequisites
    int[] pc = new int[numCourses];
    for(int[] p: prerequisites){
        pc[p[0]]++;
    }

    // add no prerequisite course to the final order
    List<Integer> order = new LinkedList<>();
    for(int i=0;i<numCourses;i++){
        if(pc[i] == 0){
            order.add(i);
        }
    }

    // Initialize a queue of no prerequisite course
    LinkedList<Integer> queue = new LinkedList<>(order);

    // BFS search for the final order
    while(!queue.isEmpty()){
        int npCourse = queue.removeFirst();
        for(int[] p: prerequisites){
            if(p[1] == npCourse){
                int pCourse = p[0];
                pc[pCourse]--;
                if(pc[pCourse] == 0){
                    queue.add(pCourse);
                    order.add(pCourse);
                }
            }
        }
    }

    if(order.size() == numCourses){
        return order.stream().mapToInt(i->i).toArray();
    } else {
        return new int[0];
    }
}
{% endcodeblock %}