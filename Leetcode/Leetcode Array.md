# Leetcode Array

[TOC]

## Easy

### Leetcode 125

the regex! review

```java
class Solution {
    public boolean isPalindrome(String s) {
        String regex = "[^a-z0-9]+";
        s = s.toLowerCase().replaceAll(regex, "");
        if(s == null || s.length() ==0) return true;
        for (int i = 0; i< s.length()/2; i++){
            if (s.charAt(i) != s.charAt(s.length()-1-i)) return false;
        }
        return true;
    }
}
```



### Leetcode 485

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int count = 0, res =0 ;
        for (int i=0; i<nums.length; i++){
            if(nums[i] == 1){
                count++;
            } 
            else {
                res = Math.max(res, count);
                count =0;
            }
        }
        return Math.max(res, count);
    }
        
}
```

### Leetcode 242

or use the alphabet sequence to store instead of the map

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        Map <Character, Integer> m = new HashMap<>();
        for (int i =0 ;i<s.length(); i++){
            char ch = s.charAt(i);
            m.put(ch,m.getOrDefault(ch,0)+1);
        }
        for(int i =0 ;i<t.length(); i++){
            char c = t.charAt(i);
            if (m.containsKey(c)){
                m.put(c,m.get(c)-1);
            }
            else return false;    
            
        }
        for(char d:m.keySet()){
            if(m.get(d)!=0) return false;
        }
       return true;
        
    }
}
```

### Leetcode 409

```java
class Solution {
    public int longestPalindrome(String s) {
        Map <Character, Integer> m = new HashMap<>();
        for(int i=0; i<s.length();i++){
            char ch = s.charAt(i);
            m.put(ch, m.getOrDefault(ch,0)+1);
        }
        int count = 0;
        
        for (char item:m.keySet()){
            count = m.get(item)/2+count;
        }
        if(count*2<s.length()) return count*2+1;
        else return count*2;
        
    }
}
```

### Leetcode 169

```java
class Solution {
    public int majorityElement(int[] nums) {
        Map <Integer, Integer> m = new HashMap<>();
        for(int item :nums){
            m.put(item, m.getOrDefault(item, 0)+1);
        }
        int max_count= 0;
        int max_item = nums[0];
        for(int item :m.keySet()){
            if(max_count< m.get(item)){
                max_item = item;
            }
            max_count = Math.max(max_count,m.get(item));
        }
        return max_item;
    }
}
```

### Leetcode 217

hashset 

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set <Integer> s = new HashSet<>(nums.length);
        for(int item:nums){
            if(s.contains(item))  return true;
            s.add(item);
        }
        return false;
    }
}
```

### Leetcode 977

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int [] sq = new int[nums.length];
        int left = 0;
        int right = nums.length-1;
        for (int i = nums.length-1; i >=0 ; i--){
            if (Math.abs(nums[right])>Math.abs(nums[left])){
                sq[i] = nums[right]*nums[right];
                right--;
            }
            else{
                sq[i] = nums[left]*nums[left];
                left++;
            }
        }
            

        
        return sq;
    }
}
```

### Leetcode 252

lambda function and array sort

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> (o1[0] - o2[0]));
        for( int i=0; i<intervals.length-1;i++){
            if (intervals[i][1]>intervals[i+1][0]) return false;
        }
        return true;
    }
}
```

### Leetcode 14

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        
        for(int i = 0 ; i<strs[0].length() ; i++){
            for (int j = 0 ; j < strs.length ; j++){
                if (i == strs[j].length() || strs[j].charAt(i) != strs[0].charAt(i) )
                    return strs[0].substring(0,i);
            }
        }
        return strs[0];
    }
}
```

### Leetcode 40

```java
class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> m = new HashMap<>();
        m.put('I',1);
        m.put('V',5);
        m.put('X',10);
        m.put('L',50);
        m.put('C',100);
        m.put('D',500);
        m.put('M',1000);
        int sum =0, i;
        for (i =0; i< s.length()-1; i++){
            if(m.get(s.charAt(i))<m.get(s.charAt(i+1)))  sum = sum-m.get(s.charAt(i));
            else sum = sum + m.get(s.charAt(i));
        }
        return sum+m.get(s.charAt(i));
        
        
    }
}
```



## N Sum/Hash

### Leetcode 1

use hashmap to store the data

