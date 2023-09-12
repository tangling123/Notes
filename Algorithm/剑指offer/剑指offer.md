# 剑指offer

## 1 [栈与队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

### （1）用两个栈实现队列

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 -1 )
>
> 输入：
>
> ```
> ["CQueue","appendTail","deleteHead","deleteHead","deleteHead"]
> [[],[3],[],[],[]]
> ```
>
> 输出：
>
> ```
> [null,null,3,-1,-1]
> ```
>
> - `1 <= values <= 10000`
> - 最多会对` appendTail、deleteHead `进行` 10000` 次调用

思路：用两个栈可以模拟队列的实现，栈s1用来存储队列中的元素，辅助栈s2当需要弹出队头元素时，将s1中的内容全部pop到s2，最后s2的栈顶元素就是队头元素，再讲s2中的元素pop回s1，`deleteHead`的时间复杂度O(n)

```c++
class CQueue {
private:
    stack<int> s1, s2;
public:
    CQueue() {

    }
    
    void appendTail(int value) {
        s1.push(value);
    }
    
    int deleteHead() {
        if (s1.empty()) 
            return -1;
        while (s1.size()){
            s2.push(s1.top());
            s1.pop();
        }
        int temp = s2.top();
        s2.pop();
        while (s2.size()){
            s1.push(s2.top());
            s2.pop();
        }
        return temp;
    }
};
```



### （2）[包含min函数的栈](https://leetcode.cn/problems/bao-han-minhan-shu-de-zhan-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
>
> **示例:**
>
> ```
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.min();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.min();   --> 返回 -2.
> ```
>
> **提示：**
>
> 1. 各函数的调用总次数不超过 20000 次

1）、使用两个辅助栈

栈`st`存储栈中的元素，`min_st`存储每次入栈之后元素的最小值，当栈`st`中的元素pop出栈时，同时将`min_st`中的元素pop

![fig1](剑指offer.assets/jianzhi_30.gif)

在min_st初始化时，将INT_MAX入栈，就能保证min_st中至少存在一个元素，不用判断栈是否为空

```c++
class MinStack {
    stack<int> st, min_st;
public:
    /** initialize your data structure here. */
    MinStack() {
        min_st.push(INT_MAX);
    }
    
    void push(int x) {
        st.push(x);
        min_st.push( x <= min_st.top() ? x :  min_st.top());
    }
    
    void pop() {
        st.pop();
        min_st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int min() {
        return min_st.top();
    }
};
```



2）只实用一个栈

只使用一个栈，对于栈中的元素而言，只有当前待入栈的元素`val < min`时，才需要更新栈中最小的元素。但如何在最小的元素出栈之后，以O(1)的时间来找到出栈后的最小元素？

[solution using one stack](https://leetcode.com/problems/min-stack/solutions/49014/java-accepted-solution-using-one-stack/?orderBy=most_votes)

当`val <= min`时，min元素会被更新，我们应该还保存之前的min，这样当最小的元素出栈后，可以以O(1)的时间来找到最小的元素。

[详细通俗的思路分析，多解法](https://leetcode.cn/problems/min-stack/solutions/42521/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-38/)

```c++
入栈 3，存入 3 - 3 = 0
|   |   min = 3
|   |     
|_0_|    
stack   

入栈 5，存入 5 - 3 = 2
|   |   min = 3
| 2 |     
|_0_|    
stack  

入栈 2，因为出现了更小的数，所以我们会存入一个负数，这里很关键
也就是存入  2 - 3 = -1, 并且更新 min = 2 
对于之前的 min 值 3, 我们只需要用更新后的 min - 栈顶元素 -1 就可以得到    
| -1|   min = 2
| 5 |     
|_3_|    
stack  

入栈 6，存入  6 - 2 = 4
| 4 |   min = 2
| -1| 
| 5 |     
|_3_|    
stack  

出栈，返回的值就是栈顶元素 4 加上 min，就是 6
|   |   min = 2
| -1| 
| 5 |     
|_3_|    
stack  

出栈，此时栈顶元素是负数，说明之前对 min 值进行了更新。
入栈元素 - min = 栈顶元素，入栈元素其实就是当前的 min 值 2
所以更新前的 min 就等于入栈元素 2 - 栈顶元素(-1) = 3
|   | min = 3
| 5 |     
|_3_|    
stack     
```

代码

```c++
class MinStack {
    stack<int> st;
    int MIN = INT_MAX;
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    
    void push(int x) {
        if (x <= MIN){
            st.push(MIN);
            MIN = x;
        }
        st.push(x);
    }
    
    void pop() {
        if (MIN == st.top()){
            st.pop();
            MIN = st.top();
        }
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int min() {
        return MIN;
    }
};
```



## 2 链表（简单）

### （1）[从尾到头打印链表](https://leetcode.cn/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
> **示例 1：**
>
> ```
> 输入：head = [1,3,2]
> 输出：[2,3,1]
> ```
>
> **限制：**
>
> ```
> 0 <= 链表长度 <= 10000
> ```

1）、利用reverse函数，将结果进行反转

```c++
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int> res;
        while (head){
            res.push_back(head->val);
            head = head->next;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```



2）利用递归的特性

```c++
class Solution {
public:
    //使用递归
    vector<int> res;
    vector<int> reversePrint(ListNode* head) {
        dfs(head);
        return res;
    }

    void dfs(ListNode *head){
        if (!head)  return ;
        dfs(head->next);
        res.push_back(head->val);
    }
};
```



### （2）[反转链表](https://leetcode.cn/problems/fan-zhuan-lian-biao-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
>
> **示例:**
>
> ```
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> ```
>
> **限制：**
>
> ```
> 0 <= 节点个数 <= 5000
> ```

1）尾插法建立单链表

```c++
class Solution {
public:
    // 1、使用头插法反转单链表
    // 2、递归
    ListNode* reverseList(ListNode* head) {
        ListNode* dummy = new ListNode(-1), *p;
        while (head){
            p = head->next;
            head->next = dummy->next, dummy->next = head;
            head = p;
        }
        return dummy->next;
    }
};
```

时间复杂度O(n)，空间复杂度O(1)



2）递归

**注意递归边界以及递归的方式**

```c++
class Solution {
public:
    // 1、使用头插法反转单链表
    // 2、递归  
    ListNode* reverseList(ListNode* head) {
        return dfs(NULL, head);
    }

    ListNode* dfs(ListNode* pre, ListNode* cur){
        if (!cur)
            return pre;
        ListNode* res = dfs(cur, cur->next);	//cur反转之后的 cur <- res
        cur->next = pre;
        return res;
    }
};
```



3）反转指针



<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\fig\RGIF2.gif" alt="reverting a list" style="zoom:67%;" />

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL, *next;
        while (head){
            next = head->next;  //保存head的后继
            head->next = pre;
            pre = head;
            head = next; 
        }
        return pre;
    }
};
```



### （3）[复杂链表的复制](https://leetcode.cn/problems/fu-za-lian-biao-de-fu-zhi-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

1）使用哈希表

时间复杂度O(N)，空间复杂度O(N)

首先创建一个哈希表，再遍历原链表，遍历的同时再不断创建新节点
我们将原节点作为**key**，新节点作为**value**放入哈希表中

<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\5b578a2e33a4f87536c7fe50f71ac01904ae689b26ee3e2751dac0144f009d77-8.jpg" alt="8.jpg" style="zoom:67%;" />

第二步我们再遍历原链表，这次我们要将新链表的next和random指针给设置上

<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\ec904c68195c9e8741e9b3302133f7def57fc4f2a02521985e08cfd92fefc67a-9.jpg" alt="9.jpg" style="zoom:67%;" />

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> mp;
        Node* cur = head;
        while (cur){
            mp[cur] = new Node(cur->val);
            cur = cur->next;
        }
        cur = head;
        while (cur){
            mp[cur]->next = mp[cur->next];
            mp[cur]->random = mp[cur->random];
            cur = cur->next;
        }
        return mp[head];
    }
};
```



2）

[两种实现+图解 138. 复制带随机指针的链表](https://leetcode.cn/problems/copy-list-with-random-pointer/solutions/295083/liang-chong-shi-xian-tu-jie-138-fu-zhi-dai-sui-ji-/)

**第一步**，根据遍历到的原节点创建对应的新节点，每个新创建的节点是在原节点后面，比如下图中原节点**1**不再指向原原节点**2**，而是指向新节点**1**

![5.jpg](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\360dbd3b89c25324287f4cef2c22ba8a20e946891ac887f70703b211893aafa0-5.jpg)

**第二步**是最关键的一步，用来设置新链表的随机指针

![6.jpg](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\b531fb496fd478a2db6ba7bc805cda08b825771817dd24cdd616946a89800fbb-6-16778518768985.jpg)

上图中，我们可以观察到这么一个规律

- 原节点1的随机指针指向原节点3，新节点1的随机指针指向的是原节点3的next
- 原节点3的随机指针指向原节点2，新节点3的随机指针指向的是原节点2的next

也就是，原节点`i`的随机指针(如果有的话)，指向的是原节点`j`
**那么新节点`i`的随机指针，指向的是原节点`j`的next**

**第三步**就简单了，只要将两个链表分离开，再返回新链表就可以

![7.jpg](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\9b5c6e99aa89284c8a7b423bc36fec7af39fac3f8bb709e77483e574e02ef1cd-7.jpg)

时间复杂度O(N)，空间复杂度O(1)

```c++
class Solution {
public:
    Node* copyRandomList(Node* head) {
        Node* dummy = new Node(-1), *p = head, *r;
        //1、创建一个新的链表
        while (p){
            Node* newNode = new Node(p->val);
            newNode->next = p->next;
            p->next = newNode;
            p = newNode->next;
        }

        //2、复制random
        p = head;
        while (p){
            if (p->random)  //核心
                p->next->random = p->random->next;
            p = p->next->next;
        }

        p = head, r = dummy;
        while (p){
            r->next = p->next;
            r = p->next;
            p->next = r->next;
            p = p->next;
        }

        return dummy->next;
    }
};
```



## 3 字符串（简单）

### （1）[替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/solutions/)

> 请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。
>
> **示例 1：**
>
> ```
> 输入：s = "We are happy."
> 输出："We%20are%20happy."
> ```

1）边扫描边替换

```c++
class Solution {
public:
    string replaceSpace(string s) {
        string res = "";
        for (char ch : s){
            if (ch != ' ')
                res += ch;
            else    res += "%20";
        }
        return res;
    }
};
```



2）利用函数直接在s中进行替换

```c++
string& replace (size_t pos, size_t len, const string& str);
```

- `pos`：要替换的字符串的起始位置。
- `len`：要替换的字符串的长度。
- `str`：要插入的新字符串。

```c++
class Solution {
public:
    string replaceSpace(string s) {
        while (s.find(" ") != string::npos){
            s.replace(s.find(" "), 1, "%20");
        }
        return s;
    }
};
```



3）修改s的长度，进行替换

<img src="https://pic.leetcode-cn.com/1599931882-pPgkor-Picture6.png" alt="Picture6.png" style="zoom: 33%;" />

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int cnt = 0, n = s.size();
        for (char ch : s){
            if (ch == ' ')  cnt++;
        }
        s.resize(n+2*cnt);  //字符串扩展
        for (int i = n-1, j = s.size()-1; i>= 0 && i != j; ){
            if (s[i] != ' ')
                s[j--] = s[i--];
            else{
                s[j] = '0', s[j-1] = '2', s[j-2] = '%';
                j -= 3, i--; 
            }
        }
        return s;
    }
};
```



### （2）[左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/) 

> 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。
>
> **示例 1：**
>
> ```
> 输入: s = "abcdefg", k = 2
> 输出: "cdefgab"
> ```
>
> **示例 2：**
>
> ```
> 输入: s = "lrloseumgh", k = 6
> 输出: "umghlrlose"
> ```

1）字符串拼接

时间复杂度O(N)，空间复杂度O(1)

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string res = "";
        for (int i = n; i < s.size(); i++)    
            res += s[i];
        for (int i = 0; i < n; i++)
            res += s[i];
        return res;
    }
};
```



2）两次翻转

`abcdefg, k = 2`

将前k个字符和剩下的字符翻转，`bagfedc`

再将整个字符串翻转`cdefgab`

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```





## 4 查找算法（简单）

### （1）[数组中重复的数字](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 找出数组中重复的数字。
> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
> **示例 1：**
>
> ```
> 输入：
> [2, 3, 1, 0, 2, 5, 3]
> 输出：2 或 3 
> ```
>
> **限制：**
>
> ```
> 2 <= n <= 100000
> ```

1）使用哈希表

利用`hashTable[MAXN]`来统计每个数字出现的次数，当`nums[i]`的出现次数>1，直接将`nums[i]`返回

时间复杂度$O(N)$，空间复杂度$O(N)$

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int hashTable[100010] = {0};
        for (int i = 0; i < nums.size(); i++){
            if (hashTable[nums[i]])
                return nums[i];
            hashTable[nums[i]]++;
        }
        return -1;
    }
};
```



**2）充分挖掘题目信息**

