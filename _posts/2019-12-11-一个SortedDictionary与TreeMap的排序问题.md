---
layout: post
title: "一个SortedDictionary与TreeMap的排序问题"
date: 2019-12-11
excerpt: ""
tags: [Java,C#]
comments: true
---

​		之前我们的物流平台要对接找油网，找油网的接口采用了数据防篡改设计：

> 在POST请求中会传递三部分参数，一个是根据双方持有密钥签名之后的摘要
> （sign），一个是时间戳，一个是真实的参数数据。找油网平台接收到数据后，以同样
> 的算法进行签名，生成摘要，对比两者的摘要是否相同，如果不同，说明传递过程中发
> 生数据篡改。

其中，时间戳可以起到防止重放攻击的作用。

​		参数以键值对的形式存放在Map数据结构中，找油网在对参数签名之前使用java的TreeMap对参数进行排序，而我们的物流平台采用的是dotnet core平台开发，所以我使用了C#的SortedDictionary对参数进行排序。此时我的心理活动还是“so easy”，但是打脸的事情还是发生了--非法调用！！！

​		冷静冷静，应该是我签名的姿势不对，哪有这么快被数据篡改的。但是仔细的端详code也没发现哪里不对，所以还是老老实实进行一下debug吧。调试过程中发现使用java签名的结果和使用C#的结果不同，原因是TreeMap和SortedDictionary对参数的排序结果不同，请看以下实验：



```java
public class App {
    public static void main(String[] args) throws Exception {
        TreeMap<String, String> treeMap = new TreeMap<String, String>();
        treeMap.put("A", "xxx");
        treeMap.put("a", "xxx");
        treeMap.put("1", "xxx");
        for(Map.Entry<String, String> entry: treeMap.entrySet()) {
            System.out.println(entry.getKey() + entry.getValue());
        }
    }
}
/** 结果是：
1xxx
Axxx
axxx
 */
```



```java
namespace testSortedDictionary
{
    class Program
    {
        static void Main(string[] args)
        {
            var sortedDic = new SortedDictionary<string, string>();
            sortedDic.Add("A", "xxx");
            sortedDic.Add("a", "xxx");
            sortedDic.Add("1", "xxx");

            foreach (var item in sortedDic)
            {
                Console.WriteLine(item.Key + item.Value);
            }
        }
    }
}

/** 结果是：
1xxx
axxx
Axxx
*/
```



​		现在细心的童鞋已经发现了TreeMap和SortedDictionary对小写字母和大写字母的排序不同，TreeMap是采用ascii码大小进行排序的，ascii码大写字母的十进制数比小写字母的小，所以大写字母排在小写字母前，而SortedDictionary不是按ascii排序的，小写字母排在大写字母前。

​		至此，我们的问题终于找见原因了。那么怎么让C#的Dictionary按ascii码排序呢？请看以下代码：



```java
namespace testSortedDictionary
{
    class Program
    {
        static void Main(string[] args)
        {
            var sortedDic = new SortedDictionary<string, string>(new CustomComparer());
            sortedDic.Add("A", "xxx");
            sortedDic.Add("a", "xxx");
            sortedDic.Add("1", "xxx");

            foreach (var item in sortedDic)
            {
                Console.WriteLine(item.Key + item.Value);
            }
        }
    }

    class CustomComparer : IComparer<string>
    {
        public int Compare(string x, string y)
        {
            return string.CompareOrdinal(x, y);
        }
    }
}

/** 结果是：
1xxx
Axxx
axxx
*/
```

