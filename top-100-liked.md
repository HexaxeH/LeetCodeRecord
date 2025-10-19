#### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

思路：两个两的相加直到找出等于target的两个数，通过双层循环，第二层循环遍历第一层循环固定的那个数后面的数，分别与固定的数相加，直到试完所以组合。

```java
class Solution {

  public int[] twoSum(int[] nums, int target) {

​    for (int i = 0; i < nums.length; i++) {

​      for (int j = i + 1; j < nums.length; j++) {

​        if (nums[i] + nums[j] == target) {

​          return new int[] { i, j };

​        }

​      }

​    }

​    return new int[]{};

  }

}
```

![6b325dad8b448e95b957855ca345c4dd](./top-100-liked.assets/6b325dad8b448e95b957855ca345c4dd.png)

#### [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

思路：用flag标记非零元素应该放置的位置，i遍历整个数组，当i指针遇到非零元素时，交换i上的值和flag上的值，且flag标记前进一位。交换到i到达最后且flag标记过所有非零元素

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int flag = 0;
        for(int i = 0;i<nums.size();i++){
            if(nums[i]!=0){
                if(i!=flag){
                int n = nums[i];
                nums[i]=nums[flag];
                nums[flag]=n;
                }
                flag++;
            }
        }
    }
};
```

![94a43ba8365d2f34c14a7d6c27b05f8d](./top-100-liked.assets/94a43ba8365d2f34c14a7d6c27b05f8d.png)

#### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

思路：双重循环逐一比对两个链表的所有节点，第二次循环，指针pb每次重新指向头节点，直到找到第一个相同的节点（相同的地址）。

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *pa = headA;
        while(pa != NULL){
            ListNode *pb = headB;
            while(pb != NULL){
                if(pa == pb){
                    return pa;
                }
                pb = pb->next;
            }
            pa = pa->next;
        }
        return NULL;
    }
};
```

![5642a075331e1ce4564899b0bdb8d34c](./top-100-liked.assets/5e3dc1c990f8c150df451810a6371cf4.png)



#### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

思路：用头插法反转链表，将原链表节点逐个移到新链表头部，实现顺序颠倒。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* newHead = new ListNode();
        ListNode* p = head;
        while(p){
            ListNode* temp = p->next;
            p->next=newHead->next;
            newHead->next=p;
            p=temp;
        }
        ListNode* result = new ListNode();
        result= newHead->next;
        return result;
    }
};
```

![image-20250904185402829](./top-100-liked.assets/image-20250904185402829.png)

#### [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

思路：通过尾插法复制原链表得到相同顺序的副本，再用头插法反转该副本得到逆序链表，最后比较原链表与逆序链表是否一致来判断是否为回文。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* copyHead = new ListNode();
        ListNode* t = copyHead;
        ListNode* curr = head;
        //复制
        while (curr) {
            ListNode* newNode = new ListNode(curr->val);
            t->next = newNode; 
            t = t->next;
            curr = curr->next;
        }
        ListNode* reversedCopyHead = reverseList(copyHead);
        // 比较原链表和反转后的复制链表
        curr = head;
        while (curr&& reversedCopyHead) {
            if (curr->val != reversedCopyHead->val) {
                return false;
            }
            curr = curr->next;
            reversedCopyHead = reversedCopyHead->next;
        }
        return true;
    }
//反转复制的链表
    ListNode* reverseList(ListNode* head) {
        ListNode* newHead = new ListNode();
        ListNode* p = head;
        while (p) {
            ListNode* temp = p->next;
            p->next = newHead->next;
            newHead->next = p;
            p = temp;
        }
        ListNode* result = newHead->next;
        return result;
    }
};
```

![image-20250904191836610](./top-100-liked.assets/image-20250904191836610.png)

#### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

思路：用哈希集合记录已访问节点，从表头开始遍历：若当前节点在集合中，说明有环；若不在，则加入集合并继续遍历下一个节点。visited.count()用于检查集合中是否存在指定元素

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
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode*> visited;
        ListNode* p = head;
        while(p){
            if(visited.count(p)){
                return true;
            }
            visited.insert(p);
            p=p->next;
        }
        return false;
    }
};
```

![image-20250905171320554](./top-100-liked.assets/image-20250905171320554.png)

#### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

思路：`newlist`作为合并的新链表开头，用指针`p`构建新链表，`p1`、`p2`分别遍历两个输入链表。比较`p1`和`p2`指向的值，将较小节点接入新链表，同时移动对应指针，遍历结束后，因为输入链表是升序排列，所以直接将剩余链表接入新链表尾部

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* p1 = list1;
        ListNode* p2 = list2;
        ListNode* newlist = new ListNode();
        ListNode* p = newlist;
        while(p1&&p2){
            if(p1->val<=p2->val){
                p->next = p1;
                p1=p1->next;
            }else{
                p->next = p2;
                p2=p2->next;
            }
            p=p->next;
        }
        if(p1){
            p->next = p1;
        }else{
            p->next=p2;
        }
        ListNode* result = newlist->next;
        return result;
    }
};
```

