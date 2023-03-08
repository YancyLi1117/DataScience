## Java SE

### Java for-each loop

```java
for(data_type item : collections) {
    ...
}
```

### Java break/continue with label

```java
while (testExpression) {
   // code
   second:
   while (testExpression) {
      // code
      while(testExpression) {
         // code
         break second;
      }
   }
   //jump to here
}
```

### Java Lambda

```java
// 1. 不需要参数,返回值为 5  
() -> 5  
// 2. 接收一个参数(数字类型),返回其2倍的值  
x -> 2 * x  
// 3. 接受2个参数(数字),并返回他们的差值  
(x, y) -> x – y  
// 4. 接收2个int型整数,返回他们的和  
(int x, int y) -> x + y  
// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
(String s) -> System.out.print(s)
```

### Java Bit

<< >> 左移 右移

\>>> 无符号右移，正数右移，高位用0补，负数右移，高位用1补 \<<< 无符号左移 低位用0补

& | ^ ~ 与 或 抑或 非



### Java Copy

for the primitive data type (byte, short, int, long, float, double, boolean and char)

System.arraycopy，Arrays.copyOf：对于基本数据类型 深拷贝 其他浅拷贝

serialization：序列化 

### Java Array

without initialization, int array:0, bool array: false

for multiple dimension array, each line may have a different length

copy the array: 

if use `int [] newnum = num;  ` it's a shallow copy, if num changes, newnum will also change

deep copy: use for loop to assign, or:

`System.arraycopy(Object src, int srcPos,Object dest, int destPos, int length)`

`Arrays.copyOfRange(source, start, end)`

`Arrays.sort(num, comparator)`: 

### Java String &StringBuffer

Java String is **unchangeable**, if you want to change the content, please use stringbuffer.

`length()`: the length of the string

`concat(string)` or +: connect two string

` printf() or format() `: format output, %f for float, %d for int, %s for string 

`charAt(int index)`: the no. index  element

`int indexOf(int ch)`: the first ch index

`str.substring(i,j)`: the sub string of [i,j)

`String replace(char oldChar, char newChar)`: replace the old char with new char

`String replaceAll( String regex, String newChar)`: also replace, but can use regex

`String split(String regex)`: split the string with regex

`String trim()`: remove the blank of front and back from the string

`char toCharArray()`:string to char array

`String Arrays.toString()`: array to string `String.copyValueOf()`

### StringBuffer

`StringBuffer append(String s)`: add more to this string

`StringBuffer reverse()`: reverse the string

`delete(int start, int end)`

` insert(int offset, String str)`

## Java Collection

### Java List

#### Java ArrayList



#### Java LinkedList



#### Java Vector



#### Java Stack

declaration:  `Stack<...> s1 = new Stack<...>();`

`peek( )`: check the top item of the stack, but don't remove

`pop()`: remove the top item of the stack

`push(item)`: push the item into the stack

`serch(item)`: return the place of the item

### Java Set

#### Java HashSet

### Java Queue

#### Java Priority Queue

//补充。。。

## Java Map

### Java HashMap

**The differences between hashmap and map:**

HashMap is the class, and Map is the interface: HashMap implements all the methods of Map

`Map <...> map = new HashMap<..>(); `

## Java Iterator
