# Two Pointers - 5/5
<p>&nbsp;</p>

## 215 Valid Palindrome - 1/5
Find whether one sentence is a palindrome 
### Learned
How to control two pointers with some invalid elements

#### Method1: use if in while 
``` java
class Solution {
    public boolean isPalindrome(String s) {
        int l = 0;
        int r = s.length()-1;
        s = s.toLowerCase();
        while (l < r){
            if (!Character.isLetterOrDigit(s.charAt(l))){
                l++;
                continue;
            }
            if (!Character.isLetterOrDigit(s.charAt(r))){
                r--;
                continue;
            }
            if (s.charAt(l)!= s.charAt(r)) return false;
            l++;
            r--;
        }
        return true;
    }
}
```
#### Method2: use while in while 
🕳️ Pay attention to the range 🧱:
```java
class Solution {
    public boolean isPalindrome(String s) {
        int l = 0;
        int r = s.length()-1;
        s = s.toLowerCase();
        while (l < r){
            while (l < s.length() && !Character.isLetterOrDigit(s.charAt(l))){
                l++;
            }
            while (r >= 0 && !Character.isLetterOrDigit(s.charAt(r))){
                r--;
            }
            if (l < r && s.charAt(l)!= s.charAt(r)) return false;
            l++;
            r--;
        }
        return true;
    }
}
```
<p>&nbsp;</p>
  

## 167. Two Sum II - Input Array Is Sorted - 2/5
Find two sum in a sorted array

### Learned
'Converging pointers' technique is usually applied to the **sorted array**.

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int l = 0;
        int r = numbers.length-1;
        int[] res = new int[2];
        while (l < r){
            int cur = numbers[l]+numbers[r];
            if (cur == target){
                res[0] = l+1;
                res[1] = r+1;
                break;
            }else if (cur > target){
                r--;
            }else if (cur < target){
                l++;
            }
        }
        return res;
    }
}
```
<p>&nbsp;</p>


## 15. 3Sum - 3/5
Given an array with duplicate elements, find all triplets that make sum equal to 0.

### Learned
Variation of the "Converging pointers". We need to nest a "Converging pointers" in a one pass. 
> [!IMPORTANT]
> To avoid duplication, make sure the  first and second elements are non-duplicate with previous answers. 

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        List<List<Integer>> res = new ArrayList<>();

        for (int i = 0; i < n; i++){
            if (i-1 >= 0 && nums[i-1] == nums[i]){
                continue;
            }
            if (nums[i] > 0) break;
            int l = i+1;
            int r = n-1;
            while (l < r){
                int cur = nums[i]+nums[l]+nums[r];
                if (cur < 0){
                    l++;
                }else if (cur > 0){
                    r--;
                }else{
                    List<Integer> comb = new ArrayList<>();
                    comb.add(nums[i]);
                    comb.add(nums[l]);
                    comb.add(nums[r]);
                    res.add(comb);
                    
                    l++;
                    while(l < n && nums[l-1] == nums[l]){
                        l++;
                    }
                }
            }
        }

        return res;
    }
}
```

<p>&nbsp;</p>


## 11. Container With Most Water - 4/5
Given heights array, find out the most water these heights of Pillars can contain. 
Variation of the "Converging pointers".
> [!IMPORTANT]
> This is not a sorted array. We move the lower pillar.

```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0;
        int r = height.length-1;
        int max = 0;
        while (l < r){
            int area = 0;
            if (height[l] < height[r]){
                area = (r-l)*height[l];
                max = Math.max(area, max);
                l++;
            }else{
                area = (r-l)*height[r];
                max = Math.max(area, max);
                r--;
            }
        }
        return max;
    }
}
```
<p>&nbsp;</p>

## 42. Trapping Rain Water - 5/5
Figure out how much water the elevation map can trap.

### Learned
I considered this question as a pre-fix sum question. 
> [!IMPORTANT]
> The water we can trap for each position = min(max right, max left) - its height (the result should also compare with 0)


```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] left = new int[n];
        left[0] = 0;
        int[] right = new int[n];
        right[n-1] = 0;
        for (int i = 1; i < n; i++){
            left[i] = Math.max(left[i-1], height[i-1]);
        }
        for (int i = n-2; i >= 0; i--){
            right[i] = Math.max(right[i+1], height[i+1]);
        }
        int res = 0;
        for (int i = 0; i < n; i++){
            int cur = Math.max(Math.min(left[i], right[i]) - height[i], 0);
            res += cur;
        }
        return res;
    }
}
```