![image-20250905173107827](./top-100-liked.assets/image-20250905173107827.png)

#### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

思路：采用递归方式实现中序遍历，按照左子树 、根节点 、右子树（中序遍历规则）访问节点，通过**引用传递**节点的值，直到为空节点。

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
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> reslut;
        traversal(root,reslut);
        return reslut;
    }
    void traversal(TreeNode* node,vector<int>& val){
        if(node==nullptr)
        return;
        traversal(node->left,val);
        val.push_back(node->val);
        traversal(node->right,val);
    }
};
```

![image-20250906203945300](./top-100-liked.assets/image-20250906203945300.png)

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

思路：使用深度优先搜索计算二叉树最大深度。以空节点深度为 0 作为终止条件，递归计算当前节点左右子树的最大深度，取两者中的最大值加 1（计入当前节点），即为当前节点所在子树的最大深度。从**叶子节点**逐层向上叠加，最终得到整棵树的最大深度。

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
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr)
        return 0;
        int leftDepth = maxDepth(root->left);
        int rightDepth = maxDepth(root->right);
        return max(leftDepth,rightDepth)+1;
    }
};
```

![image-20250906210653308](./top-100-liked.assets/image-20250906210653308.png)

#### [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

思路：采用**递归**思路，先判断当前节点是否为空，若为空则直接返回；否则递归翻转当前节点的左子树和右子树，然后交换当前节点的左右子树指针，最终返回处理后的当前节点，从而实现整棵二叉树的翻转。

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
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr){
            return nullptr;
        }
        TreeNode* left = invertTree(root->left);
        TreeNode* right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
```

![image-20250907161532588](./top-100-liked.assets/image-20250907161532588.png)

#### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

思路：还是通过递归，先检查根节点是否为空（空树视为对称），否则调用symmetric函数比较左右子树；辅助函数先判断两节点是否都为空（对称）或仅有一个为空（不对称），再比较节点值是否相等，最后递归比较左节点的左子树与右节点的右子树、左节点的右子树与右节点的左子树，只有所有对应位置都满足条件才返回对称。

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
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr)
        return true;
        return symmetric(root->left,root->right);
    }
    bool symmetric(TreeNode* left,TreeNode* right){
        if(!left && !right)
        return true;
        if(!left || !right)
        return false;
        if(left->val != right->val){
            return false;
        }
        return symmetric(left->left,right->right)&&symmetric(left->right,right->left);
    }
};
```

![image-20250907163254849](./top-100-liked.assets/image-20250907163254849.png)

#### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)

思路：遍历所有节点，算出每个节点的 "左段长度 + 右段长度"，其中最大的那个值就是整个树的直径。通过递归的方式，一边计算每个节点往下能伸多远（深度），一边把左右两段加起来和当前最大直径比一比，更新最大的那个值。

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
    int maxDiameter = 0;
    int depth(TreeNode* root) {
        if(root == nullptr) return 0;
        
        int leftDepth = depth(root->left);
        int rightDepth = depth(root->right);
        maxDiameter = max(maxDiameter, leftDepth + rightDepth);
        return max(leftDepth, rightDepth) + 1;
    }
    
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root == nullptr) return 0;
        depth(root);
        return maxDiameter;
    }
};
```

![image-20250908181201727](./top-100-liked.assets/image-20250908181201727.png)

#### [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

思路：以**二分法**选取数组中间元素作为当前子树的根节点（保证左右子树平衡），再**递归**地用同样方法将中间元素左侧数组构建为左子树、右侧数组构建为右子树，最终形成满足 “左小右大” 特性且左右高度差不超过 1 的平衡二叉搜索树。

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
public:
    TreeNode* traversal(vector<int>& nums, int left, int right) {
        if (left > right) {
            return nullptr;
        }
        int mid = left + (right - left)/2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = traversal(nums, left, mid - 1);
        root->right = traversal(nums, mid + 1, right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return traversal(nums, 0, nums.size() - 1);
    }
};
```

![image-20250908195547418](./top-100-liked.assets/image-20250908195547418.png)

#### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

