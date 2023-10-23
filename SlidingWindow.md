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
