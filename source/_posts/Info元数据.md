---
title: Info元数据
date: 2024-01-26 15:33:23
tags:
---

## 参数

| 1    | `fields`         | `list`         | 字段       | [info.fields配置](https://wiki.hcmcloud.cn/pages/viewpage.action?pageId=26869947) |
| ---- | ---------------- | -------------- | ---------- | :----------------------------------------------------------: |
| 2    | `actions`        | `list`         | 操作按钮   | info层很少配置个性化按钮一般都是会复写提交按钮、即复submit方法<img src="Info元数据\image-20240126154819602.png" alt="image-20240126154819602" style="zoom:80%;" /> |
| 3    | `groups`         | `list`         | 字段分组   | 用于给filed进行分组<img src="Info元数据\image-20240126154555025.png" alt="image-20240126154555025" style="zoom:80%;" /> |
| 4    | `description`    | `str|function` | 描述       | <img src="Info元数据\image-20240126154800945.png" alt="image-20240126154800945" style="zoom:80%;" /> |
| 5    | `form_relations` | `list`         | 字段值关系 |               用于处理字段值联动以及字段默认值               |

## form_relations