[题解](https://leetcode.cn/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solutions/96623/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/)

可以注意到`nums`中的数字都在`[0, n-1]`之间，刚好和下标相对应

<img src="https://pic.leetcode-cn.com/1618146573-bOieFQ-Picture0.png" alt="Picture0.png" style="zoom: 50%;" />

遍历中，第一次遇到数字 x 时，将其交换至索引 x 处；而当第二次遇到数字 `x` 时，一定有 `nums[x]=x` ，此时即可得到一组重复数字。

**算法流程：**

1. 遍历数组`nums` ，设索引初始值为 `i=0`:

   1. **若 `nums[i]=i ：`** 说明此数字已在对应索引位置，无需交换，因此跳过；

   2. **若` nums[nums[i]]=nums[i]` ：** 代表索引`nums[i]`处和索引 `i`处的元素值都为 `nums[i]`，即找到一组重复值，返回此值 `nums[i] `；

   3. **否则：** 交换索引为 `i` 和 `nums[i]`的元素值，将此数字交换至对应索引位置。

      `swap(nums[i], nums[nums[i]])`能够将索引为`i`上的`nums[i]`元素交换到索引为`nums[i]`的位置上

2. 若遍历完毕尚未返回，则返回 −1 。

**复杂度分析：**

- **时间复杂度 O(N)：** 遍历数组使用 O(N)，每轮遍历的判断和交换操作使用 O(1) 。
- **空间复杂度 O(1) ：** 使用常数复杂度的额外空间。

```
数组				 [2, 3, 1, 0, 2, 5, 3]
i = 0时，nums[0] != 0，判断nums[nums[i]] == nums[i]，现在不等于，将元素进行交换
				  [1, 3, 2, 0, 2, 5, 3]
对于i=0而言，当前位置上的元素仍不满足nums[i] = i，继续判断是否nums[i]位置上的元素等于i位置上的元素
				  [3, 1, 2, 0, 2, 5, 3]
下一步				[0, 1, 2, 3, 2, 5, 3]
每次swap(nums[i], nums[nums[i]])能够将索引为i上的nums[i]元素交换到索引为nums[i]的位置上
i++,
当i=4时，nums[nums[4]] == nums[4]时，返回nums[i]
```

简而言之，因为数组中元素都在`[0, n-1]`

那么我们通过交换，使得`nums[i] = i`，但数组中存在重复的元素必定使得有多个元素对应同一个下标

对于索引为`i`上的`nums[i]`元素

如果`i == nums[i]`，那么`i++`，元素已经在对应的位置上

否则，就将索引为`i`和`nums[i]`位置上的元素作比较（因为不满足`i == nums[i]`，那么对于索引为`i`位置上的元素`nums[i]`的位置应该在`nums[nums[i]]`），如果两者相等说明已经找到了重复元素

再将索引为`i`和`nums[i]`位置上的元素进行交换（每次交换之后都可以使索引为`nums[i]`的元素放在正确的位置`nums[nums[i]]`上）



**explanation:**

> 这个原地交换法就相当于分配工作，每个索引代表一个工作岗位，每个岗位必须专业对口，既0索引必须0元素才能上岗。而我们的目的就是找出溢出的人才，既0索引岗位有多个0元素竞争。
>
> 我们先从0索引岗位开始遍历，首先我们看0索引是不是已经专业对口了，如果已经专业对口既`nums[0]=0`，那我们就跳过0岗位看1岗位。如果0索引没有专业对口，那么我们看现在0索引上的人才调整到他对应的岗位上，比如num[0]=2，那我们就把2这个元素挪到他对应的岗位上既num[2]，这个时候有两种情况:
>
> 1、num[2]岗位上已经有专业对口的人才了，既num[2]=2，这就说明刚刚那个在num[0]上的2是溢出的人才，我们直接将其返回即可。
>
> 2、num[2]上的不是专业对口的人才，那我们将num[0]上的元素和num[2]上的元素交换，这样num[2]就找到专业对口的人才了。
>
> 之后重复这个过程直到帮num[0]找到专业对口的人才，然后以此类推帮num[1]找人才、帮num[2]找人才，直到找到溢出的人才。

**代码**

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        for (int i = 0; i < nums.size(); ){
            if (nums[i] == i){ 
                i++;
                continue;
            }
            if (nums[nums[i]] == nums[i])
                return nums[i];
            swap(nums[i], nums[nums[i]]);
        }
        return -1;
    }
};
```



（2）[在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/description/)


1）二分+库函数

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        return upper_bound(nums.begin(), nums.end(), target) -lower_bound(nums.begin(), nums.end(), target);
    }
};
```

2）手写二分

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        return lower_bound(nums, target+1) - lower_bound(nums, target);
    }

    int lower_bound(const vector<int> &nums, int target){
        //寻找数组中>=target的第一个位置
        int low = 0, high = nums.size();
        while (low < high){
            int mid = (low + high) / 2;
            if (nums[mid] >= target)    high = mid;
            else    low = mid+1;
        }
        return low;
    }
};
```



### （3）[0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/description/) ❤

1）哈希表

2）利用数组中的元素递增并且缺失的是最小未出现的元素

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int i;
        for(i = 0; i < nums.size(); i++) {
            if(nums[i] != i) {
                return i;
            }
        }
        return i;
    }
};
```

3）二分查找

对于有序数组而言，使用二分查找可以到达$O(logN)$的时间

[算法解析：](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/solutions/155915/mian-shi-ti-53-ii-0n-1zhong-que-shi-de-shu-zi-er-f/)

1. **初始化：** 左边界`i=0 `，右边界` j=len(nums)−1`；代表闭区间 `[i,j] `。
2. 循环二分： 当`i≤j`时循环，（即当闭区间 `[i,j]`为空时跳出）；
   1. 计算中点 `m=(i+j)/2` ；
   2. 若 `nums[m]=m` ，则 “右子数组的首位元素” 一定在闭区间`[m+1,j]` 中，因此执行` i=m+1`
   3. 若 `nums[m]≠m` ，则 “左子数组的末位元素” 一定在闭区间 `[i,m−1]` 中（缺失了元素，必定达不到m），因此执行 j=m−1
3. **返回值：** 跳出时，变量 `i` 和 `j`分别指向 “右子数组的首位元素” 和 “左子数组的末位元素” 。因此返回`i`即可。

<img src="https://pic.leetcode-cn.com/1cdbdb450af28f6db2aa8a1c8530a7d96b8293779392ee29efb47b025317851e-Picture7.png" alt="img" style="zoom:50%;" />

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        //二分查找：未缺失元素的性质nums[i] == i
        int left = 0, right = nums.size()-1;
        while (left <= right){
            int mid = (left + right) >> 1;
            if (nums[mid] == mid)
                left = mid+1;
            else
                right = mid-1;    
        }
        return left;
    }
};
```



## 5 查找算法（中等）

### （1）[二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 在一个 n * m 的二维数组中，每一行都按照从左到右 **非递减** 的顺序排序，每一列都按照从上到下 **非递减** 的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
>
> **示例:**
>
> 现有矩阵 matrix 如下：
>
> ```
> [ [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]]
> ```
>
> 给定 target = `5`，返回 `true`。
>
> 给定 target = `20`，返回 `false`。
>
> **限制：**
>
> ```
> 0 <= n <= 1000
> 0 <= m <= 1000
> ```

1）遍历矩阵

**时间复杂度$O(n * m)$，空间复杂度$O(1)$**



2）二分

矩阵每一行的元素都是递增有序的

**时间复杂度$O(n * log_m)$，空间复杂度$O(1)$**

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0) return false;
        int m = matrix.size(), n = matrix[0].size();
        for (vector<int> row : matrix){
            int j = lower_bound(row.begin(), row.end(), target) - row.begin();
            if (j < n && row[j] == target)
                return true;
        }
        return false;
    }
};
```



3）将矩阵看成二叉树

从左下角(m-1, 0)或者右上角(0, n-1)出发，将矩阵看成一棵二叉树

充分利用矩阵每一行有序、每一列有序

**时间复杂度$O(n + m)$，空间复杂度$O(1)$**

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.size() == 0) return false;
        int m = matrix.size(), n = matrix[0].size();
        int row = m-1, col = 0;
        while (row >= 0 && col < n){
            if (matrix[row][col] == target)
                return true;
            else if (matrix[row][col] > target)
                row--;
            else
                col++;
        }
        return false;
    }
};
```



### （2） **旋转数组的最小数字❤**

> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。
>
> 给你一个可能存在 **重复** 元素值的数组 `numbers` ，它原来是一个升序排列的数组，并按上述情形进行了一次旋转。请返回旋转数组的**最小元素**。例如，数组 `[3,4,5,1,2]` 为 `[1,2,3,4,5]` 的一次旋转，该数组的最小值为 1。 
>
> 注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` 旋转一次 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。
>
> **示例 1：**
>
> ```
> 输入：numbers = [3,4,5,1,2]
> 输出：1
> ```
>
> **示例 2：**
>
> ```
> 输入：numbers = [2,2,2,0,1]
> 输出：0
> ```
>
> **提示：**
>
> - `n == numbers.length`
> - `1 <= n <= 5000`
> - `-5000 <= numbers[i] <= 5000`
> - `numbers` 原来是一个升序排序的数组，并进行了 `1` 至 `n` 次旋转

1）遍历

**时间复杂度$O(n)$，空间复杂度$O(1)$**

```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        return *min_element(numbers.begin(), numbers.end());
    }
};
```



**2）二分**

[题解](https://leetcode.cn/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/solutions/102826/mian-shi-ti-11-xuan-zhuan-shu-zu-de-zui-xiao-shu-3/)

如下图所示，寻找旋转数组的最小元素即为寻找 右排序数组 的首个元素 `nums[x]` ，称 x 为 旋转点 。

<img src="https://pic.leetcode-cn.com/1599404042-JMvjtL-Picture1.png" alt="Picture1.png" style="zoom:50%;" />

排序数组的查找问题首先考虑使用 二分法 解决，其可将 遍历法 的 线性级别 时间复杂度降低至 对数级别 。

**算法流程：**
初始化： 声明 `i, j` 双指针分别指向 `nums`数组左右两端；
循环二分： 设 `m=(i+j)/2` 为每次二分的中点（ 因此恒有 `i ≤ m < j`），可分为以下三种情况：
当 `nums[m]>nums[j]`时： `m` 一定在 左排序数组 中，即旋转点 `x` 一定在`[m + 1, j]`闭区间内，因此执行 `i=m+1`；
当 `nums[m]<nums[j]`时： `m` 一定在 右排序数组 中，即旋转点 `x` 一定在`[i,m]` 闭区间内，因此执行 `j=m`；
当 `nums[m]=nums[j]`时： 无法判断 `m` 在哪个排序数组中，即无法判断旋转点 `x `在 `[i,m] `还是 `[m+1,j]` 区间中。

解决方案： 执行 `j=j−1`缩小判断范围，分析见下文。
返回值： 当`i=j` 时跳出二分循环，并返回 旋转点的值 `nums[i]` 即可。



**为什么是nums[m]和右端点nums[j]比较**：

如果`nums[m]`和`左端点nums[i]`比较，那么可能无法区分最小值的位置，例如

```
1 2 3 4 5
2 3 4 5 1
```

对于上述两种情况，都有`nums[mid] > nums[i]`，但是上面一种情况的最小值在m之前，而下面一种情况最小值在m之后

如果`nums[m]`和`有端点nums[j]`比较

- 对`1 2 3 4 5`而言，有`nums[m] < nums[j]`，那么`[m...j]`之间必定是递增有序的，最小值必定在`[i...m]`之间，所以可以修改区间右端点`j = m`
- 对`2 3 4 5 1`而言，有`nums[m] > nums[j]`，原数组是一个递增有序的序列，但是现在中间的值>右端点的值，即**出现了断层**，旋转点必定在`[m+1...j]`之间，所以可以修改区间左端点`i = m + 1`



```c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int n = numbers.size(), left = 0, right = n-1;
        if (numbers.size() == 1)
            return numbers[0];
        while (right > left){
            int mid = left + (right - left) / 2;
            if (numbers[mid] > numbers[right])
                left = mid + 1;
            else if (numbers[mid] < numbers[right])
                right = mid;
            else
                right--;
        }
        return numbers[left];
    }

};
```



### （3）[第一个只出现一次的字符](https://leetcode.cn/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
>
> **示例 1:**
>
> ```
> 输入：s = "abaccdeff"
> 输出：'b'
> ```
>
> **示例 2:**
>
> ```
> 输入：s = "" 
> 输出：' '
> ```
>
> **限制：**
>
> ```
> 0 <= s 的长度 <= 50000
> ```



1）哈希表

```c++
class Solution {
public:
    char firstUniqChar(string s) {
        int hastTable[26] = {0};
        for (char ch : s){
            hastTable[ch - 'a']++;
        }
        for (int i = 0; i < s.size(); i++){
            if (hastTable[s[i] - 'a'] == 1)
                return s[i];
        }
        return ' ';
    }
};
```



2）



## 6 搜索与回溯算法（简单）

### （1） [I. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回：
>
> ```
> [3,9,20,15,7]
> ```
>
> **提示：**
>
> 1. `节点总数 <= 1000`

利用广度优先遍历来得到每一层的结点值（需要注意根节点是否为空）

```c++
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        if (!root)  return {};
        queue<TreeNode*> q;
        TreeNode* p =root;
        vector<int> res;
        q.push(p);
        while (q.size()){
            p = q.front();
            q.pop();
            res.push_back(p->val);
            if (p->left)    q.push(p->left);
            if (p->right)   q.push(p->right);
        }
        return res;
    }
};
```



### （2）[II. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [9,20],
>   [15,7]
> ]
> ```
>
> **提示：**
>
> 1. `节点总数 <= 1000`

BFS+辅助变量r来判断是否到达了当前层的最后一个结点

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root)  return {};

        vector<int> layer;
        vector<vector<int>> res;

        queue<TreeNode*> q;
        TreeNode* p = root, *r = root;
        q.push(p);
        
        while (q.size()){
            p = q.front();
            q.pop();
            layer.push_back(p->val);
            if (p->left)    q.push(p->left);
            if (p->right)   q.push(p->right);

            if (p == r){		//当前节点p是当前层的最后一个结点
                res.push_back(layer);
                layer.clear();	//存储当前层的layer清空
                r = q.back();   //更新下一层的最后一个结点
            }
        }

        return res;
    }
};
```



（3）[III. 从上到下打印二叉树](https://leetcode.cn/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
>
> 例如:
> 给定二叉树: `[3,9,20,null,null,15,7]`,
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [20,9],
>   [15,7]
> ]
> ```
>
> **提示：**
>
> 1. `节点总数 <= 1000`

和上一题相比，需要在偶数层将每一层的元素反转（假设根节点位于第一层），这里在增加一个变量high用来表示当前节点的高度

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root)  return {};

        int high = 1;   //增加的表示当前层高度的变量high
        vector<int> layer;
        vector<vector<int>> res;

        queue<TreeNode*> q;
        TreeNode* p = root, *r = root;
        q.push(p);
        
        while (q.size()){
            p = q.front();
            q.pop();
            layer.push_back(p->val);
            if (p->left)    q.push(p->left);
            if (p->right)   q.push(p->right);

            if (p == r){
                if (high%2 == 0)//偶数层，需要进行翻转
                    reverse(layer.begin(), layer.end());
                res.push_back(layer);
                layer.clear();
                r = q.back();
                high++;
            }
        }

        return res;
    }
};
```



## 7 搜索与回溯算法（简单）

### （1）[树的子结构❤](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/description/)

