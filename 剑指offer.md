### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

直接看代码

```c++
class CQueue {
public:
    stack<int> instack;
    stack<int> outstack;

    CQueue() {
        
    }
    
    void appendTail(int value) {
        instack.push(value); // 直接入栈
    }
    
    int deleteHead() {
        if (instack.empty()){
            return -1; // 空队列
        }
        while (!instack.empty()){
            int tmp = instack.top();
            outstack.push(tmp);
            instack.pop();
        }
        int res = outstack.top();
        outstack.pop();
        while (!outstack.empty()){
            int tmp = outstack.top();
            instack.push(tmp);
            outstack.pop();
        }
        return res;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```

---

### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> s;
    stack<int> min_stack;
    MinStack() {

    }
    
    void push(int x) {
        if (min_stack.empty()){
            s.push(x);
            min_stack.push(x);
        }
        else{
            if ( x >= min_stack.top()){
                s.push(x);
                min_stack.push(min_stack.top());
            }
            else{
                s.push(x);
                min_stack.push(x);
            }
        }
    }
    
    void pop() {
        s.pop();
        min_stack.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int min() {
        return min_stack.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```
<<<<<<< Updated upstream

--- 

[剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```cpp
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
    vector<int> reversePrint(ListNode* head) {
        /*
            后进先出，考虑用栈解决
            注意coding细节
        */
        stack<ListNode*> s;
        ListNode* tmp = head;
        vector<int> res;
        while(tmp){
            s.push(tmp);
            tmp = tmp->next;
        }
        tmp = head;
        while(!s.empty()){
            res.push_back(s.top()->val);
            s.pop();
        }
        return res;
    }
};
```
---
### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

```cpp
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
    ListNode* reverseList(ListNode* head) {
        // 需要背
        if (head == NULL || head->next == NULL){
            return head;
        }
        ListNode* pre = NULL; // 注意1：pre为新链表的头；初始化时候置NULL
        //pre->next = head; 注意2：next指针在循环中设置
        ListNode* cur = head;
        while(cur){
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
};
```
---
### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)
> 统计一个数字在排序数组中出现的次数。
> 
> 输入: nums = [5,7,7,8,8,10], target = 8
> 
> 输出: 2

利用二分查找的思路来解，找到左右边界的下标

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = helper(nums, target, true); // left是左边界
        int right = helper(nums, target, false) - 1; // right是右边界+1，返回需要减1
        if (left <= right && right < nums.size() && nums[left] == nums[right]){
            return right - left + 1;
        }
        return 0;
    }

    int helper(vector<int>& nums, int target, bool lower){
        // lower==true, 寻找nums内第一个 >= target的下标, 左边界
        // lower==false, 寻找nums内第一个 > target的下标, 右边界
        int left = 0, right = nums.size()-1;
        int res = nums.size(); // 为了解vector内只有一个元素的情况，初始值需要设置成nums.size()
        while ( left <= right){
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)){
                right = mid-1;
                res = mid;
            }
            else{
                left = mid+1;
            }
        }
        return res;
    }
};
```
---
### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
> 一个长度为$n-1$的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围$0～n-1$之内。在范围$0～n-1$内的$n$个数字中有且只有一个数字不在该数组中，请找出这个数字。
> 
> 示例 1:
> 
> `输入: [0,1,3]`
> 
> `输出: 2`


```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int left = 0, right = nums.size()-1;
        while ( left <= right ){
            int mid = (left + right) / 2;
            if (nums[mid] == mid){
                // mid == nums[mid]，说明截止到mid的左半部分不缺数字
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        return left;
    }
};
```
---
### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

```cpp
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty() or matrix[0].empty()){
            return false;
        }
        int m = matrix.size(), n = matrix[0].size();
        int x = 0, y = n-1; // 右上角开始搜索
        while ( x >= 0 && x <= m-1 && y >= 0 && y <= n-1){
            if (matrix[x][y] == target){
                return true;
            }
            else if (matrix[x][y] > target){
                y -= 1; // 横纵坐标变化需注意，不能弄反
            }
            else{
                x += 1;
            }
        }
        return false;
=======
---
### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)
```cpp
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
        if (head == NULL){
            return NULL;
        }
        /*
            此处注掉，因为链表只有一个节点且random指向自身的时候
            单独列出会导致结果的random指向了原链表节点，不AC
        if (head->next == NULL){
            Node* res = new Node(head->val);
            res->random = head->random;
            return res;
        }
        */
        // 复制链表
        // 注意：用for替换while循环，避免bug
        for (Node* cur = head; cur != NULL ; cur = cur->next->next){
            Node* newNode = new Node(cur->val);
            newNode->next = cur->next;
            cur->next = newNode;
        }
        // 处理random指针
        for (Node* cur = head; cur != NULL ; cur = cur->next->next){
            Node* nextNode = cur->next;
            if (cur->random != NULL){
                nextNode->random = cur->random->next;
            }
            else{
                nextNode->random = NULL;
            }
        }
        // 生成新链表
        // 这里不要想复杂，就是node->next->next的操作
        Node* res = head->next;
        for (Node* cur = head; cur != NULL ; cur = cur->next){
            Node* nextNode = cur->next;
            cur->next = cur->next->next;
            if (nextNode->next != NULL){
                nextNode->next = nextNode->next->next;
            } 
            else{
                nextNode->next = NULL;
            }
        }
        return res;
>>>>>>> Stashed changes
    }
};
```