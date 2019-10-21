# play-with-graph-algorithms

Python implementation of imooc course [玩转图论算法](https://coding.imooc.com/class/370.html), will update as the course going.

Check/fork original repo from course instructor [liuyubobobo](https://github.com/liuyubobobo)!

## Notes
### Chapter 2 图的基本表示

1. 顶点Vertex，边Edge

2. 无向图Undirected Graph，有向图Directed Graph

3. 有权图Weighted Graph，无权图Unweighted Graph

4. 自环边，平行边

5. 没有自环边并且没有平行边的图称为简单图

6. 树是一种无环图

7. 无环图不一定一定是树，比如多个联通分量

8. 联通的无环图是树

9. 生成树就是指包括了原来图中的所有的点并且联通的图，该生成树的边数是V - 1，但是V - 1的边组成的新图并不一定是生成树

10. 只有连通图才有（并且一定有）生成树

11. 一个图一定有生成森林，但是并不一定有生成树（由于是否联通的问题）

12. 对于无向图来说，顶点相邻的边数就是degree

13. 邻接矩阵，邻接表来表示图

14. 邻接矩阵空间复杂度O(V^2)，建图O(E)，查看两点是否相邻O(1)，求一个点的相邻节点O(V)（相当于遍历全部节点）

15. 稀疏图（sparse graph）和稠密图（dense graph）-> 完全图（complete graph）

16. 邻接表的空间复杂度O(V + E)，建图O(E*V)，未优化时查看两点是否相邻O(degree(v))，求一个点的相邻节点O(degree(v))；优化后（使用HashSet）建图O(E)，查看两点是否相邻O(1)

### Chapter 3 图的深度优先遍历

17. 树结构的遍历不用担心重复访问的问题，但是图的遍历会有重复遍历的问题（由于可能存在环），需要记录是否该点被访问过

18. dfs模板：
```python
dfs(0, visited=[False] * V, lst=[])

def dfs(v, visited, lst):
    visited[v] = True
    # 近似于图的先序遍历
    lst.apppend(v)
    for w in adj(v):
        if not visited[w]:
            dfs(v, visited, lst)

# 感觉递归出口写在开头更好一些
def dfs(v, visited, lst):
    if visited[v]:
        return
    visited[v] = True
    # 近似于图的先序遍历
    lst.apppend(v)
    for w in adj(v):
        dfs(v, visited, lst)
```

19. 深度优先时间复杂度是O(V + E)，如果是联通图，可以简化成O(E)

### Chapter 4 图的深度优先遍历的应用

20. 可以利用visited数组（初始化全部为-1表示没有被访问过，其实赋什么都可以。访问时候赋给非负的值），而赋的值就是该节点的group id

21. 图的路径问题：询问两点之间有没有路径，如果属于同一个联通分量，意味着两点之间有路径

22. 深度优先遍历过程中记录当前递归栈的路径即可

23. 或者记录下当前遍历点的前一个点即可

23. 一个点到其他点的路径问题 -> 单源最短路径问题

24. 只需要使用一个pre数组（不需要visited）也可以做DFS遍历，可读性略低一点儿。

25. 原始单源最短路径问题是给出了源点到其他所有点的路径，但有时候我们只是单纯想知道从A点到B点能不能联通，路径是什么，并不关心A到其他点的路径是什么，此时就可以剪枝来加速。

26. 递归的语义尤为重要，要很清楚递归函数的意义和返回值（如果有的话）的意义。

27. 无向图中的环的检测：相当于从起点出发，看有没有路径返回起点（注意当前遍历点的下一个节点不能是上一个节点），也是图的深度优先遍历的思路

28. 二分图：顶点V可以分为不相交的两部分，其中图中每一条边的两个端点都分属于不同的两部分。处理匹配问题时候（比如分类）很有效

29. 思路就是对图深度优先遍历+对个当前访问的节点的下一层递归的节点（即当前边的下一个节点）染色成跟当前节点不一样的颜色；如果发现下一个节点已经染过色并且跟当前节点颜色相同，说明不是二分图了

30. 递归同时记录更多信息，并且利用返回值剪枝

31. 概念：图同构，平面图（无交叉）

### Chapter 5 图的广度优先遍历

32. 图的BFS时间复杂度是O(V+E)，相当于全部的点和边都访问了一遍

33. BFS求解单源最短路径问题跟DFS类似，使用一个pre数组，在往队列里添加节点的时候，设置该被添加的节点的父节点（即当前从队列里pop出来的那个节点）

34. BFS在求单源最短路径的时候的解是全局最优解。而使用DFS则需要全局比较（所有的paths）才能确定最优解

35. 可以在遍历的过程中多记录一个信息distance，记录下某个点target到source点的steps

36. 可以对无权图（即权值一致）直接用BFS求最短路径

37. 遍历过程中可以使用随机队列（随机的线性数据结构），比如迷宫的生成

### Chapter 7 图论搜索和人工智能

38. BFS能够比较快速解决无权图最短路径问题

39. 有时候并不能很快确定这是一个BFS问题，需要多抽象

40. 核心：状态表达。其实如果要最短的状态表达，往往就是BFS。即可以将某一个状态看做是图中的某一个顶点
 
### Chapter 8 桥和割点

41. 如果删除了一条边，整个图的联通分量数量变化，则这条边称为桥（Bridge）

42. 桥就意味着图中最脆弱的关系

43. 图中可以有多个桥；树中多有的边都是桥

44. 环是图的属性，桥是边的属性，基本思路就是DFS遍历每一条边，判断每一条边是否是桥

45. 如何判断0-1是不是桥：看通过1，能否从另外一条路回到0，如果能，就不是桥；如果不能，0-1就是桥

46. 如何使得一个桥不再是桥？将其连接的两个分量再加一条边即可

47. 寻找桥的算法使用DFS就可以解决，其实就是Tarjan算法

48. 寻找桥不能使用DFS解决

49. 前向边 -> 生成树中的边，后向边 -> 可以指向自己的祖先节点

50. 删除割点以及相临边，图中的连通分量的数量发生变化 Cut points, Articulation points

51. 桥可以找割点，但是割点找不到桥

52. 对于割点来说，low[w] >= ord[v]就可以说明w是割点

53. 对于根节点来说，如果有两个或者两个以上的孩子，则根节点就是割点

### Chapter 9 哈密尔顿问题和状态压缩

54. 汉密尔顿回路即遍历完整张图回到原点，而且每个点只访问一次

55. 汉密尔顿路径就是遍历整张图，每个点只访问一次，此时起点的选择就很重要

56. TSP问题：以最小的代价在有权图中遍历完所有的点，而且每个点只能遍历一次

57. 本质上就是回溯法

58. 利用位操作实现状态压缩

59. 记忆化搜索 ^

### Chapter 10 欧拉回路和欧拉路径

60. 每条边都访问过一次并回到原点的路径叫做欧拉回路；如果不能回到原点，就叫欧拉路径

61. 每个点的度都是偶数 <=> 图存在欧拉回路，前提是无向连通图

62. 两个相连的环，一定可以组成一个新环

63. 对于无向连通图，如果所有的点的度都是偶数，先随便找一个环，如果有剩下的边，一定还是环

64. 两个相连的环，一定可以组成一个新环

65. 结合63和64，就能推出 所有点的度都是偶数 => 一定存在欧拉回路

### Chapter 11 最小生成树
66. Kruskal基本思路就是尽量使用最短边（贪心），只要当前遍历的边的两个顶点不在同一个并查集内，就添加到mst中

67. 切分定理：图中的顶点分成两个部分，称为一个切分；如果一个边的两个端点，属于不同的两个部分的两边，这条边称为横切边

68. 横切边中的最短边，一定属于最终的最小生成树的一部分

69. Kruskal的时间复杂度是O(ElogE)，即只跟边的数目有关

70. Prim算法：逐点遍历，将当前最短的横切边添加到mst中

71. Prim是逐点遍历，Kruskal思想是逐边遍历

72. Prim时间复杂度：O((V-1)*(V+E)) = O(VE)，可以优化为O(ElogE)，和Kruskal一样

73. 带权图的最小生成树才考虑使用Kruskal或者Prim算法，否则直接DFS或者BFS也可以

74. Prim其实可以优化成O(ElogV)，但是需要借助索引堆的数据结构

75. Fredman-Tarjan O(E + VlogV)

76. Chazelle O(E*) 比E高一点点

### Chapter 12 最短路径算法
77. 第一版非经优化的Dijkstra算法的复杂度是O(V^2)

78. 可以使用堆优化查找最小值的循环，优化成O(ElogE)，相当于对边来遍历

79. 求解最短路径：使用pre数组来判断每个点在哪儿来的

80. 可以用索引堆优化到O(ElogV)

81. Dijkstra不能处理处理负权边!!!

82. Bellman-ford可以求解负权边的图最短路径

83. Bellman-ford算法：
```
(1) 初始dis[s] = 0, 其余dis为正无穷
(2) 对所有边进行一次松弛操作，则求出了到所有点，经过的变数最多为1的最短路径
(3) 对所有边再进行一次松弛操作，则求出了到所有点，经过的边数最多为2的最短路径
(4) 对所有边进行V - 1次松弛操作，则求出了到所有点，经过的变数最多为V - 1的最短路径
(5) 最后再进行一次松弛操作，如果有更新最短距离dis，则肯定有负权环
```

84. Bellman-Ford时间复杂度：O(V*E)

85. Bellman-Ford如果只关注s到t之间的最短路径，不能提前终止

86. Bellman-Ford的优化(SPFA, shortest path fast algorithm)

87. Floyd算法：求所有点到所有点的最短距离，基本思路就是三重循环，t v w其中t是v和w的中间点

88. Floyd算法复杂度O(V^3)

### Chapter 13 有向图算法
89. floodfill, 最小生成树，桥和割点，二分图检测不会在有向图中考虑

90. DFS BFS Dijkstra Bellman-ford Floyd 有向图和无向图是一样的

91. 有向图的环检测和无向图很不一样；已经遍历过不代表环，需要再遍历过程中添加一个标记，表示某个点在当前路径上

92. DAG: directed acyclic graph

93. 有向图有入度和出度的概念

94. 有向图存在欧拉回路的充分必要条件：每个点的入度等于出度

95. 有向图存在欧拉路径的充分必要条件：除了两个点，其余每个点的入度等于出度，这两个点，一个入度比出度大1，一个出度比入度大1

96. 拓扑排序可以用来检测环，时间复杂度O(V+E)

97. 只有DAG才能进行拓扑排序

98. 拓扑排序还可以使用深度优先遍历后序遍历

99. 对于一个节点，遍历完所有相邻节点之后，再遍历它自身，注意最后的结果是反向的，需要reverse一下

100. 但是注意深度优先比那里后序遍历不能是有环，只能是在DAG(有向无环图)中才能用来求拓扑排序

101. 强连通分量：在有向图中，任意两点都可以（注意一定要是在有向图中！！！）

102. 一个有向图中，可能有多个强连通分量！

103. 可以将所有的强连通分量看做一个点，得到的有向图一定是一个DAG

104. Kosaraju：先做原图的反图（即将所有的有向边反向），再对这个反图做深度优先后序遍历

105. Kosaraju中"反图"的思路是核心中的核心！！！

106. Kosaraju求有向图中的强连通分量(scc)，时间复杂度O(V+E)

### Chapter14 网络流
106. 源点：入度为0，汇点：出度为0，有向非负带权图，权值表示容量（capacity）

107. 0/9 -> 表示流量为0，容量为9

108. 容量限制，平衡限制（对于每个点，流入量等于流出量）

109. Ford-Fulkerson思想:
- 有个虚拟的反向边(反向边最大容量是当前正向边的流量)
- 在残量图中不断找增广路径，直到没有增广路径为止
- 增广路径就是指还能找到的新的能通的路径

110. 反向可以逆流，但是有范围限制。比如现在是点1到2的路径是3/5，表示容量为5，当前流量为3，则反向只能是0~3之间的流量。因为如果超过3的反向，相当于当前整条路径是负值了，同理反向流量也不能小于0

111. 残量图（residual graph），正向图的权值是正向的容量-正向的当前流量，表示当前最多还能容纳多少；反向图的权值是正向边的当前流量，也能够表示当前反向能容纳多少

112. Edmonds-Karp算法
- 先构建残量图
- 使用BFS在残量图中寻找增广路径
- 该增广路径的最大流量是当前每条边上的权值的(记得权值是当前的最大流量，即表示当前还能容纳多少)的最小值
- 更新增广路径上的每条边的权值
- 继续寻找下一条增广路径，直到找不到增广路径为止
- (要区分原图的x/y和残量图权值的意义！！！)

113. Ford-Fulkerson思想：时间复杂度O(maxflow * E)

114. 具体到Edmonds-Karp算法：时间复杂度O(VE ^ 2)

