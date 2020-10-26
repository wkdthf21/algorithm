### Link
---
https://leetcode.com/explore/interview/card/top-interview-questions-easy/98/design/562/

<br>

```java
import java.util.*;

class MinStack {

    private Stack<Node> stack;

    private static class Node{

        int data;
        int MIN;

        public Node(int data, int MIN){
            this.data = data;
            this.MIN = MIN;
        }

    }

    /** initialize your data structure here. */
    public MinStack() {
        stack = new Stack<>();
    }

    public void push(int x) {
        if(stack.size() == 0){
            Node node = new Node(x, x);
            stack.push(node);
        }
        else{
            int prevMin = stack.peek().MIN;
            int curMin = (x < prevMin) ? x : prevMin;
            Node node = new Node(x, curMin);
            stack.push(node);
        }

    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek().data;
    }

    public int getMin() {
        return stack.peek().MIN;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```
