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
