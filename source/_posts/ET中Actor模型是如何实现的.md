---
title: ET中Actor模型是如何实现的
date: 2024-02-27 23:27:04
tags:
---

# Actor模型

### 非Lcation

首先需要使用ComponentFactory创建一个具有Id的Entity，其中Id是IdGenerater类中GenerateId函数通过AppId(服务器的类型)、时间、值生成的  

给Entity添加MialBoxComponent   

```csharp
Unit unit = ComponentFactory.CreateWithId<Unit>(IdGenerater.GenerateId());
unit.AddComponent<MailBoxComponent>();
```

获取Scene的ActorMessageSenderComponent组件，使用Get函数利用ActorId获取对应的ActorMessageSender发送Message
