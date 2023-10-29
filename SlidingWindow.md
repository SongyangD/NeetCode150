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
## 3. Longest Substring Without Repeating Characters 2/6
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

<p> &nb </p>

## 424. Longest Repeating Character Replacement 3/6

### Learned
- Find out the valid window with k number of impurities
- Top element could only be replaced by the new character discovered by right pointer
- inpurities(k) = r-l+1 - top
- Move left pointer when inpurities > k, and remove left character from the window

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int l = 0;
        int max = 0;
        int[] count = new int[26];
        int top = 0;
        for (int i = 0; i < s.length(); i++){
            char cur = s.charAt(i);
            count[cur - 'A']++;
            top = Math.max(top, count[cur - 'A']);
            
            while (i-l+1-top > k){
                char left = s.charAt(l);
                count[left - 'A']--;
                l++;
            }
            max = Math.max(i-l+1, max);
        }
        return max;
    }
}
```

## 567. Permutation in String 4/6
Whether S2 contains S1's permutation

### Learned
- Control two pointers and maintain the window
- Don't forget the corner case: if (s1.length() > s2.length()) return false;
```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        if (s1.length() > s2.length()) return false;
        int[] count = new int[26];
        for (char c: s1.toCharArray()){
            count[c -'a']++;
        }
        int l = 0;
        for (int r = 0; r < s1.length(); r++){
            char cur = s2.charAt(r);
            count[cur - 'a']--;
        }
        if (allZero(count)) return true;
        for (int r = s1.length(); r < s2.length(); r++){
            count[s2.charAt(r) - 'a']--;
            count[s2.charAt(l) - 'a']++;
            l++;
            if (allZero(count)) return true;
        }
        return false;
    }
    private boolean allZero(int[] count){
        for (int n : count){
            if (n != 0){
                return false;
            }
        }
        return true;
    }
}
```
## 76. Minimum Window Substring 5/6

###

```java
class Solution {
    public String minWindow(String s, String t) {
        if (s.length() < t.length()) return "";
        int min = s.length()+1;
        int st = 0;
        int ed = 0;

        Map<Character, Integer> target = new HashMap<>();
        Map<Character, Integer> cur = new HashMap<>();
        
        for (char c: t.toCharArray()){
            target.put(c, target.getOrDefault(c, 0)+1);
        }
        int need = target.size();
        int have = 0;

        int l = 0;
        for (int r = 0; r < s.length(); r++){
            char right = s.charAt(r);
            if (target.containsKey(right)){
                cur.put(right, cur.getOrDefault(right, 0)+1);
                if (cur.get(right).equals(target.get(right))){
                    have ++;
                }
            }

            while (have == need){
                if (r-l+1 < min){
                    min = r-l+1;
                    st = l;
                    ed = r;
                }

                char left = s.charAt(l);
                if (target.containsKey(left)){
                    cur.put(left, cur.get(left)-1);
                    if (cur.get(left) < (target.get(left))){
                        have --;
                    }
                }
                l ++;
            }
        }
        if (min == s.length()+1) return "";
        return s.substring(st, ed+1);
    }
}
```
## 239. Sliding Window Maximum 6/6

### Learned 
Monotonically Descending q
1. add if descending order
2. poll first if out of the window
3. if (r+1 >= k) add to the res, AND move left pointer
4. move right pointer

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] res = new int[nums.length-k+1];
        Deque<Integer> q = new LinkedList<>();

        int l = 0;
        int r = 0;
        int idx = 0;
        while (r < nums.length){
            while (!q.isEmpty() && nums[q.peekLast()] < nums[r]){
                q.pollLast();
            }
            q.offerLast(r);
            
            while (!q.isEmpty() && q.peekFirst() < l){
                q.pollFirst();
            }

            if (r+1 >= k){
                res[idx] = nums[q.peekFirst()];
                idx++;
                l++;
            }
            r++;
        }
        return res;
    }
}
```
