---
ztitle: ET中Actor模型是如何实现的
date: 2024-02-27 23:27:04
tags:
---

# Actor模型

### 非Location

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

如果接收的实体是网关和客户端连接的Session  

Session上添加MailboxType.GateSession类型的MailBoxComponent和对应类行的MailboxGateSessionHandler,MailboxGateSessionHandler会把消息内容转发到客户端  

session收到消息后回判断是否为Actor消息，如果是则会进行转发通过MailboxDispatcherComponent找到对应的Handlder进行处理

```csharp
session.AddComponent<MailBoxComponent, String>(MailboxType.GateSession);
```

```csharp
	[MailboxHandler(AppType.Gate, MailboxType.GateSession)]
	public class MailboxGateSessionHandler : IMailboxHandler
	{
		public async ETTask Handle(Session session, Entity entity, object actorMessage)
		{
			try
			{
				IActorMessage iActorMessage = actorMessage as IActorMessage;
				// 发送给客户端
				Session clientSession = entity as Session;
				iActorMessage.ActorId = 0;
				clientSession.Send(iActorMessage);
				await ETTask.CompletedTask;
			}
			catch (Exception e)
			{
				Log.Error(e);
			}
		}
	}
```

```csharp
	public class MailBoxComponent: Component
	{
		// Mailbox的类型
		public string MailboxType;

		public override void Dispose()
		{
			if (this.IsDisposed)
			{
				return;
			}

			base.Dispose();
		}
	}
```

## Location

需要进行Location的调用MailBoxComponent的AddLoaction的拓展函数  

在AddLoaction会把Entity的Id和InstanceId发送到Location的服务器中的LocationComponent组件中的字典进行保存  

如果发送Loaction消息则会根据Entity的Id到Location的服务器中的LocationComponent组件中进行查询最新的InstanceId发送,剩下的流程和非Location的Actor消息发送接收流程一样

```
unit.AddComponent<MailBoxComponent>().AddLocation();
```

