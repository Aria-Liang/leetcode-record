# Day10 Queue and Stack 02

## Related Questions:
### [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)
```
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++){
            if (s.charAt(i) == ')') {
                if (stack.size() == 0 || stack.pop() != '('){
                    return false;
                }
            }
            else if (s.charAt(i) == '}'){
                if (stack.size() == 0 || stack.pop() != '{'){
                    return false;
                }
            }
            else if (s.charAt(i) == ']'){
                if (stack.size() == 0 || stack.pop() != '['){
                    return false;
                }
            }
            else{
                stack.push(s.charAt(i));
            }
        }

        if (stack.size() != 0){
            return false;
        }
        return true;
    }
}
```
#### 我的思路比较简单，要求: 每一个open brackets must be closed by the same type of brackets, and every close bracket must has a corresponding open bracket of the same type, and open brackets must be closed in the correct order. 
#### 解法: 每遇到一个close brackets都看看有没有对应的open brackets, 有的话就pop()出来，没有就return false, 每遇到
open brackets, add it to the stack. After the for loop, let's check the size of the stack, if it is not equals to 0, then return false;

#### Another solution:
每次遇到open brackets, 把对应的close brackets 加进去，因为这表示了close brackets应该出现的顺序， 接着出现了close brackets, 就看看peek() 是不是这个close brackets， 是的话就pop出来， 最后再判断一下size of stack是不是为0；

```
class Solution {
    public boolean isValid(String s) {
        Deque<Character> deque = new LinkedList<>();
        char ch;
        for (int i = 0; i < s.length(); i++) {
            ch = s.charAt(i);
            //碰到左括号，就把相应的右括号入栈
            if (ch == '(') {
                deque.push(')');
            }else if (ch == '{') {
                deque.push('}');
            }else if (ch == '[') {
                deque.push(']');
            } else if (deque.isEmpty() || deque.peek() != ch) {
                return false;
            }else {//如果是右括号判断是否和栈顶元素匹配
                deque.pop();
            }
        }
        //最后判断栈中元素是否匹配
        return deque.isEmpty();
    }
}
```
### [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)
```
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> stack = new Stack<>();

        for (int i = 0; i < s.length(); i++){
            if (stack.empty()){
                stack.push(s.charAt(i));
            }
            else{
                if (stack.peek() == s.charAt(i)){
                    stack.pop();
                }
                else{
                    stack.push(s.charAt(i));
                }
            }
        }

        char[] charDic = new char[stack.size()];
        for (int i = charDic.length - 1; i >= 0; i--){
            charDic[i] = stack.pop();
        }

        String output = new String(charDic);
        return output;

    }
}
```
后面的把stack元素转化成string可以更优化： 
```
String str = "";
while (!stack.isEmpty()) {
      str = stack.pop() + str;
}
return str;
```
也可以用deque
```
class Solution {
    public String removeDuplicates(String S) {
        //ArrayDeque会比LinkedList在除了删除元素这一点外会快一点
        //参考：https://stackoverflow.com/questions/6163166/why-is-arraydeque-better-than-linkedlist
        ArrayDeque<Character> deque = new ArrayDeque<>();
        char ch;
        for (int i = 0; i < S.length(); i++) {
            ch = S.charAt(i);
            if (deque.isEmpty() || deque.peek() != ch) {
                deque.push(ch);
            } else {
                deque.pop();
            }
        }
        String str = "";
        //剩余的元素即为不重复的元素
        while (!deque.isEmpty()) {
            str = deque.pop() + str;
        }
        return str;
    }
}
```
也可以用string直接作为栈
```
class Solution {
    public String removeDuplicates(String s) {
        // 将 res 当做栈
        // 也可以用 StringBuilder 来修改字符串，速度更快
        // StringBuilder res = new StringBuilder();
        StringBuffer res = new StringBuffer();
        // top为 res 的长度
        int top = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            // 当 top > 0,即栈中有字符时，当前字符如果和栈中字符相等，弹出栈顶字符，同时 top--
            if (top >= 0 && res.charAt(top) == c) {
                res.deleteCharAt(top);     //delete char at
                top--;
            // 否则，将该字符 入栈，同时top++
            } else {
                res.append(c);
                top++;
            }
        }
        return res.toString();
    }
}
```

### [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
```
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stack = new LinkedList();
        for (String s : tokens) {
            if ("+".equals(s)) {        // leetcode 内置jdk的问题，不能使用==判断字符串是否相等
                stack.push(stack.pop() + stack.pop());      // 注意 - 和/ 需要特殊处理
            } else if ("-".equals(s)) {
                stack.push(-stack.pop() + stack.pop());
            } else if ("*".equals(s)) {
                stack.push(stack.pop() * stack.pop());
            } else if ("/".equals(s)) {
                int temp1 = stack.pop();
                int temp2 = stack.pop();
                stack.push(temp2 / temp1);
            } else {
                stack.push(Integer.valueOf(s));
            }
        }
        return stack.pop();
    }
}
```
