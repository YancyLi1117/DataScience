# Leetcode 200

> Number of Islands
>
> BFS, DFS, Union-Find

Given an m x n 2D binary grid which represents a map of '1's (land) and '0's (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four grid edges are surrounded by water.

**Input:** grid = [

 ["1","1","1","1","0"],

 ["1","1","0","1","0"],

 ["1","1","0","0","0"],

 ["0","0","0","0","0"]

]

**Output:** 1

**Input:** grid = [

 ["1","1","0","0","0"],

 ["1","1","0","0","0"],

 ["0","0","1","0","0"],

 ["0","0","0","1","1"]

]

**Output:** 3

## DFS Solution

### Pseudocode

```pascal
function numIslands(grid)	/*int*/ 
	if grid == null or len(grid) == 0
		then return 0
	end if
	result <-0
	row <- len(grid)
	col <- len(grid[0])
	for i <-0 to i < row by i++ do
		for j <-0 to j <col by j++ do
			if grid[i][j] ==‘1’
				then result = result+1
					dfs(grid, i, j, row, col) /*update the grid*/
	return result
end function

function dfs(grid, x, y, row, col) /*void*/
	if x<0 or y<0 or x>=row or y>=col or grid[x][y]==‘0’
		then return
	grid[x][y]<-0
	
	dfs(grid, x, y-1, row, col) /*use recursion*/
	dfs(grid, x, y+1, row, col)
	dfs(grid, x-1, y, row, col)
	dfs(grid, x+1, y, row, col)
end function
```

### Fallible Point

initial judgement--two for loops--the point is 1: result++, and update the grid using dfs

define dfs--the exit condition--change the point 0--update the point around

- for 1 and 0, they are char type
- the initial judgement of grid =null
- use global var will be faster

### Analysis

This way is not so good, especially for the large matrix.

Using the recursion, when it is too deep, it will consume more memory. 

Time complexity: $O(M \times N)$

Space complexity: $O(M\times N)$

### Submission

```java
class Solution {
    int row;
    int col;
    public int numIslands(char[][] grid) {
        
        row = grid.length;
        col = grid[0].length;
        if (grid ==null ||row==0) return 0;
        int result = 0;
        
        for (int i=0; i<row; i++){
            for (int j =0; j<col; j++){
                if (grid[i][j] == '1'){
                    result = result +1;
                    dfs(grid, i, j);
                }
            }
        }
           
        return result;
        
    }
    public void dfs(char[][] grid, int x, int y){
        if (x< 0 || y<0 || x>=row || y>=col || grid [x][y] == '0'){
            return;
        }
        grid[x][y]='0';
        dfs(grid, x, y-1);
        dfs(grid, x, y+1);
        dfs(grid, x-1, y);
        dfs(grid, x+1, y);
    }
}
```

## BFS Solution

### Pseudocode

```pascal
function numIslands(grid)	/*int*/ 
	if grid == null or len(grid) == 0
		then return 0
	end if
	result = 0
	row = len(grid)
	col = len(grid[0])
	for i <-0 to i < row by i++ do
		for j <-0 to j <col by j++ do
			if grid[i][j] ==‘1’
				then result = result+1
					bfs(grid, i, j, row, col) /*update the grid*/
	return result
end function
function bfs(grid, x, y, row, col ) /*void*/
	Queue q
	q.enqueue([x][y])
	grid[x][y]='0'
	while q is not empty do
		cur = q.dequeue()
		a = cur[0]
		b = cur[1]
		if a-1 >=0 and grid[a-1][b]=='1'
			then q.enqueue([a-1][b]) grid[a-1][b]='0'
		if a+1 <row and grid[a+1][b]=='1'
			then q.enqueue([a+1][b]) grid[a+1][b]='0'
		if b-1 >=0 and grid[a][b-1]=='1'
			then q.enqueue([a][b-1]) grid[a][b-1]='0'
		if b+1 <col and grid[a][b+1]=='1'
			then q.enqueue([a][b+1]) grid[a][b+1]='0'
	end while
end function
```

### Fallible Point

initial judgement--two for loops--the point is 1: result++, and update the grid using bfs

define bfs--use the queue--pop the item, add the point around, update them to 0

- store the pair by an integer, or by pair

### Analysis

Time complexity: $O(M \times N)$

Space complexity: $O(\min (M, N))$

### Submission

```java
class Solution {
    int row;
    int col;
    public int numIslands(char[][] grid) {
        if (grid ==null ||grid.length==0) return 0;
        int result = 0;
        row = grid.length;
        col = grid[0].length;
        for (int i=0; i<row; i++){
            for (int j =0; j<col; j++){
                if (grid[i][j] == '1'){
                    result = result +1;
                    bfs(grid, i, j);
                }
            }
        }
           
        return result;
        
    }
    public void bfs(char[][] grid, int x, int y){
        
        grid[x][y]='0';
        Queue<Integer> q = new LinkedList<>();
        q.add(x*col+y);
        while(!q.isEmpty()){
            int cur = q.remove();
            int a = cur/col;
            int b = cur%col;
            if (a-1>=0 && grid[a-1][b] =='1'){
                q.add((a-1)*col+b);
                grid[(a-1)][b] = '0';         
            }
            if (a+1<row && grid[a+1][b] =='1'){
                q.add((a+1)*col+b);
                grid[(a+1)][b] = '0';         
            }
            if (b-1>=0 && grid[a][b-1] =='1'){
                q.add(a*col+b-1);
                grid[a][b-1] = '0';         
            }
            if (b+1<col && grid[a][b+1] =='1'){
                q.add(a*col+b+1);
                grid[a][b+1] = '0';         
            }
        } 
        
    }
}
```

## Find-Union Solution

### Pseudocode