```java
class Solution {
    public int[] twoSum(int[] nums, int target) { 
        int [] mylist=new int[2];
        Map<Integer,Integer> map = new HashMap<>();
        for (int i =0; i< nums.length; i++){
            if (map.containsKey(target-nums[i])) {
                mylist[0] = map.get(target-nums[i]);
                mylist[1] = i;
                return mylist;
            }
            else map.put(nums[i],i);
        }
        return null;
    }
}
```

### Leetcode 167

Two pointers!!! when meeting the sorted array, first consider the two pointers

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left =0;
        int right = numbers.length-1;
        while (left < right){
            int sum = numbers[left] + numbers[right];
            if (sum == target)
                return new int[] {left+1, right+1};
            if (sum < target) left++;
            if (sum > target) right--;
            
        }
        return null;
    }
}
```

### Leetcode 383

```java
class Solution {
    public boolean canConstruct(String r, String ma) {
        Map<Character, Integer> m = new HashMap<>();
        for(int i =0 ;i<ma.length();i++){
            char c= ma.charAt(i);
            m.put(c,m.getOrDefault(c,0)+1);
        }
        for(int i =0; i<r.length();i++){
            char d = r.charAt(i);
            if(m.containsKey(d)==false) return false;
            else if (m.get(d) == 0) return false;
            else m.put(d,m.get(d)-1);
            
        }
        return true;
    }
}
```



## Two Pointers

### Leetcode 26

the basic two pointers

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int slow = 0;
        int fast = 0;
        while(fast <nums.length){
            if(nums[slow] != nums[fast]){
                slow++;
                nums[slow]=nums[fast];
            }
            fast++;
        }
        return slow+1;
    }
}
```

### Leetcode 27

This is an example of two pointers

#### Solution 1: reader and writer

j is the reader, i is the writer, and count the number

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i =0;
        for (int j=0; j<nums.length; j++){
            if (nums[j]!= val){
                nums[i]=nums[j];
                i++;
            }
        }
        return i;
    }
}
```

#### Solution 2 : scanner and counter

i is the scanner, n is the counter

```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    int n = nums.length;
    while (i < n) {
        if (nums[i] == val) {
            nums[i] = nums[n - 1];
            // reduce array size by one
            n--;
        } else {
            i++;
        }
    }
    return n;
}
```

### Leetcode 283

nearly the same as 27

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int left = 0;
        for(int i =0; i< nums.length; i++){
            if (nums[i] !=0){
                nums[left] = nums[i];
                left++;
            }
        }
        for(int i = left; i<nums.length; i++){
            nums[i]=0;
        }
    }
}
```

### Leetcode 344

