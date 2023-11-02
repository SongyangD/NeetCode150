# Stack 

## 20. Valid Parentheses - 1/7
The parentheses are valid 

### Learned 
Two cases: 
- if it is a left bracket
- if it is a right bracket

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        Map<Character, Character> map = new HashMap<>();
        map.put('(', ')');
        map.put('{', '}');
        map.put('[', ']');
        for (char c: s.toCharArray()){
            if (c == '(' || c == '{' || c == '['){
                stack.push(c);
            }else if(stack.isEmpty() || map.get(stack.peek()) != c){
                return false;
            }else{
                stack.pop();
            }
        }
        return stack.isEmpty() ? true : false;
    }
}
```
## 150. Evaluate Reverse Polish Notation - 2/7
Simple calculator

### Learned 
If the cur is a operator, get the previous two and calculate 
If the cur is a num, push into the stack directly

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();
        for (String s: tokens){
            if (s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/")){
                int num2 = stack.pop();
                int num1 = stack.pop();
                if (s.equals("+")){
                    stack.push(num1+num2);
                    System.out.println(num1+num2);
                }else if (s.equals("-")){
                    stack.push(num1-num2);
                }else if (s.equals("*")){
                    stack.push(num1*num2);
                }else{
                    stack.push(num1/num2);
                }
            }else{
                int n = Integer.valueOf(s);
                stack.push(n);
            }
        }
        return stack.pop();
    }
}
```

## 22. Generate Parentheses - 3/7
Given a number of pairs of the parentheses, generate all valid combination as result

### Learned 
Backtracking, we can use StringBuilder in Java as temp container

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        backtracking(res, sb, n, 0, 0);
        return res;
    }
    private void backtracking(List<String> res, StringBuilder sb, int n, int left, int right){
        if (sb.length() == n*2){
            res.add(sb.toString());
            // System.out.println("sb: "+ sb);
            return;
        } 
        if (left == right){ // must left
            sb.append('(');
            backtracking(res, sb, n, left+1, right);
            sb.deleteCharAt(sb.length() -1);
        }else if (left == n){
            sb.append(')');
            backtracking(res, sb, n, left, right+1);
            sb.deleteCharAt(sb.length() -1);
        }else{
            sb.append('(');
            backtracking(res, sb, n, left+1, right);
            sb.deleteCharAt(sb.length() -1);

            sb.append(')');
            backtracking(res, sb, n, left, right+1);
            sb.deleteCharAt(sb.length() -1);
        }       
    }
}
```

## 155. Min Stack - 4/7
Implement a stack with min in O(1) time

### Learned
Use two list, one for stack, and one for prefix min
- It's hard to use idx in this question, directly use size of the list to find the top of the stack

```java
class MinStack {
        List<Integer> stack;
        List<Integer> min;
    public MinStack() {
        this.stack = new ArrayList<>();
        this.min = new ArrayList<>();
    }
    
    public void push(int val) {
        int curMin;
        if (min.size() != 0){
            curMin = Math.min(min.get(min.size() -1), val);
        }else{
            curMin = val;
        }
        stack.add(val);
        min.add(curMin);
    }
    
    public void pop() {
        stack.remove(stack.size()-1);
        min.remove(min.size()-1);
    }
    
    public int top() {
        return stack.get(stack.size()-1);
    }
    
    public int getMin() {
        return min.get(min.size()-1);
    }
}
```
## 39. Daily Temperatures - 5/7
Monotonically stack

### Learned 
Put the idx in the stack, attention when pop from the stack

```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] res = new int[n];
   
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n; i++){
            int temp = temperatures[i];
            while (!stack.isEmpty() && temperatures[stack.peek()] < temp){
                int idx = stack.pop();
                res[idx] = i-idx;
            }
            stack.push(i);
        }
        return res;
    }
}
```
## 853. Car Fleet - 6/7
Monotonically decreasing stack

### Learned
(double) should be used in the division to make sure to keep the decimmal 

```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < position.length; i++){
            map.put(position[i], speed[i]);
        }
        Arrays.sort(position);
        Stack<Double> stack = new Stack<>();
        for (int i = 0; i < position.length; i++){
            double time = ((double)(target - position[i])) / map.get(position[i]);
            while (!stack.isEmpty() && stack.peek() <= time){
                double pop = stack.pop();
                // System.out.println("pop: "+ pop);
            }
            stack.push(time);
        }
        return stack.size();
    }
}
```
Improved version: because the target is the ceiling of number, we can use an array instead of map. Also when we look backforward, it should be a ascending res, only count the number that is bigger than the previous one.
```java
class Solution {
    public int carFleet(int target, int[] position, int[] speed) {
        double[] time = new double[target];
        for (int i = 0; i < position.length; i++){
            time[position[i]] = (double) (target - position[i])/speed[i];
        }
        double pre = 0.0;
        int res = 0;
        for (int i = target-1; i >= 0; i--){
            if (time[i] > pre){
                pre = time[i];
                res ++;
            }
        }
        return res;
    }
}
```

## 84. Largest Rectangle in Histogram - 7/7