> 输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)
>
> B是A的子结构， 即 A中有出现和B相同的结构和节点值。
>
> 例如:
> 给定的树 A:
>
> `   3   4  5  1  2`
> 给定的树 B：
>
> `  4  1`
> 返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。
>
> **示例 1：**
>
> ```
> 输入：A = [1,2,3], B = [3,1]
> 输出：false
> ```
>
> **示例 2：**
>
> ```
> 输入：A = [3,4,5,1,2], B = [4,1]
> 输出：true
> ```

递归

```c++
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (!A || !B)
            return false;
        return dfs(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B);
        //B是以A为根的子结构     B是以A的左子树为根的子结构     B是以A的右子树为根的子结构
    }

    bool dfs(TreeNode* A, TreeNode* B){
        if (!B) return true;
        if (!A) return false;
        return A->val == B->val && dfs(A->left, B->left) && dfs(A->right, B->right);
    }
};
```



### （2）[二叉树的镜像](https://leetcode.cn/problems/er-cha-shu-de-jing-xiang-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 请完成一个函数，输入一个二叉树，该函数输出它的镜像。
>
> 例如输入：
>
> `   4     2   7   1  3 6  9`
> 镜像输出：
>
> ```
> 4    7   2    9  6 3  1
> ```
>
> **示例 1：**
>
> ```
> 输入：root = [4,2,7,1,3,6,9]
> 输出：[4,7,2,9,6,3,1]
> ```
>
> **限制：**
>
> ```
> 0 <= 节点个数 <= 1000
> ```

**交换二叉树的左右孩子**

```c++
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (!root)  return root;
        // TreeNode* p = root->left, *q = root->right;
        root->left = mirrorTree(root->left);
        root->right = mirrorTree(root->right);
        swap(root->left, root->right);
        return root;
    }
};
```



### （3）[对称的二叉树](https://leetcode.cn/problems/dui-cheng-de-er-cha-shu-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
>
> 例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
> `  1   2  2  3  4 4  3`
> 但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
>
> ```
>   1   2  2   3   3
> ```
>
> **示例 1：**
>
> ```
> 输入：root = [1,2,2,3,4,4,3]
> 输出：true
> ```
>
> **示例 2：**
>
> ```
> 输入：root = [1,2,2,null,3,null,3]
> 输出：false
> ```
>
> **限制：**
>
> ```
> 0 <= 节点个数 <= 1000
> ```

**递归**

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root)  return true;
        return dfs(root->left, root->right);
    }

    bool dfs(TreeNode* p, TreeNode* q){
        if (!p && !q)
            return true;
        else if (p && q){
            return p->val == q->val && dfs(p->left, q->right) && dfs(p->right, q->left);
        }else
            return false;
    }
};
```



## 8 动态规划（简单）

### （1）[斐波那契数列](https://leetcode.cn/problems/fei-bo-na-qi-shu-lie-lcof/description/)

> 写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项（即 `F(N)`）。斐波那契数列的定义如下：
>
> ```
> F(0) = 0,   F(1) = 1
> F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
> ```
>
> 斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> **示例 1：**
>
> ```
> 输入：n = 2
> 输出：1
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 5
> 输出：5
> ```
>
> **提示：**
>
> - `0 <= n <= 100`

1）动态规划

```c++
class Solution {
    static const int N = 110, mod = 1e9 + 7;
    int dp[N];
public:
    int fib(int n) {
        if (n < 2)
            return dp[n] = n;
        if (dp[n] != 0)
            return dp[n];
        return dp[n] = (fib(n-1) + fib(n-2)) % mod;
    }

};
```



2）动态规划 + 滚动数组

```c++
class Solution {
    static const int mod = 1e9 + 7;
public:
    int fib(int n) {
        if (n < 2)  return n;
        int a = 0, b = 1, res;
        for (int i = 2; i <= n; i++){
            res = (a + b) % (mod);
            a = b;
            b = res;
        }
        return res;
    }

};
```



### （2）[青蛙跳台阶问题](https://leetcode.cn/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 `n` 级的台阶总共有多少种跳法。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> **示例 1：**
>
> ```
> 输入：n = 2
> 输出：2
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 7
> 输出：21
> ```
>
> **示例 3：**
>
> ```
> 输入：n = 0
> 输出：1
> ```
>
> **提示：**
>
> - `0 <= n <= 100`



```c++
class Solution {
    static const int mod = 1e9 + 7;
public:
    int numWays(int n) {
        if (n < 2)  return 1;
        int a = 1, b = 1, res;
        for (int i = 2; i <= n; i++){
            res = (a + b) % mod;
            a = b;
            b = res;
        }
        return res;
    }
};
```



（3）股票的最大利润



```c++
class Solution {
public:
    //dp[i]保存前面股票的最低价
    //dp[i] = min(dp[i-1], prices[i])
    int maxProfit(vector<int>& prices) {
        if (!prices.size())
            return 0;
        int n = prices.size(), res = 0;
        int a = prices[0], b = 0;
        for (int i = 1; i < n; i++){
            b = min(a, prices[i-1]);
            res = max(res, prices[i] - b);
            a = b;
        }
        return res;
    }
};
```





##  9 动态规划（中等）

### （1）[连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
>
> 要求时间复杂度为O(n)。
>
>  
>
> **示例1:**
>
> ```
> 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> ```
>
>  
>
> **提示：**
>
> - `1 <= arr.length <= 10^5`
> - `-100 <= arr[i] <= 100`

动态规划

`dp[i]`以`nums[i]`结尾的子数组可以获得的最大子数组和，如果对于前面的和`dp[i-1] > 0`，那么最大的子数组和`dp[i] = dp[i-1] + nums[i]`，否则`dp[i] = nums[i]`，边界条件`dp[0] = nums[0]`

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size(), res = nums[0];
        vector<int> dp(n);
        dp[0] = nums[0];
        for (int i = 1; i < n; i++){
            dp[i] = max(dp[i-1] + nums[i], nums[i]);
            res = max(res, dp[i]);
        }
        return res;
    }
};
```





### （2）[礼物的最大价值](https://leetcode.cn/problems/li-wu-de-zui-da-jie-zhi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
>
> **示例 1:**
>
> ```
> 输入: 
> [
>   [1,3,1],
>   [1,5,1],
>   [4,2,1]
> ]
> 输出: 12
> 解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
> ```
>
>  
>
> 提示：
>
> - `0 < grid.length <= 200`
> - `0 < grid[0].length <= 200`

定义`dp[i][j]`从棋盘左上角到达位置`(i, j)`可以获得的礼物的最大价值

对于到达`(i, j)`前一步，因为每次只能向右或者向下移动一个，那么他上一次所在的位置就是`(i-1,j)`或者`(i, j-1)`

`dp[i][j] = max(dp[i-1][j], dp[i][j-1] + grid[i][j])`

```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int> (n, 0));
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (i-1 >= 0)
                    dp[i][j] = max(dp[i][j], dp[i-1][j]);
                if (j-1 >= 0)
                    dp[i][j] = max(dp[i][j], dp[i][j-1]);
                dp[i][j] += grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```

对于`dp[i][j]`而言，每次只用到了当前行的`dp[i][j-1]`和上一行的`dp[i-1][j]`，那么可以优化空间复杂度，降低为O(n)

利用&运算

```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(2, vector<int> (n, 0));
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (i-1 >= 0)
                    dp[i & 1][j] = max(dp[i & 1][j], dp[(i-1 ) & 1][j]);
                if (j-1 >= 0)
                    dp[i & 1][j] = max(dp[i & 1][j], dp[i & 1][j-1]);
                dp[i&1][j] += grid[i][j];
            }
        }
        return dp[(m-1) & 1][n-1];
    }
};
```





## 10 动态规划（中等）

### （1）[把数字翻译成字符串](https://leetcode.cn/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。
>
>  
>
> **示例 1:**
>
> ```
> 输入: 12258
> 输出: 5
> 解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
> ```
>
>  
>
> **提示：**
>
> - $0 <= num < 2^{31}$

为了方便遍历数字中的每一位首先将数组转换为字符创，将`dp[i]`为定义为`str[0...i]`中翻译方法的种数，对于当前位`str[i]`要么能够和`str[i-1]`构成10-25之间的数字，要么自己单独构成一位

状态转移方程：`dp[i] = dp[i-1]`	`str[i-1]和str[i]`两个数不能构成一个有效的字母

​							`dp[i] = dp[i-1] + dp[i-2]`	`str[i-1]和str[i]`两个数能构成一个有效的字母

```c++
class Solution {
public:
    int translateNum(int num) {
        if (num < 10)   return 1;
        string str = to_string(num);
        int n = str.size();
        vector<int> dp(n);
        dp[0] = 1, dp[1] = (str[0] - '0') * 10 + (str[1] - '0') <= 25 ? 2 : 1;
        for (int i = 2; i < n; i++){
            int digit = (str[i-1] - '0') * 10 + (str[i] - '0');
            if (digit >= 10 && digit <= 25)
                dp[i] = dp[i-1] + dp[i-2];
            else
                dp[i] = dp[i-1];
        }
        return dp[n-1];
    }
};
```

在`dp`之前，需要注意是否num中有两位数字





### （2）[最长不含重复字符的子字符串](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/description/) ❤

> 请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。
>
>  **示例 1:**
>
> ```
>输入: "abcabcbb"
> 输出: 3 
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
> ```
> 
> **示例 2:**
>
> ```
>输入: "bbbbb"
> 输出: 1
> 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
> ```
> 
> **示例 3:**
>
> ```
>输入: "pwwkew"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
>   请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
> ```
>    
> 
>
>  提示：
>
> - `s.length <= 40000`

1）双指针

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int hashTable[128] = {0}, res = 0;
        for (int i = 0, j = i; j < s.size(); j++){
            hashTable[s[j]]++;
            while (hashTable[s[j]] > 1){
                hashTable[s[i++]]--;
            }
            res = max(res, j-i+1);
        }
        return res;
    }
};
```



2）动态规划

[题解](https://leetcode.cn/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/solutions/210129/mian-shi-ti-48-zui-chang-bu-han-zhong-fu-zi-fu-d-9/)

- **状态定义：** 设动态规划列表`dp` ，`dp[j]`代表以字符 `s[j]`为结尾的 “最长不重复子字符串” 的长度。

- 转移方程：固定右边界`j`，设字符`s[j]`左边距离最近的相同字符为`s[i]`，即`s[i]=s[j]`

  1. 当 `i<0`，即 `s[j]` 左边无相同字符，则 `dp[j]=dp[j−1]+1`；
  2. 当 `dp[j−1]<j−i`，说明字符 `s[i]` 在子字符串 `dp[j−1]`**区间之外** ，则 `dp[j]=dp[j−1]+1`；
  3. 当 `dp[j−1]≥j−i`，说明字符 `s[i]` 在子字符串 `dp[j−1]`**区间之中** ，则 `dp[j]`的左边界由 `s[i]s`决定，即 `dp[j]=j−i`;

  > 当 `i<0`时，由于 `dp[j−1]≤j`恒成立，因而 `dp[j−1]<j−i`恒成立，因此分支 `1.` 和 `2.` 可被合并。

- **返回值：** `max⁡(dp)`，即全局的 “最长不重复子字符串” 的长度。

![Picture1.png](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\c576757494724070d0c40cd192352ef9f48c42e14af09a1333972b9d843624a3-Picture1.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        //dp[i][j]表示s[i...j]中无重复字符的最长子串的长度
        if (s.size() == 0)  return 0;
        int n = s.size(), hashTable[128] = {0};
        vector<int> dp(n, 0);
        memset(hashTable, -1, sizeof(hashTable));//hashTable数组初始化
        hashTable[s[0]] = 0;
        dp[0] = 1;
        for (int j = 1; j < n; j++){
            int i = hashTable[s[j]];
            if (i >= 0 && dp[j-1] >= j-i)
                dp[j] = j-i;
            else
                dp[j] = dp[j-1] + 1;
            hashTable[s[j]] = j;
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```







## 11 双指针（简单）

### （1）[删除链表的节点](https://leetcode.cn/problems/shan-chu-lian-biao-de-jie-dian-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
>
> 返回删除后的链表的头节点。
>
> **注意：**此题对比原题有改动
>
> **示例 1:**
>
> ```
> 输入: head = [4,5,1,9], val = 5
> 输出: [4,1,9]
> 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
> ```
>
> **示例 2:**
>
> ```
> 输入: head = [4,5,1,9], val = 1
> 输出: [4,5,9]
> 解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
> ```
>
> - 题目保证链表中节点的值互不相同

对于链表中的题目，一般可以通过增加虚拟头结点dummy让dummy来指向题目中所给的head，这样可以避免对于边界条件的判断

```c++
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        ListNode* dummy = new ListNode(-1), *r;
        dummy->next = head, r = dummy;
        //需要删除节点，利用r->next来寻找
        while (r->next && r->next->val != val)  r = r->next;
        //r->next可能是链表中最后一个节点
        if (r->next->next == NULL)
            r->next = NULL;
        else
            r->next = r->next->next;
        return dummy->next;
    }
};
```





### （2）[链表中倒数第k个节点](https://leetcode.cn/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。
>
> 例如，一个链表有 `6` 个节点，从头节点开始，它们的值依次是 `1、2、3、4、5、6`。这个链表的倒数第 `3` 个节点是值为 `4` 的节点。
>
>  
>
> **示例：**
>
> ```
> 给定一个链表: 1->2->3->4->5, 和 k = 2.
> 
> 返回链表 4->5.
> ```

双指针，可以在草稿纸上画一画

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode *p = head, *q = head;
        while (k--) q = q->next;
        while (q){
            p = p->next;
            q = q->next;
        }
        return p;
    }
};
```





## 12 双指针（简单）



### （1）[合并两个排序的链表](https://leetcode.cn/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/description/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
> **示例1：**
>
> ```
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```
>
> **限制：**
>
> ```
> 0 <= 链表长度 <= 1000
> ```



本质上和合并两个有序数组一致，利用双指针

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(), *r;
        r = dummy;
        while (l1 || l2){
            if (l1 && l2){
                if (l1->val <= l2->val){
                    r->next = l1;
                    l1 = l1->next;
                    r = r->next;
                }else{
                    r->next = l2;
                    l2 = l2->next;
                    r = r->next;
                }
            }else if (l1){
                r->next = l1;
                l1 = l1->next;
                r = r->next;
            }else{
                r->next = l2;
                l2 = l2->next;
                r = r->next;
            }
        }
        r->next = NULL;
        return dummy->next;
    }
};
```





