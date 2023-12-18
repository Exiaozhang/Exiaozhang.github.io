---
title: .net DI and IOC design mode
date: 2023-12-18 15:12:24
tags: ['.net','C#']
categories: ['编程','学习']
---

# 面向对象的设计模式

## 依赖倒置(DIP Dependency Inversion Principle)

面向对象编程的六大基本原则。  

目的解耦，使高层次的模块不依赖于低层次的模块，低层次的模块依赖于高层次的模块。  

该原则规定:

- 高层次的模块不应该依赖低层次模块，二者都应该依赖抽象接口。

- 抽象接口不依赖于具体实现么人具体实现应该依赖抽象接口。

示例

```csharp
public SendingEmail{
    public void Send(string message)
    {
        ## do something
    }
}


public Order{
    public SendingEmail sendEmail;
    Order(string message)
    {
        //Order do something
        if(sendEmail!=null)
            sendEmail.send(message)
    }
}
```

上述代码中实现了创建一个Order示例会发送Email的场景，但是如果场景中需要发送短信，则必须要创建新的类型。十分复杂所以要解耦，让高层代码不依赖于底层的具体实现。  

为了实现上述的原则，提出了一下两种设计模式。

# 控制反转(IOC Inversion of Contorl)

控制反转使高层模块依赖于抽象，而不是底层模块的具体实现。  

上述例子 IOC 的具体实现方式:  

创建一个新的接口

```csharp
public interface ICustomerCommunication{
 void Send(string message);
}
```

使发送邮件和短信的类继承此接口，当Order中需要发送短信时只需要实例化对应的类调用接口中的函数即可。这种方法还是有一定的耦合。

# 依赖注入(DI Depeondency Injection)

常用的依赖注入有三种方法

- 构造函数注入

- 方法注入

- 属性注入

实现方式就是创建新的接口，使底层模块继承此接口，在构造函数，方法，属性中传入对应的底层模块的实现，实现完全解耦。