思路：通过二分查找：先用左右指针分别指向数组的起始和末尾，然后在左右指针未交叉（left ≤ right）的循环中，不断计算中间位置 mid，将数组中间元素与目标值对比： 若中间元素小于目标值，说明目标值在 mid 右侧，将 left 移至 mid+1；若中间元素大于等于目标值，说明目标值在 mid 左侧或就是 mid，将 right 移至 mid-1。当循环结束时，left 指针恰好停在**第一个大于等于目标值的位置**，这个位置就是目标值插入后能保持数组有序的正确位置，直接返回 left 即可。

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
    int left=0,right=nums.size()-1;
    while(left<=right){
        int mid=(left+right)/2;
        if(nums[mid]<target){
            left=mid+1;
        }else{
            right=mid-1;
        }
    }
    return left;
    }
};
```

![image-20250909200447462](./top-100-liked.assets/image-20250909200447462.png)

#### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

思路：用栈来匹配括号：遍历字符串时，遇到左括号（'(', '[', '{'）就暂时存到栈里；遇到右括号时，就检查栈顶是否有对应的左括号（比如 ')' 对应 '(', '}' 对应 '{'），如果没有对应左括号或栈为空（没左括号可匹配），就说明无效；如果匹配上了，就把栈顶的左括号移除。最后如果栈空了，说明所有括号都正确配对，返回有效；否则无效。

```c++
class Solution {
public:
    bool isValid(string s) {
      stack<int> p;
      for (int i = 0; i < s.size(); i++) {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{') p.push(i);
        else {
          if (p.empty()) return false;
          if (s[i] == ')' && s[p.top()] != '(') return false;
          if (s[i] == '}' && s[p.top()] != '{') return false;
          if (s[i] == ']' && s[p.top()] != '[') return false;
          p.pop();
        }
      }
      return p.empty();
    }
};
```

![image-20250909201414018](./top-100-liked.assets/image-20250909201414018.png)

#### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

思路：cost 为初始买入成本、 profit 为0，即无交易利润。遍历股价时，用 min 更新最低成本，用 max 计算当前价与最低价的差值，不断刷遍历新最大利润，最终返回结果。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int cost = prices[0];
        int profit = 0;
        for (int price : prices) {
            cost = min(cost, price);
            profit = max(profit, price - cost);
        }
        return profit;
    }
};
```

![image-20250910225255939](./top-100-liked.assets/image-20250910225255939.png)

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

思路：因为每次只能跨1阶或2阶，要到第n级台阶，最后一步要么是从第n-1级跨1阶，要么是从第n-2级跨2阶。 比如已知n=3时有3种跳法（1+1+1、1+2、2+1），n=4时有5种跳法（1+1+1+1、1+2+1、2+1+1、1+1+2、2+2）；求n=5的跳法时，只需给n=4的5种跳法末尾都加1阶（对应最后跨1阶的情况），给n=3的3种跳法末尾都加2阶（对应最后跨2阶的情况），总共5+3=8种，即f(5)=f(4)+f(3)=8，完全符合所以f(n)=f(n-1)+f(n-2)。

通过循环n-1次，不断更新 a 和 b （ a 代表前2级的方法数， b 代表前1级的方法数），最终 b 即为爬到第n级的总方法数。

```c++
class Solution {
public:
    int climbStairs(int n) {
        int a = 1, b = 1, sum;
        for(int i = 0; i < n - 1; i++){
            sum = a + b;
            a = b;
            b = sum;
        }
        return b;
    }
};
```

![image-20250910225504153](./top-100-liked.assets/image-20250910225504153.png)

#### [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

思路：杨辉三角的性质是每个数等于它左上方和右上方的数之和。**vector<vector<int>> result(numRows)**;创建一个名为result的二维向量，初始化为包含 `numRows` 行。每行初始化为长度等于行数 + 1 且元素全为 1（直接确定首尾的 1）`resize(i + 1, 1)` ，再通过循环计算每行中间元素，其值为上一行对应位置左上方与正上方元素之和。

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result(numRows);
        for (int i = 0; i < numRows; i++) {
            result[i].resize(i + 1, 1);
            for (int j = 1; j < i; j++) {
                result[i][j] = result[i - 1][j - 1] + result[i - 1][j];
            }
        }
        return result;
    }
};
```

![image-20250911193044258](./top-100-liked.assets/image-20250911193044258.png)

#### [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

思路：题目要求时间复杂度 *O*(*N*) ，空间复杂度 *O*(1) ，因此不能用暴力法。使用哈希表解题，定义`unordered_map<int, int>`类型的哈希表map，键为数组元素，值为该元素出现的次数。第一个循环遍历数组nums，对每个元素执行map[num]++，完成所有元素出现次数的统计（出现两次的元素值会被记为 2，只出现一次的记为 1）。第二个循环再次遍历数组，对每个元素num检查其在哈希表中的次数，若map[num] == 1，则该元素即为目标，直接返回。

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int,int> map;
        for(int num:nums)
            map[num]++;
        for(int num:nums)
            if(map[num] == 1) return num;
        return 0;
    }
};
```