### （2）[两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入两个链表，找出它们的第一个公共节点。
>
> 如下面的两个链表**：**
>
> [<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\160_statement.png" alt="img" style="zoom:67%;" />](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
>
> 在节点 c1 开始相交。
>
>  
>
> **示例 1：**
>
> [<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\160_example_1.png" alt="img" style="zoom:67%;" />](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)
>
> ```
> 输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
> 输出：Reference of the node with value = 8
> 输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
> ```
>
>  
>
> **示例 2：**
>
> [<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\160_example_2.png" alt="img" style="zoom:67%;" />](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)
>
> ```
> 输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
> 输出：Reference of the node with value = 2
> 输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
> ```
>
>  
>
> **示例 3：**
>
> [<img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\160_example_3.png" alt="img" style="zoom:67%;" />](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
>
> ```
> 输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
> 输出：null
> 输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
> 解释：这两个链表不相交，因此返回 null。
> ```
>
>  
>
> **注意：**
>
> - 如果两个链表没有交点，返回 `null`.
> - 在返回结果后，两个链表仍须保持原有的结构。
> - 可假定整个链表结构中没有循环。
> - 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

tips：先让两个链表中较长的链表先走`abs(lenA - lenB)`个结点（如果两个链表有公共的结点，那么从这个节点一直到链表结束，他们之后的节点都一致，即从当前结点开始两个链表的长度一致）

为了方便起见，可以假设`headA`是那个较长的链表，如果不是用swap来交换两个链表的头结点

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = getListLength(headA), lenB = getListLength(headB);
        if (lenA < lenB){
            swap(lenA, lenB);
            swap(headA, headB);
        }
        int diff = lenA - lenB;
        while (diff--)  headA = headA->next;
        while (headA){
            if (headA == headB) return headA;
            headA = headA->next;
            headB = headB->next;
        }
        return NULL;
    }

    int getListLength(ListNode* head){
        int len = 0;
        while (head){
            head = head->next;
            len++;
        }
        return len;
    }
};
```





## 13 双指针（简单）

### （1）[调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。
>
>  
>
> **示例：**
>
> ```
> 输入：nums = [1,2,3,4]
> 输出：[1,3,2,4] 
> 注：[3,1,2,4] 也是正确的答案之一。
> ```
>
>  
>
> **提示：**
>
> 1. `0 <= nums.length <= 50000`
> 2. `0 <= nums[i] <= 10000`

本质上和快速排序，利用双指针将`<pivot`的元素移动到数组的前面，`>pivot`的元素移动到数组的末尾一致

```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int left = 0, right = nums.size()-1;
        while (left < right){
            while (left < right &&nums[left]%2 == 1)    left++;
            while (right > left && nums[right]%2 == 0)  right--;
            swap(nums[left++], nums[right--]);
        }
        return nums;
    }
};
```





### （2） 和为s的两个数字

> 输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
>
>  
>
> **示例 1：**
>
> ```
> 输入：nums = [2,7,11,15], target = 9
> 输出：[2,7] 或者 [7,2]
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [10,26,30,31,47,60], target = 40
> 输出：[10,30] 或者 [30,10]
> ```
>
>  
>
> **限制：**
>
> - `1 <= nums.length <= 10^5`
> - `1 <= nums[i] <= 10^6`



（1）双指针

时间复杂度O(n)，空间复杂度O(1)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int low = 0, high = nums.size()-1;
        while (low < high){
            if (nums[low] + nums[high] == target)
                return {nums[low], nums[high]};
            else if (nums[low] + nums[high] > target)
                high--;
            else    
                low++;
        }
        return {};
    }
};
```



（2）哈希表

时间复杂度O(n)，空间复杂度O(n)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mp;
        for (int i : nums){
            if (mp.find(target - i) != mp.end())
                return {i, target-i};
            mp[i]++;
        }
        return {};
    }
};
```



（3）二分

时间复杂度O(nlogn)，空间复杂度O(1)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i++){
            int j = lower_bound(nums.begin()+i+1, nums.end(), target - nums[i]) - nums.begin();
            if (j < nums.size() && nums[i] + nums[j] == target){
                return {nums[i], nums[j]};
            }
        }
        return {};
    }
};
```





### （3）[翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。
>
>  
>
> **示例 1：**
>
> ```
> 输入: "the sky is blue"
> 输出: "blue is sky the"
> ```
>
> **示例 2：**
>
> ```
> 输入: "  hello world!  "
> 输出: "world! hello"
> 解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
> ```
>
> **示例 3：**
>
> ```
> 输入: "a good   example"
> 输出: "example good a"
> 解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
> ```

双指针

```c++
class Solution {
public:
    string reverseWords(string s) {
        string res = "";
        int right = s.size()-1, left;
        while (right >= 0){
            while (right >= 0 && s[right] == ' ')   right--;
            //退出循环时right < 0 || s[right] != ' '
            left = right;
            while (left >= 0 && s[left] != ' ')     left--;
            //退出循环时left < 0 || s[left] == ' '
            // cout << right << " " << left << endl;
            if (right - left)
                res += s.substr(left+1, right - left) + " ";
            right = left;
        }
        res.pop_back();
        return res;
    }
};
```





## 14 搜索与回溯算法（中等）

### （1）[矩阵中的路径](https://leetcode.cn/problems/ju-zhen-zhong-de-lu-jing-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。
>
> 单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
>
>  
>
> 例如，在下面的 3×4 的矩阵中包含单词 "ABCCED"（单词中的字母已标出）。
>
> <img src="https://assets.leetcode.com/uploads/2020/11/04/word2.jpg" alt="img" style="zoom:67%;" />
>
>  
>
> **示例 1：**
>
> ```
> 输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
> 输出：true
> ```
>
> **示例 2：**
>
> ```
> 输入：board = [["a","b"],["c","d"]], word = "abcd"
> 输出：false
> ```
>
>  
>
> **提示：**
>
> - `m == board.length`
> - `n = board[i].length`
> - `1 <= m, n <= 6`
> - `1 <= word.length <= 15`
> - `board `和` word `仅由大小写英文字母组成

写DFS时最好写清楚当前递归参数的含义，条件

```c++
class Solution {
private:
    int m, n;
    int dx[4] = {0, 0, -1, 1};
    int dy[4] = {1, -1, 0, 0};
    bool visited[6][6];
    vector<vector<char>> board;
    string word;
public:
    bool exist(vector<vector<char>>& board, string word) {
        this->board = board, this->word = word;
        this->m = board.size(), this->n = board[0].size();
        for (int i = 0; i < m; i++){
            for (int j = 0; j < n; j++){
                if (board[i][j] == word[0]){
                    if (DFS(i, j, 0))   
                        return true;
                }
            }
        }
        return false;
    }

    bool DFS(int x, int y, int index){
        //board[x][y]已经匹配word[index]，继续向后匹配
        if (index == word.size()-1) 
            return true;
        if (x < 0 || x >= m || y < 0 || y >= n)
            return false;
        visited[x][y] = true;
        for (int i = 0; i < 4; i++){
            int nx = x + dx[i], ny = y + dy[i];
            if (nx >= 0 && nx < m && ny >= 0 && ny < n){
                if (!visited[nx][ny] && board[nx][ny] == word[index+1]){
                    if (DFS(nx, ny, index+1))   
                        return true;
                }
            }
        }
        visited[x][y] = false;
        return false;
    }
};
```





### （2）[机器人的运动范围](https://leetcode.cn/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 地上有一个m行n列的方格，从坐标 `[0,0]` 到坐标 `[m-1,n-1]` 。一个机器人从坐标 `[0, 0] `的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？
>
>  
>
> **示例 1：**
>
> ```
> 输入：m = 2, n = 3, k = 1
> 输出：3
> ```
>
> **示例 2：**
>
> ```
> 输入：m = 3, n = 1, k = 0
> 输出：1
> ```
>
> **提示：**
>
> - `1 <= n,m <= 100`
> - `0 <= k <= 20`

定义`DFS(x, y)`从`(x, y)`出发能够到达的格子数

```c++
class Solution {
private:
    int m, n, k, dx[4] = {0, 0, -1, 1}, dy[4] = {1, -1, 0, 0};
    bool visited[110][110];
public:
    int movingCount(int m, int n, int k) {
        this->m = m, this->n = n, this->k = k;
        return DFS(0, 0);
    }

    int DFS(int x, int y){
        if (x < 0 || x >= m || y < 0 || y >= n)
            return 0;
        if (visited[x][y] || (x/10 + x%10 + y/10 + y%10) > k)  
            return 0;
        int sum = 1;
        visited[x][y] = true;
        for (int i = 0; i < 4; i++){
            int nx = x + dx[i], ny = y + dy[i];
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) continue;
            sum += DFS(nx, ny);
        }
        return sum;
    }
};
```





## 15 搜索与回溯算法（中等）

###  （1）[二叉树中和为某一值的路径](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) 

> 给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。
>
> **叶子节点** 是指没有子节点的节点。
>
>  
>
> **示例 1：**
>
> <img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\pathsumii1.jpg" alt="img" style="zoom: 50%;" />
>
> ```
> 输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
> 输出：[[5,4,11,2],[5,8,4,5]]
> ```
>
> **示例 2：**
>
> <img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\pathsum2.jpg" alt="img" style="zoom:50%;" />
>
> ```
> 输入：root = [1,2,3], targetSum = 5
> 输出：[]
> ```
>
> **示例 3：**
>
> ```
> 输入：root = [1,2], targetSum = 0
> 输出：[]
> ```
>
>  
>
> **提示：**
>
> - 树中节点总数在范围 `[0, 5000]` 内
> - `-1000 <= Node.val <= 1000`
> - `-1000 <= targetSum <= 1000`

对于递归最好在旁边注释参数含义和函数的功能

```c++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
public:
    vector<vector<int>> pathSum(TreeNode* root, int target) {
        DFS(root, 0, target);
        return res;
    }

    void DFS(TreeNode* root, int sum, const int &target){
        if (!root)  return ;

        //preOrder
        path.push_back(root->val);
        if (root->val + sum == target && !root->left && !root->right)
            res.push_back(path);

        DFS(root->left, sum + root->val, target);
        DFS(root->right, sum + root->val, target);
        path.pop_back();
    }
};
```



### （2）[二叉搜索树与双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。
>
>  为了让您更好地理解问题，以下面的二叉搜索树为例：
>
> <img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\bstdlloriginalbst.png" alt="img" style="zoom:50%;" />
>
>  
>
> 我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。
>
>  下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。
>
> <img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\bstdllreturndll.png" alt="img" style="zoom:50%;" />
>
> 特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

需要注意这里在private处定义了几个变量，并没有将pre作为参数传到dfs中

```c++
class Solution {
private:
    Node *pre = NULL, *head, *tail;
public:
    Node* treeToDoublyList(Node* root) {
        if (!root)  return root;
        head = root, tail = root;//让head tail 分别指向二叉树的第一个和最后一个结点
        while (head->left)
            head = head->left;
        while (tail->right)
            tail = tail->right;
        
        dfs(root);

        head->left = tail, tail->right = head;

        return head;
    }

    void dfs(Node* root){   //排序的双向循环链表，中序遍历
        if (!root)
            return ;

        dfs(root->left);

        //对于当前节点的处理:上一个节点的后继 和当前节点的前驱

        if (pre)  //对于第一个节点而言，前驱为空
            pre->right = root;
        root->left = pre;
        pre = root; //更新pre

        dfs(root->right);
    }
};
```



### （3）[二叉搜索树的第k大节点](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 给定一棵二叉搜索树，请找出其中第 `k` 大的节点的值。
>
> **示例 1:**
>
> ```
> 输入: root = [3,1,4,null,2], k = 1
> 3
> / \
> 1   4
> \
> 2
> 输出: 4
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [5,3,6,2,4,null,null,1], k = 3
>     5
>    / \
>   3   6
>  / \
> 2   4
> /
> 1
> 输出: 4
> ```
>
> **限制：**
>
> - 1 ≤ k ≤ 二叉搜索树元素个数

直接递归的求二叉搜索树的右子树结点个数，根据右子树中节点的个数来判断第k大结点的位置

```c++
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        int rightCount = getNodeCount(root->right);
        if (rightCount == k-1)
            return root->val;   //右子树恰好k-1个
        else if (rightCount > k-1)
            return kthLargest(root->right, k);//在右子树中寻找
        else
            return kthLargest(root->left, k-rightCount-1);//在左子树中寻找
    }

    int getNodeCount(TreeNode* root){
        if (!root)
            return 0;
        return 1 + getNodeCount(root->left) + getNodeCount(root->right);
    }
};
```





## 16 排序

### （1）[把数组排成最小的数](https://leetcode.cn/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。
>
> **示例 1:**
>
> ```
> 输入: [10,2]
> 输出: "102"
> ```
>
> **示例 2:**
>
> ```
> 输入: [3,30,34,5,9]
> 输出: "3033459"
> ```
>
> **提示:**
>
> - `0 < nums.length <= 100`

贪心 + 排序，排序的方式有点特殊，对于两个数（假设为字符串+，不是数值加法）而言，a + b < b + a

```c++
class Solution {
public:
    string minNumber(vector<int>& nums) {
        string str = "";
        sort(nums.begin(), nums.end(), cmp);
        for (int i : nums)  str += to_string(i);
        return str;
    }

    static bool cmp(const int &a,const int &b){
        string s1 = to_string(a), s2 = to_string(b);
        return s1 + s2 < s2 + s1;
    }
};
```



### （2）[扑克牌中的顺子](https://leetcode.cn/problems/bu-ke-pai-zhong-de-shun-zi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 从**若干副扑克牌**中随机抽 `5` 张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
>
> **示例 1:**
>
> ```
> 输入: [1,2,3,4,5]
> 输出: True
> ```
>
> **示例 2:**
>
> ```
> 输入: [0,0,1,2,5]
> 输出: True
> ```
>
> **限制：**
>
> 数组长度为 5 
>
> 数组的数取值为 [0, 13] .

排序记录king的个数，遍历完数组king指向数组中最小且不为king的下标。如果数组中有两个相同的数字那么肯定不是顺子，最后返回`nums[4]` - `nums[king]`是否小于5 （这是简单题？）

