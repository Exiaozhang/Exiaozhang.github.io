---
ztitle: ET中Actor模型是如何实现的
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

ActorMessageSenderComponent的Get函数会通过IdGenerater的GetAppId反向解析ActorId中的AppId，在StartConfig中根据AppId获取对应服务器IP，创建并返回ActorMessageSender结构体  

ActorMessageSender调用拓展函数Sende，创建或者获取对应IP地址的Session，给消息ActorId赋值，使用Session的Send发送对应的Message  



```csharp
ActorMessageSenderComponent actorLocationSenderComponent = Game.Scene.GetComponent<ActorMessageSenderComponent>();
ActorMessageSender actorMessageSender = actorLocationSenderComponent.Get(ActorId);
actorMessageSender.Send(message);
```

```csharp
public ActorMessageSender(long actorId, IPEndPoint address)
	{
		this.ActorId = actorId;
		this.Address = address;
	}
```

