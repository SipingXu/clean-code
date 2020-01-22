# 没有充分理由，不要捕获异常

这是最常犯的错误之一。基本原则是：如果您要捕获异常，则需要有充分的理由。如，以更友好的方式向外层报告预期的异常就是一个很好的理由。除非有充分的理由需要忽略一个异常，否则不允许使用空的`catch`块（忽略异常），或者使用 ```System.out.print```打印异常：

```java
  private static List<String> readLines(String fileName) {
    String line;
    ArrayList<String> file = new ArrayList<String>();
    try {    
      BufferedReader in = new BufferedReader(new FileReader(fileName));
      while ((line = in.readLine()) != null)
        file.add(line);
      in.close();
    } catch (Exception e){
      System.out.println(e);
      return null;
    }
    return file;
  }
```

catch 隐藏了重要的出错信息，重构后的代码如下所示：
```java
  private static List<String> readLines(String fileName) throws IOException {
    String line;
    List<String> file = new ArrayList<String>();

    BufferedReader in = new BufferedReader(new FileReader(fileName));
    while ((line = in.readLine()) != null)
      file.add(line);
    in.close();

    return file;
  }
```



# 不要声明实现类型

参数，变量，返回类型均使用接口，而不是具体实现。

```java
  private static ArrayList<String> readLines(String fileName) throws IOException {
    String line;
    ArrayList<String> file = new ArrayList<String>();

    BufferedReader in = new BufferedReader(new FileReader(fileName));
    while ((line = in.readLine()) != null)
      file.add(line);
    in.close();

    return file;
  }
```



重构后的代码

```java
  private static List<String> readLines(String fileName) throws IOException {
    String line;
    List<String> file = new ArrayList<String>();

    BufferedReader in = new BufferedReader(new FileReader(fileName));
    while ((line = in.readLine()) != null)
      file.add(line);
    in.close();

    return file;
  }
```



# 变量用时定义

阅读代码时最难的事情之一就是了解通过代码掌握一个变量值的变化过程。例如如下代码：

```java
  String serverName = null;
  // 50 lines that don't touch serverName
  serverName = getValueFromDB();
  // ...
```

这意味着阅读时，必须扫描50行才能查看其中是否 在任何地方使用了`tmp`。重构后的代码如下所示：

```java
  int serverName = getValueFromDB();
  // ..., code that uses serverName
```

注：另外一个原则就是，不要使用一个临时变量来保存不相关的值，这样也会破坏代码可读性。



# 不要保留不使用的返回值

这也与简化跟踪变量值的变化过程有关。如：

```java
	boolean present = serverCollection.remove（serverId）;
```

建议这样写：

```java
	serverCollection.remove（serverId）;
```

