---
title: C#_lambda
date: 2024-01-11 22:44:54
tags:
---

最近我发向一个问题，C#中lambda表达式中消息体中的字段是如何传递的。这种其实不是字段的传递而是Closures

# 什么是closures

简单来说，closures允许我们捕获一些行为，传递给其他的对象，并且在其他的对象中仍然能获取到捕获行为第一次被声明的上下文。访问原始上下文的能力是将闭包与普通对象区分开来的地方。说起来挺难其实很简单，代码奉上,下面两个代码是相等的

不使用closures

```csharp
static void Main(string[] args)
{
    var inc = GetAFunc();
    Console.WriteLine(inc(5));
    Console.WriteLine(inc(6));
}

public static Func<int,int> GetAFunc()
{
    var myVar = 1;
    Func<int, int> inc = delegate(int var1)
                            {
                                myVar = myVar + 1;
                                return var1 + myVar;
                            };
    return inc;
}
```

使用closures

```csharp
static void Main(string[] args)
{
    Console.WriteLine(inc(5));
    Console.WriteLine(inc(6));
}
    
    var myVar = 1;
    Func<int, int> inc = delegate(int var1)
{
    myVar = myVar + 1;
    return var1 + myVar;
};
```
