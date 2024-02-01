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

### 功能背景

流程发起界面、常常会表单中默认带出信息、比如员工自助转正流程、要带出转正人的基本信息、单位部门组织信息等

就需要用到form_relations。它主要是用来处理 表单字段值联动以及字段默认值。

### 属性介绍

配置在info层、主要用到

value_onchange属性、根据一个字段值变化自动赋值另外一些字段

default属性 初始化表单时能自动赋值

expression：执行一些预制好公式给对应变化的key字段赋值

```json
{"form_relations": [{
      "key": "emp_id",                        //      目标key
      "default": {                            //      设置默认值相关
          "default_value":"123",              //      默认值 字符串
          "expression": "EMP().get('id')"     //      默认值表达式
      },
      "value_onchange": [{                    //      字段值联动相关
          "key": "position_id",               //      emp_id发生改变时,"position_id"字段发生改变
          "expression": "EMPPRIMARYJOB(emp_id).get('position_id')"    //"position_id"字段的取值方式
      },{
          "key": "number",                //          emp_id发生改变时,"number"字段发生改变
          "expression": "EMP(emp_id).get('number')"       //"number"字段的取值方式.
      }]
  }, {
      "key": "position_id",
      "value_onchange": [{
          "key": "depart_id",
          "force_change": true,               //      编辑时初次赋值是否变化  用于展示字段
          "expression": "POSITION(position_id).get('parent').get('id')"
      }]
  }]}
```

###常见案例

1. 最常见根据人的id带出人的任职，最高教育经历，(聘任为是)专业技术职务信息

   ```json
   
               "expression": "emp_data.get('number')",
               "key": "number"
           },
           {
               "expression": "EMPPRIMARYJOB(employee_id=employee_id)",
               "key": "cur_job"
           }, {
               "expression": "cur_job['position_id']",
               "key": "position_id"
           }, {
               "expression": "cur_job['department_id']",
               "key": "department_id"
           }, {
               "expression": "cur_job['unit_id']",
               "key": "unit_id"
           }, {
               "expression": "cur_job['job_grade_id']",
               "key": "job_grade_id"
           }, {
               "expression": "cur_job['job_step_id']",
               "key": "job_step_id"
           }, {
               "expression": "TODAY()",
               "key": "date"
           }, {
               "expression": "HIGEDU(employee_id).get('education')",
               "key": "education"
           }, {
               "expression": "MODELGET('TechnicalSkills',is_recruit=True,employee_id=employee_id).technical_skills_name",
               "key": "technical_skills_name"
           }],
       "key": "employee_id"
   }]}
   ```

   其中：employee_id是人id的key，默认当前登入人，它的变化会掉value_change事件，会带出基本信息（人员编码number）

   任职信息cur_job (unit_id单位，departmen_id部门，position_id岗位，job_grade_id职等,job_step_id职级),

   同时带出填报的时间字段date，最高教育经历，技术职务等于是的聘任信息

   需要注意：cur_job，emp_data为中间变量，赋值情况过多，需要找中间变量过渡存储下

2. 调用云函数

   ```json
   {
   "expression": "PLUGIN('云函数名称XXXX',{})",
    
   "key": "form_default_value"
   }
   ```

3. 调API

   ```json
   {
   "expression": "API('云函数名称',{'model':'EmployeeAction'参数XXXX等等})",
   "key": "form_default_value"
   }
   ```

4. 字段属性控制

   流程中想配置当job_name字段有值时job_category必填，需要在info层job_category字段的options里配置required

   示例：  "required": "=function(){return FORM().data.job_name?true:false}"

   其中想在流程子表中拿到主表的配置是：BASE_FORM().data.对应的key


## fields

| No   | key         | 属性说明                              | 备注                                                         |
| :--- | :---------- | :------------------------------------ | :----------------------------------------------------------- |
| No   | key         | 属性说明                              | 备注                                                         |
| 1    | `key`       | key值                                 |                                                              |
| 2    | `label`     | 显示字段名称                          |                                                              |
| 4    | `component` | 组件                                  | fileds自动配置的情况 filters根据检索需要自行配置(component组件,后续会有文档说明) |
| 5    | `options`   | 字段配置                              | 组件公共属性说明: `hide` :是否隐藏 `singleLine` :是否单行显示,默认两行 `readOnly` :是否只读 `placeholder` :提示内容 `showTitle` :是否显示label `require` : 是否必填`tip` : 提示信息`force_assign`: 在编辑模式时，执行默认值赋值逻辑，仅hc-text-area, hc-sys-link, hc-input-datetime有效，需谨慎使用。`split`: 在该项后添加用于分组的分割线（限移动端）`width: "col-6";` (可配置`col-1`至`col-12`),number_increment_rule": {           "key": "train_demand", # 编码规则的规则名           "type": "save"  # save: 保存时创建编码, create: 打开新增页面的时候自动创建带出编码         }(不分场景，仅在ｉｎｆｏ层配置有效)各个组件有自己的参数 详情请看具体组件的文档 |
|      | `sequence`  | 顺序号                                |                                                              |
|      | `state`     | 已弃用（请在对应的state元数据中配置） | 显示该字段的场景列表                                         |





   