![image-20250911194932693](./top-100-liked.assets/image-20250911194932693.png)

#### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

思路：对于输入的每个字符串，先对其字符进行排序（互为字母异位词的字符串排序后会完全相同），以排序后的字符串作为键，将原字符串存入该键对应的列表中；遍历所有字符串完成分组后，收集映射中所有列表，即为字母异位词的分组结果。

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        
        map<string, vector<string>> m;
        for (int i = 0; i < strs.size(); i++) {
            string data = strs[i];
            sort(data.begin(), data.end());
            m[data].push_back(strs[i]);
        }
        vector<vector<string>> ret;
        for (map<string, vector<string>>::iterator it = m.begin();
             it!= m.end(); it++) {
            ret.push_back(it->second);
        }
        return ret;
    }
};
```

![image-20250912235238010](./top-100-liked.assets/image-20250912235238010.png)

#### [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)

思路：通过排序使连续元素相邻，再遍历统计最长连续序列长度：首先处理边界情况，若数组为空则返回 0，若只有一个元素则返回 1；之后对数组排序，使连续的元素在数组中相邻排列；接着遍历排序后的数组，通过比较当前元素与下一个元素的关系统计连续序列 —— 若下一个元素是当前元素加 1，说明连续，当前序列长度加 1 并更新最长长度；若下一个元素与当前元素不相等（排除重复元素），则当前序列中断，重置当前长度为 1；若遇到重复元素则不改变当前长度；最后返回记录的最长连续序列长度。

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int count=1,n=nums.size();
        int maxLen=1;
        if(n==0 || n==1){
            return n;
        }
        sort(nums.begin(),nums.end());
        for(size_t i=0;i<n-1;++i){          
                if(nums[i+1]==nums[i]+1){
                    count++;
                    if(count>maxLen)
                        maxLen=count;
                }else if(nums[i]!=nums[i+1])
                {
                    count=1;
                }
            }
            return maxLen;
    }
};
```

![image-20250913171639953](./top-100-liked.assets/image-20250913171639953.png)

#### [169. 多数元素](https://leetcode.cn/problems/majority-element/)

思路：用哈希表统计数组中各元素的出现次数，先计算数组长度的一半作为判断依据，遍历数组时更新每个元素的计数，一旦某元素的计数超过阈值，即确定为多数元素并返回。

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
          int mid = nums.size()/2;
        unordered_map<int,int> map;
        int n;
        for(int i = 0; i<nums.size(); i++)
        {
            map[nums[i]]++;
            if(map[nums[i]]>mid)
            {
                n  =nums[i];
            }
        }
        return n;
    }
};
```

![image-20250914225414857](./top-100-liked.assets/image-20250914225414857.png)

#### [15. 三数之和](https://leetcode.cn/problems/3sum/)

思路：立意排序+双指针，先把数组从小到大排序，这样方便后续找符合条件的三个数且避免重复。接着固定第一个数（用i遍历），如果它和前一个数一样就跳过（防止重复结果）。然后把目标值设为这个数的相反数，再用两个指针——一个从第一个数后面开始（j，找第二个数），一个从数组末尾开始（k，找第三个数）。找第二个数时，若它和前一个数一样也跳过（避免重复），之后调整k的位置：只要j在k左边且两数相加比目标值大，就把k往左移（让和变小）。如果j和k碰面了，说明当前i下没符合条件的，就换下一个i；要是两数相加刚好等于目标值，就把这三个数（i、j、k对应的数）存起来，最后遍历完所有情况，得到的就是所有不重复的、和为0的三元组。

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
         vector<vector<int>> ans;
        sort(nums.begin(),nums.end());
        int n = nums.size();
        int k  = 0,target = 0;
        for(int i = 0;i < n ;i++)
        {
            if( i > 0 && nums[i] == nums[i-1])
            {
                 continue;
            }
            k = n - 1;//指向第三个数的下标
            target = -nums[i];
            for(int j= i+1;j < n;j++)
            {
                if( j > i + 1 && nums[j] == nums[j-1] )
                {
                    continue;
                }
                while( j < k && nums[j] + nums[k] > target)
                {
                    k--;
                }
                if(j == k)
                {
                    break;
                }
               if(nums[j] + nums[k] == target)
               {
                   ans.push_back({nums[i],nums[j],nums[k]});
               }
            }
        }
        return ans;
    }
};
```