```c++
class Solution {
public:
    bool isStraight(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int king = 0;
        for (int i = 0; i < 5; i++){
            if (nums[i] == 0)   king++;
            else if (i && nums[i] == nums[i-1])
                return false;
        }
        return nums[4] - nums[king] < 5;
    }
};
```



## 17 排序

### （1）[最小的k个数](https://leetcode.cn/problems/zui-xiao-de-kge-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入整数数组 `arr` ，找出其中最小的 `k` 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
>
> **示例 1：**
>
> ```
> 输入：arr = [3,2,1], k = 2
> 输出：[1,2] 或者 [2,1]
> ```
>
> **示例 2：**
>
> ```
> 输入：arr = [0,1,2,1], k = 1
> 输出：[0]
> ```
>
> **限制：**
>
> - `0 <= k <= arr.length <= 10000`
> - `0 <= arr[i] <= 10000`

排序或者利用优先队列，手写堆均可（好像在空间上效率不是很高）

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int> res;
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int i = 0; i < arr.size(); i++)    pq.push(arr[i]);
        while (k--){
            res.push_back(pq.top());
            pq.pop();
        }
        return res;
    }
};
```



### （2）[数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。
>
> 例如，
>
> [2,3,4] 的中位数是 3
>
> [2,3] 的中位数是 (2 + 3) / 2 = 2.5
>
> 设计一个支持以下两种操作的数据结构：
>
> - void addNum(int num) - 从数据流中添加一个整数到数据结构中。
> - double findMedian() - 返回目前所有元素的中位数。
>
> **示例 1：**
>
> ```
> 输入：
> ["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
> [[],[1],[2],[],[3],[]]
> 输出：[null,null,null,1.50000,null,2.00000]
> ```
>
> **示例 2：**
>
> ```
> 输入：
> ["MedianFinder","addNum","findMedian","addNum","findMedian"]
> [[],[2],[],[3],[]]
> 输出：[null,null,2.00000,null,2.50000]
> ```
>
> **限制：**
>
> - 最多会对 `addNum、findMedian` 进行 `50000` 次调用。

使用一个大根堆和一个小根堆来存储元素，利用大根堆来存储元素前半部分的值，小根堆存储后半部分元素的值

并且 | 大根堆内的元素个数 - 小根堆内元素的个数 | ≤ 1

始终维护一个性质：前半部分的元素值 < 后半部分的元素值（即大根堆内所有的元素值 < 小根堆内所有的元素值）

那么在元素进行插入时，需要判断是往小根堆还是大根堆插入，如何维护这个性质（可以通过多个if语句，但是写起来比较啰嗦）

我们对大根堆和小根堆内元素的个数进行比较：

- 两个堆中的元素个数相等

  那么我们最终要将当前元素num插入到前半部分的大根堆中，但是直接进行插入，可能会使num大于小根堆中的某个元素

  > 那么我们先将num插入到小根堆中，再将小根堆中的栈顶元素弹出，并插入到大根堆中呢？
  >
  > 这里根据我们前面假设的一个前提：大根堆内所有的元素值 < 小根堆内所有的元素值
  >
  > 那么将num插入小根堆之后，由于堆的性质，所以最小的元素在堆顶。我们将堆顶元素弹出并插入大根堆，就可以使得 大根堆内所有的元素值 < 小根堆内所有的元素值，这样是可行的

- 前半部分的大根堆比小根堆中多一个元素

  为了维护 | 大根堆内的元素个数 - 小根堆内元素的个数 | ≤ 1这个性质，我们应该向小根堆中插入一个元素

  > 有了前面的提示，想必已经知道了直接插入到小根堆中也是不可行的。
  >
  > 我们将num插入到大根堆中，并将大根堆中的栈顶元素弹出 （大根堆先push后pop所以总的元素个数不会变，小根堆中的元素个数+1）

```c++
class MedianFinder {
private:
    priority_queue<int, vector<int>, greater<int>> minHeap;
    priority_queue<int, vector<int>, less<int>> maxHeap;
public:
    /** initialize your data structure here. */
    MedianFinder() {
        //数据流的中位数，维护一个大根堆用来存储前半部分的元素，维护一个小根堆用来存储后半部分的元素
        
    }
    
    void addNum(int num) {
        if (minHeap.size() == maxHeap.size()){
            //前后部分元素数量一致，先将元素放到后半部分，再将小根堆的堆顶元素push入大根堆
            minHeap.push(num);
            maxHeap.push(minHeap.top()), minHeap.pop();
        }else{  //前半部分比后半部分多一个元素
            maxHeap.push(num);
            minHeap.push(maxHeap.top()), maxHeap.pop();
        }
    }
    
    double findMedian() {
        if (minHeap.size() == maxHeap.size()){
           return (maxHeap.top() + minHeap.top()) / 2.0;
        }else
            return maxHeap.top() * 1.0;
    }
};
```





### 18 搜索与回溯算法（中等）

### （1）[二叉树的深度](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。
>
> 例如：
>
> 给定二叉树 `[3,9,20,null,null,15,7]`，
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回它的最大深度 3 。
>
> **提示：**
>
> 1. `节点总数 <= 10000`

直接递归

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root)  return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```



### （2）[平衡二叉树](https://leetcode.cn/problems/ping-heng-er-cha-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
>
> **示例 1:**
>
> 给定二叉树 `[3,9,20,null,null,15,7]`
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回 `true` 。
>
> **示例 2:**
>
> 给定二叉树 `[1,2,2,3,3,null,null,4,4]`
>
> ```
>        1
>       / \
>      2   2
>     / \
>    3   3
>   / \
>  4   4
> ```
>
> 返回 `false` 。
>
> **限制：**
>
> - `0 <= 树的结点个数 <= 10000`

根据题意写递归即可

```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (!root)
            return true;
        return isBalanced(root->left) && isBalanced(root->right) && abs(getDepth(root->left) - getDepth(root->right)) < 2;
    }

    int getDepth(TreeNode* root){
        return !root ? 0 : 1 + max(getDepth(root->left), getDepth(root->right));
    }
};
```





## 19 搜索与回溯算法（中等）

### （1）[求1+2+…+n](https://leetcode.cn/problems/qiu-12n-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 求 `1+2+...+n` ，**要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句**（A?B:C）。
>
> **示例 1：**
>
> ```
> 输入: n = 3
> 输出: 6
> ```
>
> **示例 2：**
>
> ```
> 输入: n = 9
> 输出: 45
> ```
>
> **限制：**
>
> - `1 <= n <= 10000`

看上去有点奇奇怪怪的题目

[官方题解](https://leetcode.cn/problems/qiu-12n-lcof/solutions/271053/qiu-12n-by-leetcode-solution/)



### （2）[[I. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]
>
> ![img](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\binarysearchtree_improved.png)
>
> **示例 1:**
>
> ```
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
> 输出: 6 
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
> 输出: 2
> 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
> ```
>
> **说明:**
>
> - 所有节点的值都是唯一的。
> - p、q 为不同节点且均存在于给定的二叉搜索树中。

题目中明确说了是二叉搜索树，根据二叉搜索树的性质：左子树的结点值 < 根节点的值 < 右子树的结点值

利用结点值大小进行判断即可

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return root;			//加上这个语句速度会快很多
        else if (p->val > root->val && q->val > root->val)
            return lowestCommonAncestor(root->right, p, q);
        else if (p->val < root->val && q->val < root->val)
            return lowestCommonAncestor(root->left, p, q);
        else
            return root;
    }
};
```



### （3）[II. 二叉树的最近公共祖先 ](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> [百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”
>
> 例如，给定如下二叉树: root = [3,5,1,6,2,0,8,null,null,7,4]
>
> ![img](C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\binarytree.png)
>
> **示例 1:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
> 输出: 5
> 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
> ```
>
> **说明:**
>
> - 所有节点的值都是唯一的。
> - p、q 为不同节点且均存在于给定的二叉树中。

和上题不同，树中节点的大小关系是未知的。

递归：

- 递归出口，root == NULL || p == root || q == root
- 递归：记left左子树是否为p q的最近公共祖先，right右子树是否为p q的公共祖先
  - 如果left为空，那么返回right
  - 如果right为空，那么返回left
  - 否则返回root

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q)
            return root;

        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (!left)
            return right;
        else if (!right)
            return left;
            
        return root;
    }
};
```





## 20 分治算法（中等）

### （1）[重建二叉树](https://leetcode.cn/problems/zhong-jian-er-cha-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。
>
> 假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
>
> **示例 1:**
>
> <img src="C:\Users\hp-pc\Desktop\C++\Algorithm\剑指offer.assets\tree.jpg" alt="img" style="zoom:67%;" />
>
> ```
> Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
> Output: [3,9,20,null,null,15,7]
> ```
>
> **示例 2:**
>
> ```
> Input: preorder = [-1], inorder = [-1]
> Output: [-1]
> ```
>
> **限制：**
>
> ```
> 0 <= 节点个数 <= 5000
> ```

利用递归来解决二叉树的问题

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        return createTree(preorder, 0, n-1, inorder, 0, n-1);
    }

    TreeNode* createTree(vector<int>& preorder, int l1, int h1, vector<int>& inorder, int l2, int h2){
        if (l1 > h1)    return NULL;
        TreeNode* root = new TreeNode(preorder[l1]);
        int i;
        for (i = l2; preorder[l1] != inorder[i]; i++);  //在中序遍历中查找根节点所在的位置
        int llen = i-l2, rlen = h2-i;
        if (llen)
            root->left = createTree(preorder, l1+1, l1+llen, inorder, l2, l2+llen-1);
        if (rlen)
            root->right = createTree(preorder, h1-rlen+1, h1, inorder, h2-rlen+1, h2);
        return root;
    }
};
```

> 在`createTree`中preorder和`inorder`前面，使用&比const &更快一点？
>
> 原理：



### （2）[数值的整数次方](https://leetcode.cn/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 实现 [pow(*x*, *n*)](https://www.cplusplus.com/reference/valarray/pow/) ，即计算 x 的 n 次幂函数（即，xn）。不得使用库函数，同时不需要考虑大数问题。
>
> **示例 1：**
>
> ```
> 输入：x = 2.00000, n = 10
> 输出：1024.00000
> ```
>
> **示例 2：**
>
> ```
> 输入：x = 2.10000, n = 3
> 输出：9.26100
> ```
>
> **示例 3：**
>
> ```
> 输入：x = 2.00000, n = -2
> 输出：0.25000
> 解释：2-2 = 1/22 = 1/4 = 0.25
> ```
>
> **提示：**
>
> - `-100.0 < x < 100.0`
> - `-231 <= n <= 231-1`
> - `-104 <= xn <= 104`

 快速幂的递归写法，**注意代码风格**

```c++
class Solution {
public:
    double myPow(double x, int n) {
        return pow(x, n);
    }

     double pow(double x, long long n) {
        if (x == 0 || x == 1)    return x;
        else if (n < 0)
            return 1.0 / pow(x, -n);
        else if (n == 0)
            return 1.0;
        else{//n > 0
            if ((int)n&1)
                return x * pow(x,n-1);
            else{
                double temp = pow(x, n/2);
                return temp *temp;
            }
        }
    }   
};
```

这里如果n=-2147483648时，将n*=-1会超出整型的范围，为了避免这种方式采用了long long。也可以使用int型来进行递归，只不要加上一些判断。



### （3）[二叉搜索树的后序遍历序列](https://leetcode.cn/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。
>
> 参考以下这颗二叉搜索树：
>
> ```
>      5
>     / \
>    2   6
>   / \
>  1   3
> ```
>
> **示例 1：**
>
> ```
> 输入: [1,6,3,2,5]
> 输出: false
> ```
>
> **示例 2：**
>
> ```
> 输入: [1,3,2,6,5]
> 输出: true
> ```
>
> **提示：**
>
> 1. `数组长度 <= 1000`

首先判断根节点的左右子树是否满足条件，在递归的判断左右子树是否是BST

BST可以根据结点值的大小来确定左右子树的范围，假设右子树的索引为index，那么可以将二叉树划分为post[0...index-1]和post[index+1...high]，对于右子树中的所有节点 > 根节点的值，才可能称为一棵二叉树。

```c++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        return DFS(postorder, 0, postorder.size()-1);
    }

    bool DFS(const vector<int> &postorder, int low, int high){
        if (low >= high)    return true;
        int i;
        for (i = low; i < high && postorder[i] < postorder[high]; i++);
        //退出循环时有i==high || postorder[i] > postorder[high]

        for (int j = i; j < high; j++){     //右子树存在则遍历右子树
            if (postorder[j] < postorder[high])
                return false;
        }

        //            左子树是BST               右子树是BST
        return DFS(postorder, low, i-1) && DFS(postorder, i, high-1);
    }
};
```





## 21 位运算（简单）

### （1）[二进制中1的个数](https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm)

> 编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为 [汉明重量](http://en.wikipedia.org/wiki/Hamming_weight)).）。
>
> **提示：**
>
> - 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
> - 在 Java 中，编译器使用 [二进制补码](https://baike.baidu.com/item/二进制补码/5295284) 记法来表示有符号整数。因此，在上面的 **示例 3** 中，输入表示有符号整数 `-3`。
>
> **示例 1：**
>
> ```
> 输入：n = 11 (控制台输入 00000000000000000000000000001011)
> 输出：3
> 解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 128 (控制台输入 00000000000000000000000010000000)
> 输出：1
> 解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
> ```
>
> **示例 3：**
>
> ```
> 输入：n = 4294967293 (控制台输入 11111111111111111111111111111101，部分语言中 n = -3）
> 输出：31
> 解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
> ```
>
> **提示：**
>
> - 输入必须是长度为 `32` 的 **二进制串** 。

**1）将n转换为二进制数统计1的个数**

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while (n){
            res += (n%2);
            n /= 2;
        }
        return res;
    }
};
```

在将数字转换为二进制时，需要用到除留余数法，最后将结果reverse。这里只统计1的个数，所以直接计算。



**2）利用lowbit运算**

`lowbit()`运算：非负整数x在二进制表示下最低位1及其后面的0构成的数值。
`lowbit(12)=lowbit([1100]2)=[100]2=4 `
在计算机存储整数中一般是采用补码存储，并且把补码所表示的整数x变成其相反数-x，相当于把x的每一位取反并加上1.

`lowbit(12) = (1100) & (0011 + 0001) = (1100) & (0100) = 0100 = 4` 

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while (n){
            n -= (n & -n);
            res++;
        }
        return res;
    }
};
```