reverse the array, just switch the val of two pointers 

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length-1;
        while(left<right){
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```

### Leetcode 5

core: the clarification of the function--string, left, right

find for every char, the longest palindrome string. 

```java
class Solution {
    public String longestPalindrome(String s) {
        String finals="";
        for (int i =0; i< s.length();i++){
            String s1 = Palindrome(s,i,i);
            String s2 = Palindrome(s,i,i+1);
            if(finals.length()<s1.length()) finals = s1;
            if (s2.length()>finals.length()) finals = s2;
        }
        return finals;
    }
    public String Palindrome(String s, int l, int r){
        while(l>=0 && r<s.length() && s.charAt(l) == s.charAt(r)){
            l--;
            r++;
        }
        return s.substring(l + 1, r);
        
    }
}
```

## Pre-processing

recall the sumrange function for many times, if put for loop in this, it's more complex.

So: pre-process the array, set new an prefix array

### Leetcode 303

```java
class NumArray {
    private int[] prenums;
    public NumArray(int[] nums) {
        prenums = new int[nums.length+1];
        for(int i =0; i<nums.length; i++){
            prenums[i+1] = nums[i] + prenums[i];
        }
    }
    
    public int sumRange(int left, int right) {
        return prenums[right+1] - prenums[left];
    }
}

```

### Leetcode 304

```java
class NumMatrix {
    private int [][] sum;
    public NumMatrix(int[][] matrix) {
        sum = new int[matrix.length + 1][matrix[0].length + 1];
        for (int i=0; i<matrix.length; i++){
            for (int j=0 ;j< matrix[0].length; j++){
                sum[i+1][j+1] = matrix[i][j]+sum[i][j+1]+sum[i+1][j]-sum[i][j];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sum[row2 + 1][col2 + 1] - sum[row1][col2 + 1] - sum[row2 + 1][col1] + sum[row1][col1];
    }
}
```

### Leetcode 370

the array stores the difference array, just update the start and end for every step

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] diff = new int[length];
        for (int[] item: updates){
            int start = item[0];
            int end = item[1];
            int val = item[2];
            diff[start] = diff[start]+val;
            if (end<length-1) 
                diff[end + 1] = diff[end + 1] - val;
            
        }
        for (int i = 1; i < length; i++) {
             diff[i] += diff[i - 1];
         }
        return diff;
    }
}
```

### Leetcode 1109

totally the same as 370

```java
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {
        int [] diff = new int[n];
        for (int[] item :bookings){
            int start = item[0]-1;
            int end = item[1]-1;
            int val = item[2];
            diff[start] += val;
            if(end + 1<n) diff[end + 1] -= val;
        }
        for (int i =1;i<n; i++)
            diff[i] += diff[i-1];
        return diff;
    }
}
```

### Leetcode 1094

```java
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {
        int[] diff = new int[1001];
        for (int[] item: trips){
            int val = item[0];
            int start = item[1];
            int end = item[2];
            diff[start] += val;
            diff[end] -= val;
        }
        int pass = 0;
        for (int num : diff){
            pass += num;
            if (pass > capacity) return false;
            
        }
        return true;
    }
}
```

## Traverse the array

### Leetcode 48

Firstly reverse the matrix by the diagonal, and reverse by line

```java
class Solution {
    public void rotate(int[][] matrix) {
        for(int i = 0; i< matrix.length; i++){
            for( int j =i; j< matrix[0].length; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }
        for (int i = 0; i< matrix.length; i++){
            reverse(matrix[i]);
        }
    }
    public void reverse(int[] array){
        for( int i =0; i<array.length/2 ;i++){
            int temp = array[i];
            array[i] = array[array.length-1-i];
            array[array.length-1-i] = temp;
        }
    }
}
```

### Leetcode 151

```java
class Solution {
  public String reverseWords(String s) {
    // remove leading spaces
    s = s.trim();
    // split by multiple spaces
    List<String> wordList = Arrays.asList(s.split("\\s+"));
    Collections.reverse(wordList);
    return String.join(" ", wordList);
  }
}
```

### Leetcode 54

spiral traverse: set the boundary, and shrink the bound.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int x = matrix.length, y = matrix[0].length;
        int up = 0, down = x-1;
        int left = 0, right = y-1;
        List<Integer> res = new ArrayList<Integer>();
        
        while(res.size()<x*y){
            if (up<=down){
                for( int i =left; i<= right; i++)
                res.add(matrix[up][i]);
            up++;
            }
            if (left<=right){
                for(int i = up; i<= down; i++)
                    res.add(matrix[i][right]);
                right--;
            }
            if(up<=down){
                for( int i = right; i>=left ; i--)
                    res.add(matrix[down][i]);
                down--;
            }
            if (left <= right){
                for (int i = down; i>= up; i--)
                    res.add(matrix[i][left]);
                left++;
            }
            
        }
        return res;
    }
}
```

### Leetcode 59

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] matrix = new int[n][n];
        int up = 0, down = n-1;
        int left = 0, right = n-1;
        int num = 1;
        while(num<=n*n){
            if(down>=up){
                for(int i = left; i<=right; i++){
                    matrix[up][i]= num;
                    num++;
                }
                up++;   
            }
            if (right>=left){
                for (int i=up; i<=down; i++){
                    matrix[i][right] = num;
                    num++;
                }
                right--;
            }
            if(down>=up){
                for(int i=right; i>=left; i-- ){
                    matrix[down][i] = num;
                    num++;
                }
                down--;
            }
            if(right>=left){
                for (int i=down; i>=up; i--){
                    matrix[i][left] = num;
                    num++;
                }
                left++;
            }
        }
        return matrix;
        
    }
}
```

## Slide Window

### Leetcode 76 

need to repeat tomorrow

https://labuladong.github.io/algo/2/20/27/

```java
class Solution {
    public String minWindow(String s, String t) {
         String res = "";
        if ( t.length() > s.length() ) {
            return res;
        }

        Map <Character, Integer> window = new HashMap<Character, Integer>();
        Map <Character, Integer> need = new HashMap<Character, Integer>();
        for (int i =0 ; i<t.length(); i++)
            need.put(t.charAt(i), need.getOrDefault( t.charAt(i), 0 ) + 1 );
        int left = 0, right = 0;
        int valid = 0;
        int start = 0, len = Integer.MAX_VALUE;
        while(right<s.length()){
            char c = s.charAt(right);
            right++;
            if (need.containsKey(c)){
                window.put(c,window.getOrDefault( c, 0 ) + 1);
                if(window.get( c ).equals( need.get( c ) )) valid++;
            }
            while(valid == need.size()){
                if(right-left < len){
                    start = left;
                    len = right-left;
                }
                char d = s.charAt(left);
                left++;
                if(need.containsKey(d)){ 
                    if( window.get( d ).equals( need.get( d ) ) ) valid--;
                    window.put(d,window.get(d) - 1);   
                }
            }
        }
        return len == Integer.MAX_VALUE ? "" : s.substring( start, start+len );
        
    }
}
```

### Leetcode 567

**Java 的 Integer，String 等类型判定相等应该用 `equals` 方法而不能直接用等号 `==`**

```java
class Solution {
    public boolean checkInclusion(String t, String s) {
        Map <Character, Integer> window = new HashMap<Character, Integer>();
        Map <Character, Integer> need = new HashMap<Character, Integer>();
        for (int i =0 ; i<t.length(); i++)
            need.put(t.charAt(i), need.getOrDefault( t.charAt(i), 0 ) + 1 );
        int left = 0, right = 0;
        int valid = 0;
        
        while(right<s.length()){
            char c = s.charAt(right);
            right++;
            if (need.containsKey(c)){
                window.put(c,window.getOrDefault( c, 0 ) + 1);
                if(window.get( c ).equals( need.get( c ) )) valid++;
            }
            while(right-left == t.length()){
                if(valid == need.size()) return true;
                char d = s.charAt(left);
                left++;
                if(need.containsKey(d)){ 
                    if( window.get( d ).equals( need.get( d ) ) ) valid--;
                    window.put(d,window.get(d) - 1);   
                }
            }
        }
        return false;
    }
}
```

### Leetcode 468

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        Map<Character,Integer> window = new HashMap <>();
        Map <Character,Integer> need = new HashMap<>();
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i<p.length();i++){
            char ch = p.charAt(i);
            need.put(ch,need.getOrDefault(ch,0)+1);
        }
        int left = 0, right = 0;
        int valid = 0;
        while(right<s.length()){
           char c = s.charAt(right);
            right++;
            if (need.containsKey(c)){
                window.put(c,window.getOrDefault( c, 0 ) + 1);
                if(window.get( c ).equals( need.get( c ) )) valid++;
            }
            while(right-left == p.length()){
                if(valid == need.size()) res.add(left);
                char d = s.charAt(left);
                left++;
                if(need.containsKey(d)){ 
                    if( window.get( d ).equals( need.get( d ) ) ) valid--;
                    window.put(d,window.get(d) - 1);   
                }
            }
        }
        return res;
    }
}
```

### Leetcode 3

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> window = new HashMap <>();
        int left = 0, right = 0;
        int res = 0;
        while(right < s.length()){
            char c = s.charAt(right);
            right++;
            window.put(c,window.getOrDefault(c,0)+1);
            
            while(window.get(c) >1){
                char d = s.charAt(left);
                window.put(d,window.getOrDefault(d,0) - 1);
                left++;
            }
            res = Math.max(res,right - left);
            
        }
        return res;
    }
}
```

