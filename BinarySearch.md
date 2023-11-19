# Binary Search - 5/7

## 704. Binary Search - 1/7
Classic 
### Learned 

```java
class Solution {
    public int search(int[] nums, int target) {
        int l = 0; 
        int r = nums.length-1;
        while (l <= r){
            int m = (r-l)/2+l;
            if (nums[m] == target){
                return m;
            } else if (nums[m] > target){
                r = m-1;
            }else {
                l = m+1;
            }
        }
        return -1;
    }
}
```

## 74. Search a 2D Matrix - 2/7