lowbit运算时树状数组的基础。

[树状数组](C:\Users\hp-pc\Desktop\C++\Algorithm\挑战程序设计竞赛.md)



### （2）[不用加减乘除做加法](https://leetcode.cn/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=fkc5gdm) ❤

> 写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
>
> **示例:**
>
> ```
> 输入: a = 1, b = 1
> 输出: 2
> ```
>
> **提示：**
>
> - `a`, `b` 均可能是负数或 0
> - 结果不会溢出 32 位整数

利用位运算来模拟加法

```c++
class Solution {
public:
    int add(int a, int b) {
        while (b){
            int carry = (unsigned int) (a & b) << 1;
            a ^= b;
            b = carry;
        }
        return a;
    }
};
```





## 22 位运算（中等）

### （1）[数组中数字出现的次数](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
>
> **示例 1：**
>
> ```
> 输入：nums = [4,1,4,6]
> 输出：[1,6] 或 [6,1]
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [1,2,10,4,1,4,3,3]
> 输出：[2,10] 或 [10,2]
> ```
>
> **限制：**
>
> - `2 <= nums.length <= 10000`

**题解：**[剑指 Offer 56 - I. 数组中数字出现的次数（位运算，清晰图解）](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/solutions/572857/jian-zhi-offer-56-i-shu-zu-zhong-shu-zi-tykom/)

**预备知识：**

[只出现一次的数字](https://leetcode.cn/problems/single-number/description/)

> 给你一个 **非空** 整数数组 `nums` ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
>
> 你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

思路：

> 异或运算有以下几个特点：
> 一个数和 0 做 XOR 运算等于本身：a⊕0 = a
> 一个数和其本身做 XOR 运算等于 0：a⊕a = 0
> XOR 运算满足交换律和结合律：a⊕b⊕a = (a⊕a)⊕b = 0⊕b = b

代码

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for (int &i : nums) res ^= i;
        return res;
    }
};
```



对于本题而言，有两个不同的数字只出现了一次，其余的数字都出现了两次，所以不能直接进行异或来得到答案

设两个只出现一次的数字为 x , y ，由于 x≠y，则 x 和 y 二进制至少有一位不同（即分别为 0 和 1 ），根据此位可以将 nums 拆分为分别包含 x 和 y 的两个子数组。

易知两子数组都满足 「除一个数字之外，其他数字都出现了两次」。因此，仿照以上简化问题的思路，分别对两子数组遍历执行异或操作，即可得到两个只出现一次的数字 x, y 。



**算法流程**

- **1、遍历 nums 执行异或**
  设整型数组 nums=[a,a,b,b,...,x,y] ，对 nums 中所有数字执行异或，得到的结果为 x⊕y 

- **2、循环左移计算m**

  - 根据异或运算定义，若整数 x⊕y 某二进制位为 1 ，则 x 和 y 的此二进制位一定不同。换言之，找到 x⊕某位为1 的二进制位，即可将数组 nums 拆分为上述的两个子数组。根据与运算特点，可知对于任意整数 a 有：

    - 若 a&0001=1 ，则 a 的第一位为 1 ；
    - 若 a&0010=1 ，则 a 的第二位为 1 ；
    - 以此类推……

  - 因此，初始化一个辅助变量 m=1 ，通过与运算从右向左循环判断，可 获取整数 x⊕y 首位 1 ，记录于 m 中，代码如下：

    ```c++
    while(z & m == 0) // m 循环左移一位，直到 z & m ！= 0
        m <<= 1
    ```

- **3、拆分 nums 为两个子数组：**

- **4、分别遍历两个子数组执行异或：**
  通过遍历判断 nums 中各数字和 m 做与运算的结果，可将数组拆分为两个子数组，并分别对两个子数组遍历求异或，则可得到两个只出现一次的数字

代码

```c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int x = 0, y = 0, n = 0, m = 1;

        for (int &i : nums) n ^= i;

        while ((n&m) == 0)  m <<= 1;	//找到从右往左不为0的那位1所表示的数，例如1, 10, 100...
        
        for (int &i : nums){
            if (m&i){					//利用m来划分两个子数组
                x ^= i;
            }else
                y ^= i;
        }

        return {x, y};
    }
};
```





### （2）[数组中数字出现的次数](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。
>
> **示例 1：**
>
> ```
> 输入：nums = [3,4,3,3]
> 输出：4
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [9,1,7,9,7,9,7]
> 输出：1
> ```
>
> **限制：**
>
> - `1 <= nums.length <= 10000`
> - `1 <= nums[i] < 2^31`

感觉有点类似于投机取巧的题目：利用一个32位的hashTable来记录每一位上1出现的次数。由于其中有某个元素只出现了一次，那么它二进制为1对应的hashTable位上必定%3 !=0 （对应的hastTabel[i] = 1 || hashTable[i] = 4）

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int hashTable[32] = {0};
        for (int i : nums){
            for (int j = 31; j >= 0; j--){
                hashTable[j] += (i >> (31-j)) & 1;// &1只关心这一位是1还是0
            }
        }

        int res = 0;
        for (int j = 0; j < 32; j++){
            res = (res * 2) + (hashTable[j] % 3 & 1);//同样，只关心这一位是0还是1
        }

        return res;
    }
};
```





## 23 数学（简单）

### （1）[数组中出现次数超过一半的数字](https://leetcode.cn/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
> **示例 1:**
>
> ```
> 输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
> 输出: 2
> ```
>
> **限制：**
>
> ```
> 1 <= 数组长度 <= 50000
> ```

为了方便描述，这里用众数来指代数组中出现超过一半的这个数

1）排序，位于中点位置上的元素必定是众数	

​	时间复杂度$O(nlogn)$，空间复杂度$O(n)$

​	面试官看了直接让回去等消息



2）哈希表

​	时间复杂度$O(n)$，空间复杂度$O(n)$

​	面试官直摇头



3）投票法 （同归于尽法）

假定第一个数是我们要找的数记为majority。遍历nums数组，如果nums[i]和majority相等，出现的次数cnt++，否则cnt--。当cnt减小为0是，需要更新majority为当前的nums[i]，并将cnt = 1

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int majority = nums[0], cnt = 1;
        for (int i = 1; i < nums.size(); i++){
            if (majority == nums[i])    cnt++;
            else    cnt--;

            if (cnt == 0){
                majority = nums[i];
                cnt++;
            }
        }
        return majority;
    }
};
```





### （2）[构建乘积数组](https://leetcode.cn/problems/gou-jian-cheng-ji-shu-zu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 给定一个数组 `A[0,1,…,n-1]`，请构建一个数组 `B[0,1,…,n-1]`，其中 `B[i]` 的值是数组 `A` 中除了下标 `i` 以外的元素的积, 即 `B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]`。不能使用除法。
>
> **示例:**
>
> ```
> 输入: [1,2,3,4,5]
> 输出: [120,60,40,30,24]
> ```
>
> **提示：**
>
> - 所有元素乘积之和不会溢出 32 位整数
> - `a.length <= 100000`

本质上和利用前缀和思想一样，只不过这里换成了乘积

```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int n = a.size();
        vector<int> left(n, 1), res(n);    //利用left[i]来存储a[0...i-1]之间的乘积
        for (int i = 1; i < n; i++)
            left[i] = left[i-1] * a[i-1];

        int right = 1;
        for (int i = n-1; i >= 0; i--){
            res[i] = left[i] * right;
            right *= a[i];
        }

        return res;
    }
};
```

注意，在计算左右乘积时只需要一个left或者right数组用来存储即可。再沿着相反的方向遍历一遍数组，一边计算答案一边更新right或left





## 24 数学（中等）

### （1）[剪绳子](https://leetcode.cn/problems/jian-sheng-zi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m-1]` 。请问 `k[0]*k[1]*...*k[m-1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
>
> **示例 1：**
>
> ```
> 输入: 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1
> ```
>
> **示例 2:**
>
> ```
> 输入: 10
> 输出: 36
> 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
> ```
>
> **提示：**
>
> - `2 <= n <= 58`

**1）数学**

对于n>4的整数而言，3*(n-3) > n（移项可以得到n>4.5），所以对于>4的整数n应该尽可能拆分成3和(n-3)

时间复杂度(n)实际上是O(n/3)

```c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n <= 3)  return n-1;
        int product = 1;
        while (n > 4){
            product *= 3;
            n -= 3;
        }
        product *= n;
        return product;
    }
};
```



**2）动态规划**

状态定义，`dp[i]`为将`i`拆分为`m`个数可以获得的最大值

i可以拆分为`1 i-1, 2 i-2, ... j i-j..., `同样`i-j`也可以继续拆分

对于`dp[i] = max(j * (i-j), j * dp[i-j])   j = 1...i-2`

时间复杂度$O(n^2$)

```c++
class Solution {
public:
    int cuttingRope(int n) {
        int dp[60] = {0};
        dp[2] = 1;
        for (int i = 3; i <= n; i++){
            for (int j = 1; j < i; j++)
                dp[i] = max(dp[i], max(j * (i-j), j * dp[i-j]));
        }
        return dp[n];
    }
};
```





### （2）[和为s的连续正数序列](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。
>
> 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
>
> **示例 1：**
>
> ```
> 输入：target = 9
> 输出：[[2,3,4],[4,5]]
> ```
>
> **示例 2：**
>
> ```
> 输入：target = 15
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]
> ```
>
> **限制：**
>
> - `1 <= target <= 10^5`

**利用前缀和进行计算**

```c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<int> nums;
        vector<vector<int>> res;
        int sum = 0;

        for (int i = 1, j = i; j < target; j++){
            sum += j;
            nums.push_back(j);
            while (sum > target){
                nums.erase(nums.begin());
                sum -= i;
                i++;
            }
            if (sum == target){
                res.push_back(nums);
            }

            if (2*j > target)   break;
        }

        return res;
    }
};
```



### （3）[圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。
>
> 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
>
> 
>
> **示例 1：**
>
> ```
> 输入: n = 5, m = 3
> 输出: 3
> ```
>
> **示例 2：**
>
> ```
> 输入: n = 10, m = 17
> 输出: 2
> ```
>
> **限制：**
>
> - `1 <= n <= 10^5`
> - `1 <= m <= 10^6`

直接进行模拟时间复杂度$O(n \times m)$

**题解：**[换个角度举例解决约瑟夫环](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solutions/178427/huan-ge-jiao-du-ju-li-jie-jue-yue-se-fu-huan-by-as/)

*f*(*n*,*m*)={ 0, (*f*(*n*−1,*m*)+*m*)%n* 	

> +m 推出上一个位置的来由了。
>
> 其实就是第一轮，删掉一个 (m-1) 的数字之后， 从接下来的数开始重新编号，相当于把所有的位置左移了m位。 所以我们要返回上一层的时候，把删掉的数字补回去之后，再右移m位，即所有坐标 +m 然后 %n ，(n是上一层的个数)

```c++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int pos = 0;
        for (int i = 2; i <= n; i++){
            pos = (pos + m) % i;
        }
        return pos;
    }
};
```



## 25 模拟（中等）

### （1）[顺时针打印矩阵](https://leetcode.cn/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
>
> **示例 1：**
>
> ```
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
> 输出：[1,2,3,6,9,8,7,4,5]
> ```
>
> **示例 2：**
>
> ```
> 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
> 输出：[1,2,3,4,8,12,11,10,9,5,6,7]
> ```
>
> **限制：**
>
> - `0 <= matrix.length <= 100`
> - `0 <= matrix[i].length <= 100`

按照题意模拟，对顺时针四个方向按顺序进行遍历，注意数组为空的情况

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        if (matrix.size() == 0) return {};  
        vector<int> res;
        int up = 0, down = matrix.size()-1, left = 0, right = matrix[0].size()-1;
        while (up <= down && left <= right){
            //顺时针：右下左上四个方向
            for (int j = left; j <= right; j++)
                res.push_back(matrix[up][j]);
            if (++up > down)    break;

            for (int i = up; i <= down; i++)
                res.push_back(matrix[i][right]);
            if (--right < left) break;

            for (int j = right; j >= left; j--)
                res.push_back(matrix[down][j]);
            if (--down < up)    break;

            for (int i = down; i >= up; i--)
                res.push_back(matrix[i][left]);
            if (++left > right) break;
        }

        return res;
    }
};
```



### （2）[栈的压入、弹出序列](https://leetcode.cn/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。
>
> **示例 1：**
>
> ```
> 输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
> 输出：true
> 解释：我们可以按以下顺序执行：
> push(1), push(2), push(3), push(4), pop() -> 4,
> push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
> ```
>
> **示例 2：**
>
> ```
> 输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
> 输出：false
> 解释：1 不能在 2 之前弹出。
> ```
>
> **提示：**
>
> 1. `0 <= pushed.length == popped.length <= 1000`
> 2. `0 <= pushed[i], popped[i] < 1000`
> 3. `pushed` 是 `popped` 的排列。

利用栈来模拟，依次将pushed中的元素入栈，如果栈顶元素和`poped[j]`元素相等，那么就将当前元素出栈 `j++`。