### Leecode 28 

brute-force solution is enough

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int L = haystack.length();
        int N = needle.length();
        for( int i = 0 ; i<= L-N ; i++){
            if (haystack.substring(i,N+i).equals(needle))
                return i;
        }
        return -1;
    }
}
```



## Stack

### Leetcode 20

Valid Parentheses

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

```java
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (int i=0; i<s.length(); i++){
            char ch=s.charAt(i);
            if(stack.size()==0){
                stack.push(ch);
            }else{
                if (ch == '(' || ch =='['||ch =='{'){
                    stack.push(ch);
                }
                else{
                    if (ch == ')' && stack.pop() != '(') return false;
                    if (ch == ']' && stack.pop() != '[') return false;
                    if (ch == '}' && stack.pop() != '{') return false;
            }
           
            }
            
        }
        if (!stack.isEmpty()) return false;
        else return true;
    }
}
```

Attention: 

- the situation that we just push a single ''[''

- the difference between ==**string=null and string.length()=0**==: 

  - string s ="", which means s.length()=0, content is empty, use `  if (str.length() == 0);` and `if (str.equals(""));`to detect. 
  - string s = null, which means a pointer to null, and no memory has been distributed to it yet.

  to detect the string is neither empty nor null: `if(s == null || s.length() == 0);`, but don't change the sequence! It's **invalid for a null string calculating the length**.

### Leetcode 735

For this question, the most important is: to keep clear in mind!

trick: during multiple situations, set a **boolean flag** to deal with different conditions

and step by step!

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Stack<Integer> s = new Stack();
        for (int num: asteroids){
            if (num >0 ||s.isEmpty() == true){
                s.push(num);
            }else{
                boolean flag = true;
                while (s.isEmpty() == false && s.peek() >0 ){
                    if (-num > s.peek()){
                        s.pop();
                        flag = true;
                    }
                    else if (-num == s.peek()){
                        s.pop();
                        flag = false;
                        break;
                    }else{
                        flag = false;
                        break;
                    }        
                }
                if (flag == true) s.push(num);
                }
            }
             
        int[] ans = new int[s.size()];
        for (int t = ans.length - 1; t >= 0; --t) {
            ans[t] = s.pop();
        }
        return ans;
    }
}
```

