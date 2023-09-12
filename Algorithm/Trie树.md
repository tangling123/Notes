# Trie树

### 一般的多叉树

```c++
struct TreeNode {
    VALUETYPE value;    //结点值
    TreeNode* children[NUM];    //指向孩子结点
};
```

### Trie树 

```c++
struct TrieNode {
    bool isEnd; //该结点是否是一个串的结束
    TrieNode* next[26]; //字母映射表
};
```

**TrieNode结点中并没有直接保存字符值的数据成员**
**这时字母映射表next 的妙用就体现了，TrieNode* next[26]中保存了对当前结点而言下一个可能出现的所有字符的链接，**
**因此我们可以通过一个父结点来预知它所有子结点的值：**

```c++
for (int i = 0; i < 26; i++) {
    char ch = 'a' + i;
    if (parentNode->next[i] == NULL) {
        说明父结点的后一个字母不可为 ch
    } else {
        说明父结点的后一个字母可以是 ch
    }
}
```

[208. 实现 Trie (前缀树) - 力扣（LeetCode）](https://leetcode.cn/problems/implement-trie-prefix-tree/)

#### 1、Trie树的插入

```c++
void insert(string word){
	TrieNode node = this;
	for (char ch : word){
		if (node->next[ch - 'a'] == NULL){//当前字符不在Trie树中 
			node->next[ch - 'a'] = new Trie();//新建一个结点 
		}
		node = node->next[ch - 'a'];//node指向他的孩子结点 
	}
	node->isEnd = True;//表示存在以word结尾的单词 
}
```

#### 2、查找

```c++
bool search(string word){
	TrieNode node = this;
	for (char ch : word){
		if (node->next[ch - 'a'] == NULL){//当前字符不在Trie树中 
			return false; 
		}
		node = node->next[ch - 'a'];//node指向他的孩子结点 
	}
	retutrn node->isEnd;
}
```

#### 3、前缀匹配(不需要判断prefix是否是单词的末尾)

```c++
bool search(string word){
	TrieNode node = this;
	for (char ch : word){
		if (node->next[ch - 'a'] == NULL){//当前字符不在Trie树中 
			return false; 
		}
		node = node->next[ch - 'a'];//node指向他的孩子结点 
	}
	retutrn true;
} 
```

```c++
class Trie {
private:
    struct TrieNode{
        bool isEnd = false;
        TrieNode *next[26];
    };
    TrieNode *root = new TrieNode();
public:
    Trie() {
        
    }
    
    void insert(string word) {
        TrieNode *p = root;
        for (char ch : word){
            if (p->next[ch - 'a'] == nullptr){
                p->next[ch-'a'] = new TrieNode();
            }
            p = p->next[ch-'a'];
        }
        p->isEnd = true;
    }
    
    bool search(string word) {
        TrieNode *p = root;
        for (char ch : word){
            if (p->next[ch - 'a'] == nullptr){
                return false;
            }
            p = p->next[ch-'a'];
        }
        return p->isEnd;
    }
    
    bool startsWith(string prefix) {
        TrieNode *p = root;
        for (char ch : prefix){
            if (p->next[ch - 'a'] == nullptr){
                return false;
            }
            p = p->next[ch-'a'];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```



## Trie树，采用数组的方式构建



```c++
const int N, M;
int son[M][26], cnt[], idx = 0;	//son[p][u]表示p节点的第u个孩子的编号,cnt[u]表示以u号字符为结尾的单词数

void insert(string word){
	int p = 0;	//根节点
	for (char ch : word){
		if (son[p][ch-'a'] == 0)	son[p][u] = ++idx;//新建一个结点
		p = son[p][ch-'a']; 
	}
	cnt[p]++;
}

int search(string word){
	int p = 0; 
	for (char ch : word){
		if (son[p][ch-'a'] == 0)	return 0;
		p = son[p][ch-'a'];
	}
	return cnt[p];
} 
```

