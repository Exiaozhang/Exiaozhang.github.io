---
title: ET框架的消息通信流程
date: 2024-02-28 23:13:04
tags:
---

# 消息通信流程

ET框架中是使用Session和Channel实现的消息通信  

Session是会话，比如打电话，从拨号到挂断这就是一个Session  

Channel是通道,我的理解是比如打电话时，Channel表示是使用联通信号或者是移动信号或者是电信信号  

### Session的Send函数

```csharp
		public void Send(IMessage message)
		{
			OpcodeTypeComponent opcodeTypeComponent = this.Network.Entity.GetComponent<OpcodeTypeComponent>();
			ushort opcode = opcodeTypeComponent.GetOpcode(message.GetType());
			
			Send(opcode, message);
		}
```

通过消息类型判断opcode->得到channel的stream，序列化message和opcode将其写入到channel的stream中->调用Session的channel的Send函数发送消息

```csharp
public override void Send(MemoryStream stream){}
```

## Channel的Send函数

得到Channel的Service->根据Service的PacketSizeLength判断包体长度->设置Channel的sendBuffer长度->将stream写入到sendBuffer->调用Service的MarkNeedStartSend函数

```csharp
public void MarkNeedStartSend(long id){}
```

## Service的MarkNeedStartSend函数

将channel的id写入到Service中的HashSet needStartSendChannel中

在Service的Update生命周期中会，根据needStartSendChannel判断channel是否有待发送的信息，如果此信息待发送则调用channel的StartSend()

```csharp
public void StartSend()
```

## Channel的StartSend函数

判断连接状态->判断消息是否为空->设置发送状态->