### Leetcode 232

using two stacks to generate a queue

```java
class MyQueue {
    
    private Stack<Integer> s1,s2;
    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    public void push(int x) {
        s1.push(x);
    }
    
    public int pop() {
        if(s2.isEmpty()){
            while(!s1.isEmpty()) 
                s2.push(s1.pop());
        }
        return s2.pop();
    }
    
    public int peek() {
        if(s2.isEmpty()){
            while(!s1.isEmpty()) 
                s2.push(s1.pop());
        }
        return s2.peek();
    }
    
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty(); 
    }
}
```

### Leetcode 844

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
       return gen(s).equals(gen(t));
        
    }
    public String gen(String s){
        Stack <Character> st = new Stack<>();
        for(int i =0 ;i< s.length();i++){
            if (s.charAt(i) != '#') st.push(s.charAt(i));
            else if(st.isEmpty() == false) st.pop();
        }
        return String.valueOf(st);
    }
}
```

time and space: O(M+N)

```java
class Solution {
    public boolean backspaceCompare(String s, String t) {
        int backs = 0, backt = 0;
        int i = s.length()-1, j = t.length()-1;
        while(i>=0 ||j>=0){
            while(i>=0){
                if(s.charAt(i) == '#') {backs++;i--;}
                else if (backs>0) {backs--;i--;}
                else break;   
            }
            while(j>=0){
                if(t.charAt(j) == '#') {backt++;j--;}
                else if (backt>0) {backt--;j--;}
                else break;   
            }
            if(i>=0 && j>=0){
                if(s.charAt(i)!=t.charAt(j)) return false;
            }
            else if(i>=0 || j>=0) return false;
            i--;j--;
        }  
        return true;
    }
    
}
```

time: O(M+N)  space: O(1)

两个指针ij，从后往前遍历，backs backt计数，三种情况：

- 遇到#--back++，i--
- back！=0， back--，i-- 
- 其他比较两个是否相等，相等往前移动，不等return false

结束情况：正常应该ij都是-1；如果两个都不是，继续进行；其中一个是一个不是，return false

## Binary Search

### Leecode 704

the basic binary search

```java
class Solution {
    public int search(int[] nums, int target) {
        int left=0, right = nums.length-1;
        while(left<=right){
            int mid = left +(right-left)/2;
            if(target<nums[mid]) right = mid-1;
            else if (target>nums[mid]) left = mid+1;
            else if (target==nums[mid]) return mid;
        }
        return -1;
    }
}
```



### Leetcode 35

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int low = 0;
        int high = nums.length-1;
        int mid = (low + high) /2;
        while (mid>low){
            if (target>nums[mid])
                low = mid;
            else
                high = mid;
            mid = (low + high)/2;
            System.out.println(low+" "+ mid+" "+ high);
        }
        if (nums[high]<target){
            return high+1;
        }
        else if (nums[low]>=target) return low;
            else
                return low+1;
    }
}
```