最后判断栈是否为空

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
         int j = 0;
         stack<int> st;
         for (int i = 0; i < pushed.size(); i++){
             st.push(pushed[i]);
             while (st.size() && popped[j] == st.top()){
                st.pop();
                j++;
             }
         }
         return st.empty();
    }
};
```





## 26 字符串（中等）

### （1）[表示数值的字符串](https://leetcode.cn/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤待解决！

> 请实现一个函数用来判断字符串是否表示**数值**（包括整数和小数）。
>
> **数值**（按顺序）可以分成以下几个部分：
>
> 1. 若干空格
> 2. 一个 **小数** 或者 **整数**
> 3. （可选）一个 `'e'` 或 `'E'` ，后面跟着一个 **整数**
> 4. 若干空格
>
> **小数**（按顺序）可以分成以下几个部分：
>
> 1. （可选）一个符号字符（`'+'` 或 `'-'`）
> 2. 下述格式之一：
>    1. 至少一位数字，后面跟着一个点 `'.'`
>    2. 至少一位数字，后面跟着一个点 `'.'` ，后面再跟着至少一位数字
>    3. 一个点 `'.'` ，后面跟着至少一位数字
>
> **整数**（按顺序）可以分成以下几个部分：
>
> 1. （可选）一个符号字符（`'+'` 或 `'-'`）
> 2. 至少一位数字
>
> 部分**数值**列举如下：
>
> - `["+100", "5e2", "-123", "3.1416", "-1E-16", "0123"]`
>
> 部分**非数值**列举如下：
>
> - `["12e", "1a3.14", "1.2.3", "+-5", "12e+5.4"]`
>
>  
>
> **示例 1：**
>
> ```
> 输入：s = "0"
> 输出：true
> ```
>
> **示例 2：**
>
> ```
> 输入：s = "e"
> 输出：false
> ```
>
> **示例 3：**
>
> ```
> 输入：s = "."
> 输出：false
> ```
>
> **示例 4：**
>
> ```
> 输入：s = "    .1  "
> 输出：true
> ```
>
>  
>
> **提示：**
>
> - `1 <= s.length <= 20`
> - `s` 仅含英文字母（大写和小写），数字（`0-9`），加号 `'+'` ，减号 `'-'` ，空格 `' '` 或者点 `'.'` 。

**有限状态机**





### （2）[把字符串转换成整数](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。
>
> 首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
>
> 当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
>
> 该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
>
> 注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
>
> 在任何情况下，若函数不能进行有效的转换时，请返回 0。
>
> **说明：**
>
> 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231, 231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。
>
> **示例 1:**
>
> ```
> 输入: "42"
> 输出: 42
> ```
>
> **示例 2:**
>
> ```
> 输入: "   -42"
> 输出: -42
> 解释: 第一个非空白字符为 '-', 它是一个负号。
>      我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
> ```
>
> **示例 3:**
>
> ```
> 输入: "4193 with words"
> 输出: 4193
> 解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
> ```
>
> **示例 4:**
>
> ```
> 输入: "words and 987"
> 输出: 0
> 解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
>      因此无法执行有效的转换。
> ```
>
> **示例 5:**
>
> ```
> 输入: "-91283472332"
> 输出: -2147483648
> 解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
>      因此返回 INT_MIN (−231) 。
> ```

题解：[把字符串转换成整数（数字越界处理，清晰图解）](https://leetcode.cn/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/solutions/201301/mian-shi-ti-67-ba-zi-fu-chuan-zhuan-huan-cheng-z-4/)

**解题思路：**
根据题意，有以下四种字符需要考虑：

- 1、首部空格： 删除之即可；
- 2、符号位： 三种情况，即 ''+++'' , ''−-−'' , ''无符号" ；新建一个变量保存符号位，返回前判断正负即可。
- 3、非数字字符： 遇到首个非数字的字符时，应立即返回。
- 4、数字字符：
  - 字符转数字： “此数字的 ASCII 码” 与 “ 000 的 ASCII 码” 相减即可；
  - 数字拼接： 若从左向右遍历数字，设当前位字符为 ccc ，当前位数字为 xxx ，数字结果为 resresres ，则数字拼接公式为：
    res=10×res+x  x=ascii(c)−ascii(′0′) 

**数字越界处理：**

题目要求返回的数值范围应在$[-2^{31}, 2^{31} - 1]$，因此需要考虑数字越界问题。而由于题目指出 环境只能存储 32 位大小的有符号整数 ，因此判断数字越界时，要始终保持 res 在 int 类型的取值范围内。

在每轮数字拼接前，判断res 在此轮拼接后是否超过 21474836472 ，若超过则加上符号位直接返回。 设数字拼接边界 `bndry`=2147483647//10 ，则以下两种情况越界：

<img src="剑指offer.assets/d1b06a91801868af63f6e309da31bcfa01c7b6c385529fb974389a61e454cd12-Picture2.png" alt="Picture2.png" style="zoom: 50%;" />

`res>bndry`	情况一：执行拼接10×res ≥ 2147483650越界

`res=bndry,x>7`	情况二：拼接后是2147483648或2147483649越界

复杂度分析：
时间复杂度 O(N) ： 其中 N 为字符串长度，线性遍历字符串占用 O(N) 时间。
空间复杂度 O(N) ： 删除首尾空格后需建立新字符串，最差情况下占用 O(N) 额外空间。

> `c[j] > '7'` 这里处理很巧妙，判断 > '7' , 看似没有考虑 MIN, 但其实无论是 = '8' ,还是 >'8',返回的都是MIN。 学习了

```c++
class Solution {
public:
    int strToInt(string str) {
        if (str.size() == 0)
            return 0;

        int i, n = str.size();
        for (i = 0; i < str.size() && str[i] == ' '; i++);
        //退出循环时有str[i] != ' '

        bool sign = true;   //默认是整数
        if (str[i] == '-'){
            sign = false;
            i++;
        }else if (str[i] == '+')
            i++;

        if (!isdigit(str[i]))   //+-号之后或者空格后的非空字符不是整数，直接返回0
            return 0;

        int res = 0, border = INT_MAX/10;
        for (int j = i; j < n && isdigit(str[j]); j++){
            if (res > border || (res == border && str[j] > '7'))
                return sign ? INT_MAX : INT_MIN;
            res = res * 10 + (str[j] - '0');
        }


        return sign ? res : -res;
    }
};
```





## 27 栈与队列（困难）

### （1）[滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。
>
> **示例:**
>
> ```
> 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
> 输出: [3,3,5,5,6,7] 
> 解释: 
> 
>   滑动窗口的位置                最大值
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7
> ```
>
> **提示：**
>
> 你可以假设 *k* 总是有效的，在输入数组 **不为空** 的情况下，`1 ≤ k ≤ nums.length`。

利用单调队列可以求得滑动窗口内的最大值和最小值

但是这里的队列有点特殊，是双端队列，普通的队列只能在队头弹出元素。但当队尾元素<=nums[i]时，我们需要在队尾弹出元素，所以使用的是双端队列

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        //单调队列 求最大值 那么nums[i]前面比它小的元素永远不会成为正确答案 前面只有比nums[i]大的元素（想等的话，由于nums[i]在这个相等元素的后面，所以nums[i]要比前面的数更优）
        deque<int> dq;
        vector<int> res;
        for (int i = 0; i < nums.size(); i++){
            while (dq.size() && nums[i] >= nums[dq.back()])
                dq.pop_back();
            while (dq.size() && i-dq.front() >= k)
                dq.pop_front();
            dq.push_back(i);
            if (i >= k-1)   res.push_back(nums[dq.front()]);
        }
        return res;
    }
};
```



### （2）[队列的最大值](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 请定义一个队列并实现函数 `max_value` 得到队列里的最大值，要求函数`max_value`、`push_back` 和 `pop_front` 的**均摊**时间复杂度都是O(1)。
>
> 若队列为空，`pop_front` 和 `max_value` 需要返回 -1
>
> **示例 1：**
>
> ```
> 输入: 
> ["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
> [[],[1],[2],[],[],[]]
> 输出: [null,null,null,2,1,2]
> ```
>
> **示例 2：**
>
> ```
> 输入: 
> ["MaxQueue","pop_front","max_value"]
> [[],[],[]]
> 输出: [null,-1,-1]
> ```
>
> **限制：**
>
> - `1 <= push_back,pop_front,max_value的总操作数 <= 10000`
> - `1 <= value <= 10^5`



