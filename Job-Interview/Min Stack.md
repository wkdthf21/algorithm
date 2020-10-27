### Link
---
https://leetcode.com/explore/interview/card/top-interview-questions-easy/98/design/562/


### Explanation
---

- stack 기능 + 최소값 얻을 수 있는 기능
- stack은 LIFO인 특성을 이용해봤을 때
- 현재 stack의 top에 저장된 min 값과 최근에 들어온 min값을 비교해서
- 노드마다 min 값을 저장해두면
- stack의 top에 저장된 min값이 그 stack에서 가장 min값이다.

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
