## 1 description

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

## 2 思路

### 2.1 有向图的数据结构

课程的每个依赖关系相当于有向图的一条边，我们可以构建一个有向图然后找到一个通路（遍历所有节点，好像是“通路这个叫法”）  
那么有向图可以用什么数据结构呢？

1. 矩阵，行为起点，列为终点，可达为1，反之为0
2. 链表，维护一个下一跳节点数组

### 2.2 拓扑排序（BFS or DFS）

1. BFS 每个节点为起点，做广度优先搜索，最先搜索到一条通路，则返回，最坏情况，没有通路，放回空数组
2. DFS 同BFS，每个节点做深度优先搜索到底，如果是一条通路，则返回，不是则跳回上一跳，（这个好久没写了，貌似BFS逻辑比较简单）

## 3 知识点

### 3.1 BFS & DFS

https://www.jianshu.com/p/b42bcbb21133

上面这篇文章BFS是用链表来存的图，采用queue辅助遍历，我们一开始用矩阵来存数据，不适合用BFS，因此考虑用DFS来实现。

## 4 代码

### 4.1 C++

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        int matrix[numCourses][numCourses];
        
        for (int i=0; i<numCourses; i++) {
            for (int j=0; j<numCourses; j++) {
                matrix[i][j] = 0;
            }
        }
        
        for (int i=0; i<prerequisites.size(); i++) {
            matrix[prerequisites[i][1]][prerequisites[i][0]] = 1;
        }        
        
        stack<int> stk;
        for (int i=0; i<numCourses; i++) {
            if (DFS(i, stk, 1, numCourses,matrix[numCourses][numCourses])==true){
                stack<int> tmp;
                vector<int> result;
                while(!stk.empty()) {
                    tmp.push(stk.top());
                    stk.pop();
                }
                 while(!tmp.empty()) {
                    result.push_back(tmp.top());
                    tmp.pop();
                }
                return result;
            }
        }
        vector<int> empty;
        return empty;
    }
    
    bool DFS(int node, stack<int> s, int level, int num, int (*matrix)[num]) {
        if (level == num) return true;
        for (int i=0; i<num; i++) {
            if (matrix[node][i] == 1){
                s.push(i);
                if (DFS(i, s, level++, num) == true)return true;
            }
        }
        return false;
    }
    
};
```

## 5 抄作业

对不起了辉哥，只有先抄个作业了

```java
class Solution {
  static int WHITE = 1;
  static int GRAY = 2;
  static int BLACK = 3;

  boolean isPossible;
  Map<Integer, Integer> color;
  Map<Integer, List<Integer>> adjList;
  List<Integer> topologicalOrder;

  private void init(int numCourses) {
    this.isPossible = true;
    this.color = new HashMap<Integer, Integer>();
    this.adjList = new HashMap<Integer, List<Integer>>();
    this.topologicalOrder = new ArrayList<Integer>();

    // By default all vertces are WHITE
    for (int i = 0; i < numCourses; i++) {
      this.color.put(i, WHITE);
    }
  }

  private void dfs(int node) {

    // Don't recurse further if we found a cycle already
    if (!this.isPossible) {
      return;
    }

    // Start the recursion
    this.color.put(node, GRAY);

    // Traverse on neighboring vertices
    for (Integer neighbor : this.adjList.getOrDefault(node, new ArrayList<Integer>())) {
      if (this.color.get(neighbor) == WHITE) {
        this.dfs(neighbor);
      } else if (this.color.get(neighbor) == GRAY) {
        // An edge to a GRAY vertex represents a cycle
        this.isPossible = false;
      }
    }

    // Recursion ends. We mark it as black
    this.color.put(node, BLACK);
    this.topologicalOrder.add(node);
  }

  public int[] findOrder(int numCourses, int[][] prerequisites) {

    this.init(numCourses);

    // Create the adjacency list representation of the graph
    for (int i = 0; i < prerequisites.length; i++) {
      int dest = prerequisites[i][0];
      int src = prerequisites[i][1];
      List<Integer> lst = adjList.getOrDefault(src, new ArrayList<Integer>());
      lst.add(dest);
      adjList.put(src, lst);
    }

    // If the node is unprocessed, then call dfs on it.
    for (int i = 0; i < numCourses; i++) {
      if (this.color.get(i) == WHITE) {
        this.dfs(i);
      }
    }

    int[] order;
    if (this.isPossible) {
      order = new int[numCourses];
      for (int i = 0; i < numCourses; i++) {
        order[i] = this.topologicalOrder.get(numCourses - i - 1);
      }
    } else {
      order = new int[0];
    }

    return order;
  }
}
```