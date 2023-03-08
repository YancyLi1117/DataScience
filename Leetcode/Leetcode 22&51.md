# Leetcode 22&51

backtracking

https://labuladong.github.io/algo/4/31/110/

22s1: list all first, and trim, more time

```java
class Solution {
    private List<String> res = new ArrayList<String>();
    public List<String> generateParenthesis(int n) {
        char [] line = new char[2*n];
        for (int i = 0; i<2*n; i++)
            line[i] = '(';
        backtrack(line, 1);
        return res;
        
    }
    private void backtrack(char[] line, int row){
        
        if (row == line.length){
            if (isValid(line) == true){
            res.add(String.copyValueOf(line));
            }
            
        } else{
            line[row] = ')';
            backtrack(line, row+1);
            line[row] = '(';
            backtrack(line, row+1);
            
        }   
    }
    public boolean isValid(char [] line) {
        Stack<Character> stack = new Stack<Character>();
        
        for (int i=0; i<line.length; i++){
            char ch=line[i];
            if(stack.size()==0){
                stack.push(ch);
            }else{
            if (ch == '(')
                stack.push(ch);
            else if (ch == ')' && stack.pop() != '(') return false;
            
        }
        }
        if (!stack.isEmpty()) return false;
        else return true;
    }
   
    
}
```

22s2: trim and list

```
class Solution {
    private List<String> res = new ArrayList<String>();
    public List<String> generateParenthesis(int n) {
        char [] line = new char[2*n];
        line[0]= '(';
        backtrack(line, 1, 0);
        return res;
        
    }
    private void backtrack(char[] line, int open, int close){
        int row = open + close;
        if (row == line.length){
            res.add(String.copyValueOf(line));
            
        } 
        if (open < line.length/2){
            line[row] ='(';
            backtrack(line, open+1, close);
        }
        if (close < open){
            line[row] =')';
            backtrack(line, open, close+1);
        }
    } 
    
}
```

51

```java
class Solution {
    private List<List<String>> res = new ArrayList<List<String>>();
    public List<List<String>> solveNQueens(int n) {
        char board[][] = new char[n][n];
        for (int i = 0; i < n; i++) 
            for (int j = 0; j < n; j++) 
                board[i][j] = '.';
            
        backtrack(board, 0);
        return res;
    }
    
   private void backtrack(char[][] board, int row){
        if (row == board.length ){
            res.add(build(board));
            return;
        }
        for ( int col = 0; col < board.length; col++){
            if (isValid(board, row, col) == false)
                continue;
            board[row][col] = 'Q';
            backtrack (board, row+1);
            board[row][col] = '.';
        }
    }
    private List<String> build (char[][] board){
        List<String> vec = new ArrayList<String>();
        for (int i=0; i<board.length; i++){
            vec.add(String.copyValueOf(board[i]));
        }
        return vec;
    }
    private boolean isValid( char[][] board, int row, int col){
        int n = board.length;
        for (int i = 0; i < n; i++) {
        if (board[i][col] == 'Q')
            return false;
    }
    
    for (int i = row - 1, j = col + 1; 
            i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q')
            return false;
    }
    
    for (int i = row - 1, j = col - 1;
            i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q')
            return false;
    }
    return true;
    }
    
}
```