![image-20250915210657047](top-100-liked.assets/image-20250915210657047.png)

#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

思路：双指针：用左右指针分别位于数组两端，通过计算当前指针形成的容器水量（由两指针处高度的最小值与间距决定）并不断在比较后更新最大值，随后始终移动较矮边界的指针（因移动较高边界无法增大水量），最终找到最大水量。

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;                   
        int right = height.size() - 1;  
        int max_water = 0;     
        while (left < right) {
            int current_water = min(height[left], height[right]) * (right - left);

            if (current_water > max_water) {
                max_water = current_water;
            }

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }
        return max_water;
    }
};
```

![image-20250916232741098](./top-100-liked.assets/image-20250916232741098.png)

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

思路：通过两层for循环可以实现暴力破解寻找无重复字符串，最外层循环，作为每次生成子字符串的头节点；第二层循环，每次从i位置开始，拼装子字符串，用哈希表记录当前子串中的字符以检查是否重复，一旦遇到重复就停止，同时记录当前子串的长度并更新全局最长长度，最终返回找到的最大长度。

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int result = 0;
        int temp = 0;
        for (int i = 0; i < s.length(); i++){
            unordered_map<char, int> map;
            temp = 0;
            for (int j = i; j < s.length(); j++) {
                if (map.find(s[j]) != map.end()) {
                    break;
                } else {
                    map[s[j]] = j;
                    temp++;
                    result = max(result, temp);
                }
            }
        }
        return result;
    }
};
```

![image-20250918001248403](./top-100-liked.assets/image-20250918001248403.png)

#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

思路：

**findAnagrams 函数**

初始mp1记录p中每个字符的出现次数（固定不变）。mp2：记录s中当前滑动窗口内字符的出现次数（随窗口移动动态更新）。维护一个长度为p.size()的窗口，每次滑动调用`check`函数判断当前窗口是否为p的异位词，若是则记录`left`（起始索引）。然后移动窗口：左边界右移`left++`，需减少mp2中对应字符的频率（若频率变为 0 则从mp2中删除，避免干扰后续判断）；右边界右移`right++`，需增加`mp2`中对应新字符的频率。遍历结束后，所有记录的left即为s中所有p的异位词的起始索引，最终返回这些索引组成的数组ret。

**check 函数**

异位词的本质是 “字符种类和每种字符的出现次数完全相同”，因此用两个map（mp1和mp2）分别存储p和s中当前窗口的字符频率。check函数两点验证是否为异位词：

- 若两个map的大小不同（字符种类不同），直接返回false；

- 遍历mp1中的每个字符，检查mp2中是否存在该字符且频率相同，若有任何不匹配则返回false，否则返回true。

  

```c++
class Solution {
public:
    bool check(map<int,int>& mp1,map<int,int>& mp2)
    {
        if (mp1.size() != mp2.size()) 
            return false;
 
        for (const auto pair : mp1) 
        {
            auto it = mp2.find(pair.first);
            if (it == mp2.end() || it->second != pair.second) 
                return false;
        }
        return true;
    }
 
    vector<int> findAnagrams(string s, string p)
    {
        vector<int> ret;
        map<int,int> mp1,mp2;
        for (int i=0;i<p.size();i++)
        {
            mp1[p[i]]++;
            mp2[s[i]]++;
        }
            
        for (int left=0,right=p.size()-1; right<s.size();left++,right++)
        {
            if (check(mp1,mp2))
            {
                ret.push_back(left);
            }
            mp2[s[left]]--;
            mp2[s[right+1]]++;
            if (mp2[s[left]]==0)
                mp2.erase(s[left]);
        }
        return ret;
    }
};
```

![image-20250919000147122](./top-100-liked.assets/image-20250919000147122.png)

#### [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

