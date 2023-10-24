# Sliding Window
<p> &nb </p>

## 121. Best Time to Buy and Sell Stock 1/6
Find out the maximum profit at the given prices.

### Learned
For me, this is a pre-fix sum problem. The cur - left min should be updated in the res if it smaller than cur max. 


```java
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int min = Integer.MAX_VALUE;
        for (int n: prices){
            max = Math.max(n - min, max);
            min = Math.min(min, n);
        }
        return max;
    }
}
```
## 3. Longest Substring Without Repeating Characters
Find the longest non-duplicated substring.

### Learned
The definition of our window is that all elements in the window are valid. The last character is the only character that could be duplicated in the current window. 

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int l = 0;
        int max = 0;
        
        for (int i = 0; i < s.length(); i++){
            while (set.contains(s.charAt(i))){
                set.remove(s.charAt(l));
                l++;
            }
            set.add(s.charAt(i));
            max = Math.max(i-l+1, max);
        }
        return max;
    }
}
```
