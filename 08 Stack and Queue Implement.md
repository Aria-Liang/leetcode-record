# Day9 Queue and Stack 01

##  Basic Knowledge about Stack
- Stack/Queue instantiation:
```
Stack<Integer> st = new Stack<Integer>();
Queue<String> queue = new LinkedList<String>();
```

- Stack method: 
```
st.push(new Integer(a));
Integer a = st.pop();
st.empty(); //判断是否为空
st.peek(); //取栈顶端（不出栈）
```
```
queue.offer("a");
queue.poll();
queue.peek();
queue.isEmpty();
for (String q: queue){
    System.out.println(q);
```

## Related Questions:
### [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)
```
class MyStack {
    public Queue<Integer> queue1;
    public Queue<Integer> queue2;
    
    public MyStack() {
        queue1 = new LinkedList<>();
        queue2 = new LinkedList<>();
    }
    
    public void push(int x) {
        if (!queue2.isEmpty()){
            queue2.offer(x);
        }
        else{
            queue1.offer(x);
        }
    }
    
    public int pop() {
        if (!queue2.isEmpty()){
            while (queue2.size() != 1){
                queue1.offer(queue2.poll());
            }
            Integer element = queue2.poll();
            return element;
        }
        else if (!queue1.isEmpty()){
            while (queue1.size() != 1){
                queue2.offer(queue1.poll());
            }
            Integer element = queue1.poll();
            return element;
        }
        return -1;
    }
    
    public int top() {
        Integer output = pop();
        if (!queue2.isEmpty()){
            queue2.offer(output);
        }
        else{
            queue1.offer(output);
        }
        return output;
    }
    
    public boolean empty() {
        return queue1.isEmpty() && queue2.isEmpty();
    }
}
```
#### 可以优化，用一个queue来解决,在push一个元素来的同时就把顺序变成我们想要的顺序
#### 比如我们输入的顺序首先是1， 它会加入queue  <-【1】<-;
#### 接着是2  <-【1,2】<-,  我们把1 poll出来offer到2后面，变成了   <-【2,1】<-
#### 接着是3   <-【2，1,3】<-  我们可以把2,1看成一个整体，因为如果对2,1一次做poll(), 再offer()到queue中，顺序依然是2,1，所以以此类推把前面排好序的queue当做一个整体，统一先poll()再offer到queue中，就可以实现了

```
class MyStack {
    public Queue<Integer> queue1;

    public MyStack() {
        queue1 = new LinkedList<>();
    }
    
    public void push(int x) {
        queue1.offer(x);
        int size = queue1.size();
        for (int i = 0; i < size - 1; i++){
            queue1.offer(queue1.poll());
        }
    }
    
    public int pop() {
        return queue1.poll();
    }
    
    public int top() {
        return queue1.peek();
    }
    
    public boolean empty() {
        return queue1.isEmpty();
    }
}
```

[232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)
通过思考得知，如果只有一个stack的话没办法实现
```
class MyQueue {
    public Stack<Integer> stack1;
    public Stack<Integer> stack2;

    public MyQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }
    
    public void push(int x) {
        stack1.push(x);
    }
    
    public int pop() {
        if (!stack2.empty()){
            return stack2.pop();
        }
        else{
            while(!stack1.empty()){
                stack2.push(stack1.pop());
            }
            return stack2.pop();
        }
    }
    
    public int peek() {
        if (!stack2.empty()){
            return stack2.peek();
        }
        else{
            while(!stack1.empty()){
                stack2.push(stack1.pop());
            }
            return stack2.peek();
        }
    }
    
    public boolean empty() {
        return stack1.empty() && stack2.empty();
    }
}
```
思路基本正确，但是pop() 和 peek()两边代码有重复，而且stack1和stack2命名有点随意，其实q1是负责进stack, q2是负责所有的出stack,改进如下：
```
class MyQueue {

    Stack<Integer> stackIn;
    Stack<Integer> stackOut;

    /** Initialize your data structure here. */
    public MyQueue() {
        stackIn = new Stack<>(); // 负责进栈
        stackOut = new Stack<>(); // 负责出栈
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        stackIn.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {    
        dumpstackIn();
        return stackOut.pop();
    }
    
    /** Get the front element. */
    public int peek() {
        dumpstackIn();
        return stackOut.peek();
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stackIn.isEmpty() && stackOut.isEmpty();
    }

    // 如果stackOut为空，那么将stackIn中的元素全部放到stackOut中
    private void dumpstackIn(){
        if (!stackOut.isEmpty()) return; 
        while (!stackIn.isEmpty()){
                stackOut.push(stackIn.pop());
        }
    }
}
```