思路：利用暴力枚举法，外层循环固定子数组的起始索引left，内层循环从 left 开始遍历结束索引 right，在遍历过程中实时累加从nums [left] 到 nums [right]的元素和 sum，每当sum等于k时，就将count加1，最终返回count（所有符合条件的连续子数组的总数）

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;
        int len = nums.size();
        for (int left = 0; left < len; left++) {
            int sum = 0;
            for (int right = left; right < len; right++) {
                sum += nums[right];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
};
```

![image-20250920003203786](./top-100-liked.assets/image-20250920003203786.png)

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

思路：通过一次线性遍历，动态维护 “以当前元素结尾的子数组最大和” 与 “全局最大子数组和”。初始化时，两者均设为数组首个元素；遍历后续元素时，若前者为非正数（说明之前的子数组无增益），则从当前元素重新开始计算局部和，否则将当前元素加入局部和；每次更新局部和后，都与全局最大值比较并更新，确保全局最大值始终记录已遍历部分的最优解。

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0];
        int maxnum = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            if (maxnum <= 0) {
                maxnum = nums[i];
            } else if (maxnum > 0) {
                maxnum += nums[i];
            }
            res = max(maxnum, res);
        }
        return res;
    }
};
```

![image-20250921234822553](./top-100-liked.assets/image-20250921234822553.png)

#### [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)

思路：通过三次反转操作来实现数组的旋转。给定一个整数数组 nums 和一个整数 k，将数组向右旋转 k 个位置。旋转后，数组的最后 k 个元素会移动到数组的开头，其余元素依次向后移动。首先反转整个数组，使得数组的顺序完全颠倒。再反转前 k 个元素，使得它们恢复到正确的顺序。最后反转剩下的 n - k 个元素，使得它们恢复到正确的顺序

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {        
        int n = nums.size();
        k %= n;
        reverse(nums,0, n - 1);
        reverse(nums,0, k - 1);
        reverse(nums,k, n - 1);
    }
private:
    void reverse(vector<int>& nums, int i, int j) {
        for (; i < j; i++, j--) {
            swap(nums[i], nums[j]);
        }
    }
};
```

![image-20250922233424548](./top-100-liked.assets/image-20250922233424548.png)

#### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

思路：对所有区间按照「起始值」进行升序排序。排序后，区间的起始值呈现递增趋势，极大简化了重叠判断逻辑。初始化一个结果集合存储合并后的区间，然后逐个遍历排序后的区间，当前区间 [L,R] 与结果集最后区间比较，结果集空或 L > 最后区间结束值（无重叠），直接加入，否则（有重叠），合并（结束值取两者最大）。遍历完成后，结果集合中即为所有合并后的不重叠区间。

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
         sort(intervals.begin(), intervals.end());
        vector<vector<int>> merged;
        for (int i = 0; i < intervals.size(); ++i) {
            int L = intervals[i][0], R = intervals[i][1];
            if (!merged.size() || merged.back()[1] < L) {
                merged.push_back({L, R});
            }
            else {
                merged.back()[1] = max(merged.back()[1], R);
            }
        }
        return merged;
    }
};
```

![image-20250924222536761](./top-100-liked.assets/image-20250924222536761.png)

#### [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)

