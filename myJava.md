# 数组

## 切片

```java
public int[] getSlice(int[] array, int start, int end) {
    return Arrays.copyOfRange(array, start, end);
}//这两种方法都可以返回原数组的一个切片，从索引start（包含）到end（不包含）。
```

# 树

## 二叉搜索树

```java
中序遍历二叉搜索树等于遍历有序数组
```

# StringBuilder

## insert

```java
import java.util.StringBuilder;  
  
public class Main {  
    public static void main(String[] args) {  
        // 创建一个StringBuilder对象并初始化  
        StringBuilder sb = new StringBuilder("Hello");  
          
        // 在索引为2的位置插入字符'W'  
        // 注意：索引从0开始，所以索引2实际上是字符串中的第三个字符之前  
        sb.insert(2, 'W');  
          
        // 打印结果  
        System.out.println(sb.toString()); // 输出：HeWllo  
    }  
}
```

# String

## join

```java
public static void main(String[] args) {
    List<String> strings = Arrays.asList("a","s");
    String mergedString;
    mergedString = String.join(", ", strings);
    System.out.println("合并字符串: " + mergedString);
}
```

流的写法	

```java
public static void main(String[] args) {
    List<String> strings = Arrays.asList("a","s");
    String mergedString;
    mergedString = strings.stream().collect(Collectors.joining(", "));
    System.out.println("合并字符串: " + mergedString);
}
```

## 转字符数组

```java
public class Main {  
    public static void main(String[] args) {  
        String str = "Hello, World!";  
        char[] charArray = str.toCharArray();  
  
        // 打印字符数组  
        for (char c : charArray) {  
            System.out.print(c + " ");  
        }  
    }  
}
```



# 转数组

```java
public int[][] reconstructQueue(int[][] people) {
    LinkedList<int[]> res = new LinkedList<>();
    Arrays.sort(people,(a, b)->{
        if (a[0] == b[0]) return a[1]-b[1]; //a[1]-b[1] >0就交换
        return b[1]-a[1];//b[1]-a[1]>0就交换
    });
    for (int[] list:people){
        res.add(list[1],list);
    }
    return res.toArray(new int[people.length][2]); //LinkedList 转数组
}
```

```java
LinkedList<int[]> res = new LinkedList<>();
res.add(new int[]{start, rightmostRightBound});
```

# xml

## 小于号

[xml](https://so.csdn.net/so/search?q=xml&spm=1001.2101.3001.7020)写sql语句时，
**大于号>可以被识别的，但是小于号<不会被解析**

```xml
select * from commodity where price &lt; #{maxMoney};
<!--小于号的写法-->
```

# 数组转hashset

```java
HashSet<Integer> hashSet = Arrays.stream(nums1)
                                 .boxed() // 将基本类型流转换为装箱流
                                 .collect(Collectors.toCollection(HashSet::new));
```