[题解链接：如何解决 O(1) 复杂度的 API 设计题]([面试题59 - II. 队列的最大值 - 力扣（Leetcode）](https://leetcode.cn/problems/dui-lie-de-zui-da-zhi-lcof/solutions/136701/ru-he-jie-jue-o1-fu-za-du-de-api-she-ji-ti-by-z1m/))

<img src="剑指offer.assets/839e8856c964e437c7bd17faf24d1b0524b35e819296af3d81866c15b77fa478-59.gif" alt="59.gif" style="zoom: 50%;" />

利用变量max来记录队列中的最大值，可以在O(1)时间内得到队列中最大的元素，但当max出队之后，如何获得更新之后的最大元素呢

![fig3.gif](剑指offer.assets/9d038fc9bca6db656f81853d49caccae358a5630589df304fc24d8999777df98-fig3.gif)利用双端队列来维护一个单调递减的队列

```c++
class MaxQueue {
private:
    queue<int> q;
    deque<int> dq;
public:
    

    MaxQueue() {
        
    }
    
    int max_value() {
        return dq.size() ? dq.front() : -1;
    }
    
    void push_back(int value) {
        q.push(value);
        while (dq.size() && value > dq.back())
            dq.pop_back();
        dq.push_back(value);
    }
    
    int pop_front() {
        if (dq.empty()) return -1;
        int res = q.front();
        q.pop();
        if (res == dq.front())    dq.pop_front();
        return res;  
    }
};
```



## 28 搜索与回溯算法（困难）

### （1）[序列化二叉树](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) ❤

> 请实现两个函数，分别用来序列化和反序列化二叉树。
>
> 你需要设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
>
> **提示：**输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 [LeetCode 序列化二叉树的格式](https://support.leetcode-cn.com/hc/kb/article/1567641/)。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。
>
> **示例：**
>
> <img src="剑指offer.assets/serdeser.jpg" alt="img" style="zoom:67%;" />
>
> ```
> 输入：root = [1,2,3,null,null,4,5]
> 输出：[1,2,3,null,null,4,5]
> ```

利用DFS来序列化和反序列化

#### 1）使用string

```c++
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root)  return "#";		//利用#来标记空节点
        return to_string(root->val) + "," + serialize(root->left) + "," + serialize(root->right);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        return mydeserialize(data);		//需要修改data，所以自定义的函数里传递的是&
    }

    TreeNode* mydeserialize(string& data) {
        if (data[0] == '#'){	//2,#,#	遇到空节点#，如果它后面还有子树，那么str = str.substr(2)
            if (data.size() > 1)    data = data.substr(2);
            return NULL;
        }
        TreeNode *root = new TreeNode(helper(data));    //字符串会发生变化
        root->left = mydeserialize(data); 
        root->right = mydeserialize(data);
        return root;    
    }

    int helper(string &str){	//找到被,分割的字符串，将其转换为int来获取节点的数值
        int pos = str.find(",");
        int val = stoi(str.substr(0, pos));
        str = str.substr(pos+1);
        return val;
    }
};
```



#### 2）利用stringstream

> C++中的stringstream是一个类，可以用于将字符串转换为各种数据类型，并将数据类型转换为字符串。stringstream是基于iostream库的一个类，因此它可以像其他iostream对象一样进行输入和输出操作。



上述string需要频繁进行修改，导致一些不必要的开销，可以利用stringstream来提高效率

```c++
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root)  return "#";
        return to_string(root->val) + "," + serialize(root->left) + "," + serialize(root->right);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        stringstream s(data);
        return mydeserialize(s);
    }

    TreeNode* mydeserialize(stringstream& s) {
        string str;
        getline(s, str, ',');	//这行命令的作用是从stringstream对象s中读取数据，以逗号（','）为分隔符，读取一行数据，并将结果存储在字符串变量str中。
        if (str == "#"){
            return NULL;
        }
        TreeNode *root = new TreeNode(stoi(str));    //字符串会发生变化
        root->left = mydeserialize(s); 
        root->right = mydeserialize(s);
        return root;    
    }
};
```



### （2）[字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

和[47. 全排列 II - 力扣（Leetcode）](https://leetcode.cn/problems/permutations-ii/)一致，只不过将数字改成了字符

> 输入一个字符串，打印出该字符串中字符的所有排列。
>
> 你可以以任意顺序返回这个字符串数组，但里面不能有重复元素。
>
> **示例:**
>
> ```
> 输入：s = "abc"
> 输出：["abc","acb","bac","bca","cab","cba"]
> ```
>
> **限制：**
>
> ```
> 1 <= s 的长度 <= 8
> ```

**key point：如何进行剪枝**

```c++
class Solution {
    int n;
    string temp;
    vector<string> res;
    vector<bool> used;
public:
    vector<string> permutation(string s) {
        //字符串中可能存在重复的元素，需要进行剪枝
        n = s.size();
        used.resize(n);    //利用used[i]来标记i位置上的元素是否被使用
        sort(s.begin(), s.end());
        generateP(0, s);
        return res;
    }

    void generateP(int index, const string &s){
        if (index == n){
            res.push_back(temp);
            return ;
        }
        for (int i = 0; i < n; i++){
            //剪枝aa    两个元素相同但是前面一个元素还未被使用，那么我们会将之前的加入，但是这会导致重复
            if (i && s[i] == s[i-1] && !used[i-1])  continue;
            if (!used[i]){
                temp += s[i];
                used[i] = true;
                generateP(index+1, s);
                temp.pop_back();
                used[i] = false;
            }
        }
    }
};
```



### 下一个排列 ❤

> 整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。
>
> - 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。
>
> 整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。
>
> - 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
> - 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
> - 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。
>
> 给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。
>
> 必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。
>
> **示例 1：**
>
> ```
> 输入：nums = [1,2,3]
> 输出：[1,3,2]
> ```
>
> **示例 2：**
>
> ```
> 输入：nums = [3,2,1]
> 输出：[1,2,3]
> ```
>
> **示例 3：**
>
> ```
> 输入：nums = [1,1,5]
> 输出：[1,5,1]
> ```
>
> **提示：**
>
> - `1 <= nums.length <= 100`
> - `0 <= nums[i] <= 100`

在C++的库函数中有 `next_permutation(nums.begin(), nums.end());`可以用来实现本题的功能

[题解：下一个排列算法详解：思路+推导+步骤，看不懂算我输！](https://leetcode.com/problemset/all/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china)

标准的 “下一个排列” 算法可以描述为：

1. 从后向前 查找第一个 相邻升序 的元素对 (i,j)，满足 A[i] < A[j]。此时 [j,end) 必然是降序

2. 在 [j,end) 从后向前 查找第一个满足 A[i] < A[k] 的 k。A[i]、A[k] 分别就是上文所说的「小数」、「大数」

3. 将 A[i] 与 A[k] 交换

4. 可以断定这时 [j,end) 必然是降序，逆置 [j,end)，使其升序

5. 如果在步骤 1 找不到符合的相邻元素对，说明当前 [begin,end) 为一个降序顺序，则直接跳到步骤 4

   ```c++
   class Solution {
   public:
       void nextPermutation(vector<int>& nums) {
           int i, j, k;
           int n = nums.size();
           for (i = n-2, j = n-1; i >= 0 && j >= 0; i--, j--){
               if (nums[i] < nums[j])    break;  //从后往前找打递增的两个数
           }
           if (i == -1){
               reverse(nums.begin(), nums.end());
               return ;
           }
           for (k = n-1; k > i && nums[k] <= nums[i]; k--);
           // cout << i << " " << j << " " << k << endl;
           swap(nums[i], nums[k]);
           sort(nums.begin() + j, nums.end());
           
       }
   };
   ```

   



## 29 动态规划（困难） ❤

### （1）[正则表达式匹配](https://leetcode.cn/problems/zheng-ze-biao-da-shi-pi-pei-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) 

> 给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。
>
> - `'.'` 匹配任意单个字符
> - `'*'` 匹配零个或多个前面的那一个元素
>
> 所谓匹配，是要涵盖 **整个** 字符串 `s`的，而不是部分字符串。
>
> **示例 1：**
>
> ```
> 输入：s = "aa", p = "a"
> 输出：false
> 解释："a" 无法匹配 "aa" 整个字符串。
> ```
>
> **示例 2:**
>
> ```
> 输入：s = "aa", p = "a*"
> 输出：true
> 解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
> ```
>
> **示例 3：**
>
> ```
> 输入：s = "ab", p = ".*"
> 输出：true
> 解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
> ```
>
> **提示：**
>
> - `1 <= s.length <= 20`
> - `1 <= p.length <= 20`
> - `s` 只包含从 `a-z` 的小写字母。
> - `p` 只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。
> - 保证每次出现字符 `*` 时，前面都匹配到有效的字符





### （2）[丑数](https://leetcode.cn/problems/chou-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6) 

> 我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
>
> **示例:**
>
> ```
> 输入: n = 10
> 输出: 12
> 解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
> ```
>
> **说明:** 
>
> 1. `1` 是丑数。
> 2. `n` **不超过**1690。

[剑指 Offer 49. 丑数（动态规划，清晰图解）]([剑指 Offer 49. 丑数 - 力扣（Leetcode）](https://leetcode.cn/problems/chou-shu-lcof/solutions/182045/mian-shi-ti-49-chou-shu-dong-tai-gui-hua-qing-xi-t/))

利用文字表示

```c++
/* 
2: currentUglyNumber = min(1 * 2, 1 * 3, 1* 5) = 2
    produced by p2, so p2++ 
    dp [1, 2]
3: currentUglyNumber = min(2 * 2, 1 * 3, 1 * 5) = 3
    produced by p3, so p3++
    dp [1, 2, 3]
4: currentUglyNumber = min(2 * 2, 2 * 3, 1 * 5) = 4
    produced by p2, so p2++
    dp [1, 2, 3, 4]
5: currentUglyNumber = min(3 * 2, 2 * 3, 1 * 5) = 5
    produced by p5, so p5++
    dp [1, 2, 3, 4, 5]
6: currentUglyNumber = min(3 * 2, 2 * 3, 2 * 5) = 6
    produced by p2 AND p3, so p2++ and p3++
    dp [1, 2, 3, 4, 5, 6]
7: currentUglyNumber = min(4 * 2, 3 * 3, 2 * 5) = 8
    produced by p2, so p2++
    dp [1, 2, 3, 4, 5, 6, 8]
8: currentUglyNumber = min(5 * 2, 3 * 3, 2 * 5) = 9
    produced by p3, so p3++
    dp [1, 2, 3, 4, 5, 6, 8, 9]
9: currentUglyNumber = min(5 * 2, 4 * 3, 2 * 5) = 10
    produced by p2 and p5, p2++ and p5++
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10]
10: currentUglyNumber = min(8 * 2, 4 * 3, 3 * 5) = 12
    NOTE the 8, that's the value at index 6
    produced by p3, p3++
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12]
11: currentUglyNumber = min(8 * 2, 5 * 3, 3 * 5) = 15
    produced by p3 and p5, p3++ and p5++
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15]
12: currentUglyNumber = min(8 * 2, 8 * 3, 4 * 5) = 16
    produced by p2, p2++
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16]
13: currentUglyNumber = min(9 * 2, 8 * 3, 4 * 5) = 18
    produced by p2, p2++   
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, 18]
14: currentUglyNumber = min(10 * 2, 8 * 3, 4 * 5) = 20
    produced by p2 and p5, p2++ p5++   
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, 18, 20]
15: currentUglyNumber = min(12 * 2, 8 * 3, 5 * 5) = 24
    produced by p2 and p3, p2++ p3++   
    dp [1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, 18, 20, 24]

etc.. 
*/
```

代码

```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        if (n < 2)  return 1;
        vector<int> dp(n);
        dp[0] = 1;
        int p2 = 0, p3 = 0, p5 = 0;    //倍数
        for (int i = 1; i < n; i++){
            int res = min(2*dp[p2], min(3*dp[p3], 5*dp[p5]));
            if (res == 2*dp[p2])    p2++;
            if (res == 3*dp[p3])    p3++;
            if (res == 5*dp[p5])    p5++;
            dp[i] = res;
        }
        return dp[n-1];
    }
};
```



### （3）[n个骰子的点数](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。
>
> 你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。
>
> **示例 1:**
>
> ```
> 输入: 1
> 输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
> ```
>
> **示例 2:**
>
> ```
> 输入: 2
> 输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
> ```
>
> **限制：**
>
> ```
> 1 <= n <= 11
> ```

动态规划：

```c++
class Solution {
    double dp[12][70];
public:
    vector<double> dicesProbability(int n) {
        vector<double> res(5*n + 1);
        //dp[i][j] i颗骰子面数之和为j的概率
        for (int j = 1; j <= 6; j++)
            dp[1][j] = 1.0 / 6;
        //状态转移方程  dp[i][j]由于在i-1颗骰子的基础上增加了一颗，那么增加的这颗的面数可能是1-6中的任意值
        for (int i = 2; i <= n; i++){
            for (int j = i; j <= 6*i; j++){
                for (int k = 1; k <= 6; k++){
                    if (j-k >= 0)
                        dp[i][j] += dp[i-1][j-k] * (1.0 / 6);
                }
            }
        }
        for (int j = n; j <= 6*n; j++)
            res[j-n] = dp[n][j];
        return res;
    }
};
```



## 30 分治算法（困难）

### （1）[打印从1到最大的n位数](https://leetcode.cn/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 输入数字 `n`，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
>
> **示例 1:**
>
> ```
> 输入: n = 1
> 输出: [1,2,3,4,5,6,7,8,9]
> ```
>
> 说明：
>
> - 用返回一个整数列表来代替打印
> - n 为正整数

#### 1）直接进行模拟





### （2）[数组中的逆序对](https://leetcode.cn/problems/shu-zu-zhong-de-ni-xu-dui-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)  ❤

> 在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。
>
> **示例 1:**
>
> ```
> 输入: [7,5,6,4]
> 输出: 5
> ```
>
> **限制：**
>
> ```
> 0 <= 数组长度 <= 50000
> ```

#### 1）归并排序

对于归并排序而言，在合并时左右两个子数组有序，将两个有序的子数组合并为一个更大的数组

```c++
/*
7 5 6 4
5 7		4 6
i		j 
当合并是nums[i] > nums[j]，所以j会后移，当j指向6时nums[i] <= nums[j]，
由于数组是有序的那么nums[i] > nums[mid+1....j-1]，一共j-(mid+1)个元素，可以在O(1)的时间内计算出逆序对的数量
4 5 6 7
*/
```

时间复杂度$O(nlogn)$，空间复杂度$O(n)$

```c++
class Solution {
    int res = 0;
    vector<int> temp;
public: 
    //1、使用归并排序求解
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        temp.resize(n);
        mergeSort(nums, 0, n-1);
        return res;
    }

    void mergeSort(vector<int> &nums, int low, int high){
        if (low < high){
            int mid = low + (high - low) / 2;
            mergeSort(nums, low, mid);
            mergeSort(nums, mid+1, high);
            merge(nums, low, mid, high);
        }
    }

    void merge(vector<int> &nums, int low, int mid, int high){
        int i, j, k;
        for (i = low; i <= high; i++)
            temp[i] = nums[i];
        for (i = low, j = mid+1, k = low; i <= mid && j <= high; k++){
            if (temp[i] <= temp[j]){    //逆序对：temp[i]和temp[mid+1...j-1]
                nums[k] = temp[i++];
                res += j-(mid+1);
            }else
                nums[k] = temp[j++];
        }
        while (i <= mid){
            res += j-(mid+1);
            nums[k++] = temp[i++];
        }
        while (j <= high){
            nums[k++] = temp[j++];
        }
    }
};
```



2）树状数组

对于树状数组而言，可以在`O(logn) 的时间内进行修改，在O(logn)的时间内查找 `>nums[i]` 和 `<nums[i]` 的元素数量

1、离散化

2、对于离散化的 `arr[i]` 而言，统计前面>arr[i]的数，即 `getSum(n) - getSum(arr[i])`

```c++
class Solution {
    static const int MAXN = 50010;
    int n, t[MAXN] = {0};
public:
    int reversePairs(vector<int>& nums) {
        //1、离散化将数组映射到1-n的整数
        n = nums.size();
        vector<pair<int, int>> vec;
        for (int i = 0; i < n; i++){
            vec.push_back({nums[i], i});
        }
        sort(vec.begin(), vec.end());

        vector<int> arr(n);
        for (int i = 0; i < n; i++){
            if (i == 0 || vec[i].first != vec[i-1].first)
                arr[vec[i].second] = i+1;
            else
                arr[vec[i].second] = arr[vec[i-1].second];
        }

        int res = 0;
        for (int i = 0; i < n; i++){
            int cnt = getSum(n) - getSum(arr[i]);   //统计前面比nums[i]大的数
            res += cnt;
            update(arr[i], 1);
        }
        
        return res;
    }

    int lowbit(int x){
        return x & (-x);
    } 

    void update(int x, int v){
        for (int i = x; i <= n; i += lowbit(i))
            t[i] += v;
    }

    int getSum(int x){
        int sum = 0;
        for (int i = x; i ; i -= lowbit(i))
            sum += t[i];
        return sum;
    }
};
```





## 31 数学（困难）❤

### （1）[剪绳子 II](https://leetcode.cn/problems/jian-sheng-zi-ii-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 给你一根长度为 `n` 的绳子，请把绳子剪成整数长度的 `m` 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 `k[0],k[1]...k[m - 1]` 。请问 `k[0]*k[1]*...*k[m - 1]` 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> **示例 1：**
>
> ```
> 输入: 2
> 输出: 1
> 解释: 2 = 1 + 1, 1 × 1 = 1
> ```
>
> **示例 2:**
>
> ```
> 输入: 10
> 输出: 36
> 解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
> ```
>
> **提示：**
>
> - `2 <= n <= 1000`

#### 1）动态规划

直接将剪绳子i的代码复制过来，会导致错误

> 这一题已经不能用动态规划了，取余之后max函数就不能用来比大小了。
>
> 2000000014 会小于 1000000020



#### 2）贪心

数学："Prove that for all integers n > 4, ( ( n-3 ) * 3 ) > n".

```c++
class Solution {
public:
    int cuttingRope(int n) {
        int mod = 1e9 + 7;
        if (n <= 3)  return n-1;
        int res = 1;
        while (n > 4){
            n -= 3;
            res = ((long long)res * 3) % mod;
        }
        res = ((long long)res * n) % mod;
        return res;
    }
};
```





### （2）[1～n 整数中 1 出现的次数](https://leetcode.cn/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 输入一个整数 `n` ，求1～n这n个整数的十进制表示中1出现的次数。
>
> 例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。
>
> **示例 1：**
>
> ```
> 输入：n = 12
> 输出：5
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 13
> 输出：6
> ```
>
> **限制：**
>
> - `1 <= n < 2^31`

对1234567这个数而言，我们统计百位上1出现的次数

在百位上的1，形如01 11 21 ... 91，可以发现每经过1000个数，百位上就会为1，百位上的数[100, 199]一共出现了100次

假设上述百位1的次数是一轮循环，对于这个数而言，从0000 1.. 到1234 1..会有1234个循环

n'= n%1000 = 567，我们再来看从000 - 567百位上1出现的次数

- n' < 100，那么百位上没有出现1的次数

- 100 <= n < 200，那么百位上1出现的次数为n-100+1

- n>=200，就只有[100, 199]这些数百位上为1，一共出现了100次

  利用 min(max(n%1000 - 100 + 1, 0), 100)，就可以代替上述三行

所以1234567百位上1出现的次数为1234 * 100 + 100

同理可以计算出个位、十位、千位...上1出现的次数

可以得到计算公式公式

$\frac{n}{10^{k+1}} \times 10^k+\min \left(\max \left(n \% 10^{k+1}-10^k+1,0\right), 10\right)$

k=0, 1, 2...时分别表示个位、十位、百位

```c++
class Solution {
public:
    int countDigitOne(int n) {
        long long mulk = 1;     //表示10^k
        //计算公式n/(mulk * 10) + mulk + min(max(n%mulk*10 - mulk + 1, 0), mulk)
        int res = 0;
        while (n >= mulk){
            res += n / (mulk * 10) * mulk + min(max(n%(mulk * 10) - mulk + 1, 0LL), mulk);
            mulk *= 10;
        }
        return res;
    }
};
```

时间复杂度$O(logn)$



### （3）[数字序列中某一位的数字](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/?envType=study-plan&id=lcof&plan=lcof&plan_progress=xxofnws6)

> 数字以0123456789101112131415…的格式序列化到一个字符序列中。在这个序列中，第5位（从下标0开始计数）是5，第13位是1，第19位是4，等等。
>
> 请写一个函数，求任意第n位对应的数字。
>
> **示例 1：**
>
> ```
> 输入：n = 3
> 输出：3
> ```
>
> **示例 2：**
>
> ```
> 输入：n = 11
> 输出：0
> ```
>
> **限制：**
>
> - `0 <= n < 2^31`

[面试题44. 数字序列中某一位的数字（迭代 + 求整 / 求余，清晰图解）](https://leetcode.cn/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/solutions/219252/mian-shi-ti-44-shu-zi-xu-lie-zhong-mou-yi-wei-de-6/)

找规律题，意义不大



```
class Solution {
public:
    int findNthDigit(int n) {
        if (n == 0) return 0;
        long long digit = 1, start = 1, count = 9;
        while (n > count){
            n -= count;
            digit++;
            start *= 10;
            count = 9 * digit * start;
        }
        int num = start + (n - 1) / digit;  //在nums数中的某一位
        return (to_string(num)[(n-1) % digit] - '0');
    }
};
```
