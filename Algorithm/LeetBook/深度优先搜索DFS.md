# 深度优先搜索DFS

[深度优先搜索 - LeetBook - 力扣（LeetCode](https://leetcode.cn/leetbook/detail/dfs/)

[博客链接](https://blog.csdn.net/qq_45806136/article/details/123478402?spm=1001.2014.3001.5501)

## 勇往直前的深度优先遍历

**一条路走到底，不撞南墙不回头**，是对**深度优先遍历**的最直观描述。下面演示了以深度优先遍历的方式走迷宫找出口的搜索轨迹。
![在这里插入图片描述](https://img-blog.csdnimg.cn/795928ef488d4e0c8c92f7a244a950a8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_18,color_FFFFFF,t_70,g_se,x_16)

## 1、树的深度优先搜索遍历
## （1） 二叉树的先序遍历
![二叉树的先序遍历](https://img-blog.csdnimg.cn/a00de968d2614c219f4aeb1bd5475952.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)
## （2） 二叉树的中序遍历
![二叉树的中序遍历](https://img-blog.csdnimg.cn/aac1196996584cf6acbf1961f463b5c9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)
## （3） 二叉树的后序遍历
![二叉树的后序遍历](https://img-blog.csdnimg.cn/83672bd79f124deb88d4d02d405d716b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)

##   三种遍历方式的比较

![在这里插入图片描述](https://img-blog.csdnimg.cn/92f98ce1366448ef84c80ee7326d0df1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)
## （4）递归算法和非递归算算法的转化
![在这里插入图片描述](https://img-blog.csdnimg.cn/4882c092cb974ce390fb8a7179e838aa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)


![在这里插入图片描述](https://img-blog.csdnimg.cn/04e5ef1d21d34e95a3c48927d3212a30.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/b927e15bee9f4d89adb8287cb9ef93fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ffd74a6c59c9489683a6ac0c04a9e3c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aaC6aOO55qE5bCR5bm0LQ==,size_20,color_FFFFFF,t_70,g_se,x_16)


力扣相关例题：
### 1、[二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)   
两行代码搞定：
```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root)  return 0;   //递归的边界条件
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
        //当root不空，返回左右子树高度的最大值+1
    }
};
```

### 2、[二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
|  | 
**这里需要注意题目的要求：最小深度是从根节点到最近叶子节点的最短路径上的节点数量。**
**如果直接将上题的max换成min是不对的，因为他返回的是左右子树的最小高度+1，而左右子树不一定存在，题目要求的是根节点到叶子节点的距离，所以对应的结点必须存在**
那么分情况讨论即可：
**对于一个节点而言，无非就是5种情况：空、叶子结点、有左右子树、有左子树、有右子树**

```c++
class Solution {
public:
    int minDepth(TreeNode* root) {
        if (!root)   return 0;   //空节点
        else if (!root->left && !root->right)  return 1;//叶子结点
        else if (root->left && root->right)   return min(minDepth(root->left), minDepth(root->right)) + 1; //左右子树均不空
        else if (root->left)    return minDepth(root->left)+1;	//只有左子树
        else    return minDepth(root->right)+1;	//只有右子树
    }
};
```
### 3、[路径总和](https://leetcode-cn.com/problems/path-sum/)
```c++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root)  return false;	//空节点
        else if (!root->left && !root->right)   return root->val == targetSum;	//叶子结点
        else  
            return hasPathSum(root->left, targetSum-root->val) || hasPathSum(root->right, targetSum-root->val);
    }
};
```
### 4、[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/submissions/)

```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (!root)  return root;
        TreeNode *left = invertTree(root->left);
        TreeNode *right = invertTree(root->right);
        root->right = left;
        root->left = right;
        return root;
    }
};
```
### 5、[相同的树](https://leetcode-cn.com/problems/same-tree/submissions/)
[对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/submissions/)
```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root)  return true;
        return isSame(root->left, root->right);
    }

    bool isSame(TreeNode *p, TreeNode *q){
        //判断p、q两棵子树是否对称
        if (!p && !q)   return true;    //两棵子树均为空
        else if (p && q)                //两个子树均不空时，判断根节点是否相等、左右子树是否对称
            return (p->val == q->val) && isSame(p->left, q->right) && isSame(p->right, q->left);
        else    return false;
    }
};
```
### 6、[求根节点到叶节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)
```c++
class Solution {
private:
    int res = 0;
public:
    int sumNumbers(TreeNode* root) {
        DFS(0, root);
        return res;
    }

    void DFS(int last, TreeNode* root){//实际上是一个二叉树先序遍历的过程
        //last表示到达root之前的数值
        if (!root)  return ;
        last = last*10 + root->val;//这一步的位置很关键，如果直接放在上一行代码之前，会报错，因为root可能为空，root->val不存在
        if (!root->left && !root->right)  {
            //递归出口是叶子结点，当到达叶子结点之后说明从根节点到当前的叶子结点已经可以构成一个数，让res += last 
            res += last;
            return ; 
        }
        DFS(last, root->left);
        DFS(last, root->right);
    }
};
```
### 7、[二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || p == root || q == root) return root;//递归出口
        TreeNode *left = lowestCommonAncestor(root->left, p, q);//在左子树上寻找最低公共祖先节点
        TreeNode *right = lowestCommonAncestor(root->right, p, q);//在右子树上寻找
        if (left && right)  return root;
        else if (!left && !right)   return NULL;
        else if (left)  return left;
        else return right;    
    }
};
```
### 8、[根据前序和后序遍历构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)
```c++
class Solution {
private:
    vector<int> preorder, inorder;
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        this->preorder = preorder;
        this->inorder = inorder;
        int n = preorder.size();
        return build(0, n-1, 0, n-1);
    }

    TreeNode* build(int l1, int h1, int l2, int h2){
        TreeNode *root= new TreeNode(preorder[l1]);
        int i;
        for (i = l2; preorder[l1] != inorder[i]; i++);
        int llen = i-l2, rlen = h2-i;
        if (llen){
            root->left = build(l1+1, l1+llen, l2, l2+llen-1);
        }
        if (rlen){
            root->right = build(h1-rlen+1, h1, h2-rlen+1, h2);
        }
        return root;
    }
};
```
### 9、[从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
```c++
class Solution {
private:
    vector<int> inorder, postorder;
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        this->postorder = postorder;
        this->inorder = inorder;
        int n = postorder.size();
        return build(0, n-1, 0, n-1);
    }

    TreeNode* build(int l1, int h1, int l2, int h2){
        TreeNode *root= new TreeNode(postorder[h2]);
        int i;
        for (i = l1; postorder[h2] != inorder[i]; i++);
        int llen = i-l1, rlen = h1-i;
        if (llen){
            root->left = build(l1, l1+llen-1, l2, l2+llen-1);
        }
        if (rlen){
            root->right = build(h1-rlen+1, h1, h2-rlen, h2-1);
        }
        return root;
    }
};
```
### 10、[前序遍历构造二叉搜索树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)
```c++
class Solution {
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        TreeNode* root = nullptr;
        for (int x : preorder)  insert(root, x);
        return root;
    }

    void insert(TreeNode* &root, int x){
        if (!root){
            root = new TreeNode(x);
            return ;
        }
        if (root->val > x)  insert(root->left, x);
        else if (root->val < x) insert(root->right, x);
    }
};
```

## 数据结构 - 栈

![image.png](https://pic.leetcode-cn.com/1612150162-rhKxZd-image.png)

### 深度优先遍历的两种实现方式

在深度优先遍历的过程中，需要将当前遍历到的结点的相邻结点暂时保存起来，以便在回退的时候可以继续访问它们。遍历到的结点的顺序呈现「后进先出」的特点，因此 深度优先遍历可以通过「栈」实现。

再者，深度优先遍历有明显的递归结构。我们知道支持递归实现的数据结构也是栈。因此实现深度优先遍历有以下两种方式：

**编写递归方法；**
**编写栈，通过迭代的方式实现。**

![image.png](https://pic.leetcode-cn.com/1608890373-CFNdrG-image.png)

#### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
private:
    vector<int> res;
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (!root)  return res;
        stack<TreeNode *> st;   //辅助栈
        TreeNode *p = root;
        while(p || st.size()){
            if (p){
                res.push_back(p->val);
                st.push(p);
                p = p->left;
            }else{
                p = st.top();
                st.pop();
                p = p->right;
            }
        }
        return res;
    }
};
```

#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```c++
class Solution {
private:
    vector<int> res;
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode *>st;
        TreeNode *p = root;
        while(p || st.size()){
            if (p){
                st.push(p);
                p = p->left;
            }else{
                p = st.top();
                res.push_back(p->val);
                st.pop();
                p = p->right;
            }
        }
        return res;
    }
};
```

#### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```c++
class Solution {
private:
    vector<int> res;
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode *>st;
        TreeNode *p = root, *r = nullptr;    //上一次被访问过的节点
        while(p || st.size()){
            if (p){
                st.push(p);
                p = p->left;
            }else{
                p = st.top();
                if (p->right && p->right != r){//有右孩子并且有孩子还未被访问过
                    p = p->right;
                }else{
                    st.pop();
                    res.push_back(p->val);
                    r = p;
                    p = nullptr;    //以p为根的子树已经访问完毕
                }
            }
        }
        return res;
    }
};
```

[589. N 叉树的前序遍历](https://leetcode.cn/problems/n-ary-tree-preorder-traversal/)

递归写法：

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
private:
    vector<int> res;
public:
    vector<int> preorder(Node* root) {
        DFS(root);
        return res;
    }

    void DFS(Node* root){
        if (!root)  return;
        res.push_back(root->val);
        for (Node * p : root->children){
            DFS(p);
        }
    }
};
```

迭代写法：

方法一中利用递归来遍历树，实际的递归中隐式调用了栈，在此我们可以直接模拟递归中栈的调用。在前序遍历中，我们会先遍历节点本身，然后从左向右依次先序遍历该每个以子节点为根的子树。

在这里的栈模拟中比较难处理的在于从当前节点 u的子节点v1返回时，此时需要处理节点u的下一个节点v2，此时需要记录当前已经遍历完成哪些子节点，才能找到下一个需要遍历的节点。在二叉树，树中因为只有左右两个子节点，因此比较方便处理，在 N叉树中由于有多个子节点，因此使用哈希表记录当前节点u已经访问过哪些子节点。

每次入栈时都将当前节点的u的第一个子节点压入栈中，直到当前节点为空节点为止。
每次查看栈顶元素 pp，如果节点p的子节点已经全部访问过，则将节点p的从栈中弹出，并从哈希表中移除，表示该以该节点的子树已经全部遍历过；如果当前节点p的子节点还有未遍历的，则将当前节点的p的下一个未访问的节点压入到栈中，重复上述的入栈操作。

```c++
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> res;
        if (!root)  return res;
        stack<Node *> st;
        Node *p = root;
        st.push(p);
        while(st.size()){
            p = st.top();
            res.push_back(p->val);
            st.pop();
            for (int i = p->children.size()-1; i >= 0; i--){
                st.push(p->children[i]);
            }
        }
        return res;
    }
};
```

#### [590. N 叉树的后序遍历](https://leetcode.cn/problems/n-ary-tree-postorder-traversal/)

```c++
class Solution {
private:
    vector<int> res;
public:
    vector<int> postorder(Node* root) {
        if (!root)  return res;
        stack<Node *> st;
        Node *p = root;
        st.push(p);
        while(st.size()){
            p = st.top();
            st.pop();
            res.push_back(p->val);
            for (int i = 0; i < p->children.size(); i++){
                st.push(p->children[i]);
            }
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

![image-20220601190124551](C:\Users\hp-pc\AppData\Roaming\Typora\typora-user-images\image-20220601190124551.png)

## 深度优先遍历的应用

![image.png](https://pic.leetcode-cn.com/1611530242-omrkWW-image.png)

#### [323. 无向图中连通分量的数目](https://leetcode.cn/problems/number-of-connected-components-in-an-undirected-graph/)

DFS

```c++
class Solution {
private:
    static const int N = 2010;
    vector<int> Adj[N];           //这里n最大可以到达2000，所以使用邻接表来存储图
    bool visited[N] = {false};    //这里用visited数组来表示顶点是否被访问
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        for (auto edge : edges){
            int u = edge[0], v = edge[1];
            Adj[u].push_back(v);
            Adj[v].push_back(u);
        }
        int res = 0;
        for (int i = 0; i < n; i++){
            if (!visited[i]){
                res++;
                DFS(i);
            }
        }
        return res;
    }

    void DFS(int v){
        visited[v] = true;
        for (int i = 0; i < Adj[v].size(); i++){
            int w = Adj[v][i];
            if (!visited[w])   DFS(w);
        }
        return ;
    }
};
```

并查集（速度很快）

```c++
class Solution {
private:
    int father[2010];
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        for (int i = 0; i < n; i++)
            father[i] = i;
        for (vector<int> edge : edges){
            int a = edge[0], b = edge[1];
            Union(a, b);
        }
        int res = 0;
        for (int i = 0; i < n; i++){
            if (father[i] == i)
                res++;
        }
        return res;
    }

    int findFather(int x){ 
        if (x != father[x])
            return findFather(father[x]);
        return father[x];
    }

    void Union(int a, int b){
        int faA = findFather(a), faB = findFather(b);
        if (faA != faB){
            father[faA] = faB;
        }
    }
};
```

BFS

```c++
class Solution {
private:
    static const int N = 2010, INF = 0x3f3f3f3f;
    int n, G[N][N];    //使用邻接矩阵来存储图
    bool visited[N] = {false};
public:
    int countComponents(int n, vector<vector<int>>& edges) {
        this->n = n;
        memset(G, 0x3f, sizeof G);
        for (vector<int> edge : edges){
            int u = edge[0], v = edge[1];
            G[u][v] = G[v][u] = 1;  //这里假设他们之间存在边对应的G[u][v] = G[v][u] = 1
        }
        int res = 0;
        for (int i = 0; i < n; i++){
            if (!visited[i]){ 
                res++;
                BFS(i);
            }
        }
        return res;
    }

    void BFS(int v){
        queue<int> q;
        q.push(v);
        visited[v] = true;
        while(q.size()){
            v = q.front();
            q.pop();
            for (int u = 0; u < n; u++){
                if (G[v][u] != INF && !visited[u]){
                    visited[u] = true;
                    q.push(u);
                }
            }
        }
    }
};
```