思路：构建两个辅助数组left和right求解：left[i]存储索引i左侧所有元素的乘积，right[i]存储索引i右侧所有元素的乘积；先从左到右遍历计算left数组，再从右到左遍历计算right数组，最后每个位置i的结果answer[i]即为其左侧所有元素乘积（left[i-1]）与右侧所有元素乘积（right[i+1]）的乘积。

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
          int n = nums.size();
        vector<int> answer(n);
        vector<int> left(n);
        vector<int> right(n);
        left[0] = nums[0];
        right[n-1] = nums[n-1];
        for (int i = 1; i < n; i++) {
            left[i] = left[i-1] * nums[i];
        }
        answer[n-1] = left[n-2];
        for (int i = n-2; i >= 0; i--) {
            right[i] = right[i+1] * nums[i];
            if (i == 0) {
                answer[i] = right[i+1];
                continue;
            }
            answer[i] = left[i-1] * right[i+1];
        }
        return answer;
    }
};
```

![image-20250926232023409](./top-100-liked.assets/image-20250926232023409.png)

#### [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

思路： 使用两个标记数组去记录初始时0元素的位置，首先创建两个标记数组 `row_mark`（长度为矩阵行数 `m`）和 `column_mark`（长度为矩阵列数 `n`），用于分别记录原始矩阵中存在 0 的行和列；通过第一次遍历矩阵，若遇到元素 `matrix[i][j] == 0`，就将 `row_mark[i]` 和 `column_mark[j]` 设为 1，以此精准标记出所有需要置零的行与列；随后进行第二次遍历，对矩阵中的每个元素 `matrix[i][j]`，只要其所在行被标记（`row_mark[i] == 1`）或所在列被标记（`column_mark[j] == 1`），就将该元素设为 0，最终完成矩阵置零。

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
         int m = matrix.size();
        int n = matrix[0].size();
        vector<int> row_mark(m);
        vector<int> column_mark(n);
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (matrix[i][j] == 0)
                {
                    row_mark[i] = 1;
                    column_mark[j] = 1;
                }
            }
        }
         for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (row_mark[i] == 1 || column_mark[j] == 1)
                {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

![image-20250928000002665](./top-100-liked.assets/image-20250928000002665.png)

#### [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

思路：
初始化：用 `left` 表示左边界（初始为第 0 列），`right` 表示右边界（初始为最后一列），`top` 表示上边界（初始为第 0 行），`bottom` 表示下边界（初始为最后一行）。
循环打印： “从左向右、从上向下、从右向左、从下向上” 四个方向循环打印。
根据边界打印，即将元素按顺序添加至列表 `result`尾部。
边界向内收缩 1 （代表已被打印）。
每步遍历后通过判断边界是否交叉（如 `top > bottom`、`right < left` 等），决定是否终止循环，若打印完毕则跳出。
返回值： 返回 result 即可。

```c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int left = 0;
        int right = matrix[0].size() - 1;
        int top = 0;
        int bottom = matrix.size() - 1; 
        vector<int> result;  
        while (true) {
            for (int col = left; col <= right; ++col) {
                result.push_back(matrix[top][col]);
            }
            if (++top > bottom) {
                break;
            }
            for (int row = top; row <= bottom; ++row) {
                result.push_back(matrix[row][right]);
            }
            if (--right < left) {
                break;
            }
            for (int col = right; col >= left; --col) {
                result.push_back(matrix[bottom][col]);
            }
            if (--bottom < top) {
                break;
            }
            for (int row = bottom; row >= top; --row) {
                result.push_back(matrix[row][left]);
            }
            if (++left > right) {
                break;
            }
        }
       return result;
    }
};
```

![image-20250930234157024](./top-100-liked.assets/image-20250930234157024.png)

#### [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)

思路：

1. **转置矩阵**：交换矩阵的行和列（`mat[i][j] <-> mat[j][i]`）。
2. **翻转每一行**：对矩阵的每一行元素进行逆序反转（从左到右的元素变为从右到左)，利用`reverse` 函数（或手动交换）。

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
         for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
        for (int i = 0; i < n; i++) {
            reverse(matrix[i].begin(), matrix[i].end());
        }
    }
};
```

![image-20251003185925207](./top-100-liked.assets/image-20251003185925207.png)

#### [240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)

思路：先获取矩阵的行数和列数，然后逐行遍历矩阵；对每一行，利用二分查找法（定义左右边界，通过计算中间位置并与目标值比较，动态调整边界范围）检查该行是否包含目标值，若找到则立即返回 true；若遍历完所有行仍未找到目标值，则返回 false。

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        for(int i=0; i<m; ++i){
            int left = 0, right = n-1;
            while(left <= right){
                int mid = left + (right - left) / 2;
                if(matrix[i][mid] == target) return true;   
                else if(matrix[i][mid] < target) left = mid + 1;
                else right = mid - 1;
            }
        }
        return false;
    }
};
```

![image-20251005235351261](./top-100-liked.assets/image-20251005235351261.png)

#### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

思路：双指针法，两次相遇定位环入口：

1. **第一次相遇：检测是否有环**
   - 初始化快慢指针 `fast` 和 `slow` 都指向头节点，`fast` 每次走 2 步，`slow` 每次走 1 步。
   - 若 `fast` 走到链表末尾（`fast` 或 `fast->next` 为 `nullptr`），说明无环，返回 `nullptr`。
   - 若 `fast` 与 `slow` 相遇，说明有环，进入下一步。
2. **第二次相遇：定位环的入口**
   - 将 `fast` 重新指向头节点，`slow` 留在相遇点。
   - 两指针同时每次走 1 步，当它们再次相遇时，该节点就是环的入口，返回此节点。

原理：第一次相遇时，`slow` 已走了 `n` 个环长，`fast` 走了 `2n` 个环长；第二次同速移动时，`fast` 从头走 `a` 步到入口，`slow` 从相遇点走 `a` 步也到入口（因 `a + n*环长` 是入口位置），故再次相遇点即入口。

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
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (true) {
            if (fast == nullptr || fast->next == nullptr) return nullptr;
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) break;
        }
        fast = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }
        return fast;
    }
};
```

![image-20251007215502955](./top-100-liked.assets/image-20251007215502955.png)

#### [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

思路：利用逆序链表（个位在前）的特点，模拟手动加法过程：  