### Leetcode 278 

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1, right = n;
        while(left<=right){
            int mid = left + (right - left) / 2;
            if (isBadVersion(mid) == false) left = mid+1;
            else  right = mid -1;
                
            
        }
        return left;
    }
}
```

## Stock

### Leetcode 121

using dynamic programming: 

```java
class Solution {
    public int maxProfit(int[] prices) {
        int dp[][] = new int [prices.length][2];
        int i;
        for (i = 0; i<prices.length; i++){
            if (i == 0){
                dp[0][1] = -prices[0];
                dp[0][0] = 0; 
                continue;
            }
            dp[i][0] = Math.max(dp[i-1][0] , dp[i-1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i-1][1] , - prices[i]);
        }
        return dp[i - 1][0];
    }
}
```

```java
//using normal array
class Solution {
    public int maxProfit(int[] prices) {
        int mini = Integer.MAX_VALUE;
        int maxp = 0;
        for (int i = 0 ; i< prices.length; i++){
            if (mini>prices[i]) mini = prices[i];
            else if (maxp < prices[i]-mini) maxp = prices[i]-mini;
        }
        return maxp; 
    }
}
```

### Leetcode 70

f(n)=f(n-1)+f(n-2)

```java
class Solution {
    public int climbStairs(int n) {
        int[] f = new int[n+2];
        f[1]=1;f[2]=2;
        for(int i=3;i<n+2;i++)
            f[i]=f[i-1]+f[i-2];
        return f[n];
            
    }
}
```

## Binary

### Leetcode 67

```java
class Solution {
    public String addBinary(String a, String b) {
        int carry=0;
        StringBuilder res = new StringBuilder();
        int i=a.length()-1, j=b.length()-1;
        while(i>=0 || j>=0){
            int sum = 0;
            if(i>=0) sum = sum+a.charAt(i)-'0';
            if(j>=0) sum = sum+b.charAt(j)-'0';
            sum = sum+carry;
            carry = sum/2;
            res.append(sum%2);
            i--;j--;
        }
        if(carry!=0) res.append(carry);
        return res.reverse().toString();
    }
}
```

sum分开加，三部分：a b 进位，最后不要忘记最高位判断

### Leetcode 338

```java
class Solution {
    public int[] countBits(int n) {
       int[] res = new int[n+1];
        for(int i=0; i<=n;i++){
           res[i] = hasone(i);
       }
        return res;
    }
   public int hasone(int n){
        int count=0;
        while(n>0){
            if (n%2 ==1) count=count+1;
            n=n/2;
            }
        return count;
   } 
    
}
```

time: O(n*logn) space: O(1)

```java
class Solution {
    public int[] countBits(int n) {
       int[] res = new int[n+1];
        res[0]=0;
       for(int i=1; i<=n;i++){
           res[i] = res[i>>1]+i%2;
       }
        return res;
    }
    
}
```

P(x)=P(x/2)+x mod 2

time: O(n) space: O(1)

### Leetcode 191

n&(n-1): 消除最后一个1

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count=0;
        int mask =1;
        for(int i =0;i<32;i++){
            if((n & mask)!=0) count++;
            mask=mask<<1;
        }
        return count;
    }
}
```

```java
int hammingWeight(int n) {
    int res = 0;
    while (n != 0) {
        n = n & (n - 1);
        res++;
    }
    return res;
}
```

### Leetcode 136

```java
class Solution {
    public int singleNumber(int[] nums) {
        int n=0;
        for(int res:nums){
            n=n^res;
        }
        return n;
    }
}
```

### Leetcode 268

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n=nums.length;
        int sum = (1+n)*n/2;
        int now = 0;
        for(int i=0;i<n;i++)
            now = nums[i]+now;
        return sum-now;
    }
}
```

```java
class Solution {
    public int missingNumber(int[] nums) {
        int res =0;
        for(int i=0;i<nums.length;i++){
            res= res^(i+1)^nums[i];
            
        }
        return res;
    }
}
```

### Leetcode 190

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int res = 0;
        for(int i = 0; i < 32; i++){
        	res <<=1;
            res += (n>>i)&1;
        }
        return res;
    }
}
```