1. 用哑节点简化结果链表的头节点处理，用变量记录进位；   
2. 同步遍历两个链表，逐位取对应节点值（空节点取0），计算“两值+进位”的总和；   
3. 总和的个位数作为当前位结果，更新进位（sum/10），并构建新节点；   
4. 遍历至两链表结束且无进位，返回哑节点的下一个节点（结果链表头）。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0); // 哑节点
        ListNode* current = dummy;
        int carry = 0;
        while (l1 != nullptr || l2 != nullptr || carry != 0) {
            int x = (l1 != nullptr) ? l1->val : 0;
            int y = (l2 != nullptr) ? l2->val : 0;
            
            int sum = x + y + carry;
            carry = sum / 10; // 更新进位
            current->next = new ListNode(sum % 10); // sum的个位数
            current = current->next; 

            if (l1 != nullptr) l1 = l1->next;
            if (l2 != nullptr) l2 = l2->next;
        }
        
        return dummy->next;
    }
};
```

![image-20251009233857661](./top-100-liked.assets/image-20251009233857661.png)

#### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

思路：通过计算链表长度确定待删除节点的位置，再执行删除操作。先创建值为 0 且 next 指向原链表头节点的虚拟头节点 dummy，接着遍历原链表统计总长度 length，然后让 cur 指针从 dummy 出发，向后移动 length-n 次以定位到待删除节点的前驱节点，随后通过 cur->next = cur->next->next 跳过待删除节点完成删除操作，最后保存 dummy->next 作为新链表头节点并释放 dummy 节点，最终返回新头节点。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0,head);
        int length = 0;
        while(head){
            ++length;
            head = head->next;
        }
        ListNode* cur = dummy;
        for(int i=0;i < length-n;i++){
            cur = cur->next;
        }
        cur->next = cur->next->next;
        ListNode* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```

![image-20251012175541182](./top-100-liked.assets/image-20251012175541182.png)

#### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

思路：用递归思路实现链表节点的两两交换，核心是将问题拆解为重复的子问题：首先判断终止条件 —— 若链表为空或仅有一个节点，无需交换，直接返回；否则，将当前头两个节点视为一对，原第二个节点成为新头节点（newHead），通过递归处理这对节点之后的剩余链表，再将原第一个节点的 next 指向递归处理后的子链表，新头节点的 next 指向原第一个节点，完成当前对的交换，最终返回新头节点。如此逐层递归，从局部到整体完成整个链表的两两交换。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode* newHead = head->next;
        head->next = swapPairs(newHead->next);
        newHead->next = head;
        return newHead;
    }
};
```

![image-20251013183519500](./top-100-liked.assets/image-20251013183519500.png)

[138. 随机链表的复制](https://leetcode.cn/problems/copy-list-with-random-pointer/)

思路：通过哈希表实现带随机指针链表的复制，步骤清晰：首先遍历原链表，为每个节点创建值相同的新节点，并将原节点与新节点的对应关系存入哈希表；接着再次遍历原链表，借助哈希表快速找到每个新节点对应的 next 和 random 指针所指的新节点，完成新链表指针关系的构建；最后返回原链表头节点在哈希表中对应的新节点，即为复制链表的头节点。

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        Node* cur = head;
        unordered_map<Node*,Node*> map;
        while(cur != nullptr){
            map[cur] = new Node(cur->val);
            cur = cur->next;
        }
        cur = head;
        while(cur != nullptr){
            map[cur]->next = map[cur->next];
            map[cur]->random = map[cur->random];
            cur = cur->next;
        }
        return map[head];
    }
};
```

![image-20251014224020905](./top-100-liked.assets/image-20251014224020905.png)

#### [148. 排序链表](https://leetcode.cn/problems/sort-list/)

思路：：首先通过快慢慢指针找到链表中点，将链表分割为左右两个子链表；然后递归地对两个子链表进行排序，直到子链表长度为 1；最后通过合并函数将两个已排序的子链表按值从小到大合并，最终得到完整的排序链表。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }

    ListNode* mergeSort(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* mid = findMid(head);
        ListNode* l1 = head;
        ListNode* l2 = mid->next;
        mid->next = nullptr;
        l1 = mergeSort(l1);
        l2 = mergeSort(l2);
        return merge(l1, l2);
    }

    ListNode* findMid(ListNode* head) {
        ListNode *slow = head, *fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }

    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead = new ListNode();
        ListNode* cur = dummyHead;
        while (l1 && l2) {
            if (l1->val <= l2->val) {
                cur->next = l1;
                l1 = l1->next;
            }
            else {
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        cur->next = l1 ? l1 : l2;
        return dummyHead->next;
    }
};
```

![image-20251019141316267](./top-100-liked.assets/image-20251019141316267.png)
