# Form specify
    {
        "id": 1,
        "version": 1,
        "name": "表单名称",
        "workflow": {
            "created": {
                "on_load_exist": true,
                "on_validate_exist": true,
                "on_submit_exist": true,
            },
            "edited": {
                "on_load_exist": true,
                "on_validate_exist": true,
                "on_submit_exist": true,
            },
            "deleted": {
                "on_validate_exist": true,
                "on_delete_exist": true,
            },
        },
        "fields": 
        [
            {
                "name": "email",
                "display_name": "Email",
                "type": FieldType.Email,
                "desc": "",
                "sequence": 1,
                "workflow": {
                    "created": {
                       "on_input_exist": true,
                    },
                    "edited": {
                        "on_input_exist": true,
                        "on_update_exist": true,
                    },
                },
                "required": true,
                "readonly": false,
                "default": "",
            },
            ...
        ],
    }


# Form fields
* Single Line
* Multiple Line
* Number
* Decimal
* Phone
* Email
* Date
* Time
* Date-Time
* Dropdown
* Radio
* Checkbox
* File Upload
* Image Upload
* SubForm 向其它表单添加记录

        属性
        {
            "subform": "子表单名",
            "limit_maximum_entries": "0",
        }
* Single Lookup  查找其它表单的单条记录


        属性
        {
            "refform": "查找的表单名称"
        }

* Multiple Lookup 查找其它表单的多条记录


        属性
        {
            "refform": "查找的表单名称"
        }

# Form event

|Form Event                       | Created | Edited | Deleted |
|---------------------------------|---------|--------|---------|
|Field rules                      |   *     |   *    |         |
|Load of the form                 |   *     |   *    |         |
|User input of a field            |   *     |   *    |         |
|Validations on form submission   |   *     |   *    |         |
|Successful form submission       |   *     |   *    |         |
|Update of a field                |         |   *    |         |
|Validations on record deletion   |         |        |    *    |
|Successful record deletion       |         |        |    *    |


# Field rules
Hide fields, Show fields, Enable fields, Disable fields, Set field value.
在前端执行





# 接口列表

## 获取表单
### `GET` **/form/{name}**
> Path Parameters

参数名 | 说明
--    | --
name  | 表单名

> Response 

200: OK


    例子:
    {
        "model": 
        {
            "onload_exist": true,
            "fields": [
                {
                    "name": "email",
                    "display_name": "Email",
                    "type": FieldType.Email,
                    "desc": "",
                    "sequence": 1,
                    "on_change_exist": true,
                    "required": true,
                    "readonly": false,
                    "default": "",
                },
                {
                    "name": "mobile_number",
                    "display_name": "Mobile Number",
                    "type": FieldType.Phone,
                    "desc": "",
                    "sequence":2,
                    "on_change_exist": false,
                    "required": true,
                    "readonly": false,
                },
                {
                    "name": "Gender",
                    "display_name": "Gender",
                    "type": FieldType.Radio,
                    "desc": "",
                    "sequence": 3,
                    "on_change_exist": false,
                    "required": true,
                    "readonly": false,
                    "default": "Female",
                    "choices": [
                        "Male",
                        "Female",
                    ],
                }
            ],
            "rules": 
            [
                {
                    "condition":["Name", "GET_FIELD", "xx", "EQUALS"], 
                    "tasks": 
                    [
                        {
                            "action":"disable/enable/hide/show",
                            "field": "Email",
                        },
                        {
                            "action":"set",
                            "field":"Gender",
                            "value":"Female",
                        },
                    ]
                },
            ],
        },
    }

onload_exist: 表单被加载后，需要调用相应的workflow接口

on_change_exist: 字段被修改后，需要调用相应的workflow接口

condition: 条件表达式（后缀），当条件满足时，执行各个task

## 获取表单记录
### `GET` **/form/{name}/{recID}**
> Path Parameters

参数名 | 说明
--    | --
name  | 表单名
recID | 记录ID

> Query Parameters

参数名 | 说明
--    | --


> Response 

200: OK

    例子:
    {
        model:{},
        record: 
        {
            "email":{"value":"test@gmail.com"},
            "mobile_phone": {"value":"+8613800000000"},
            "gender": {"value":"Female"},
        }
    }

## 新增表单记录
### `POST` **/form/{name}/add**
> Path Parameters

参数名 | 说明
--    | --
name  | 表单名

> Post Body:

    例子：
    {
        "email": "test@gmail.com",
        "mobile_phone": "+8613800000000",
        "gender": "Female",
    }


> Response 

200: OK

    例子:
    {
        "id":"新增记录id",
        "successful_msg": "Successfully added a new record into form.",
        "redirect": "/form/{name}/{recID}",
    }

400: Error

    例子:
    {
        "submit_canceled": true,
        "alert": ["Can't add the record."],

        "error_no": 1,
        "error_msg": "Can't connect mysql database.",
    }

## 修改表单记录
### `PUT` **/form/{name}/{recID}** or `POST` **/form/{name}/{recID}/edit**
> Path Parameters

参数名 | 说明
--    | --
name  | 表单名
recID | 记录id

> Put Body:

    例子：
    {
        "email": "test@gmail.com",
        "mobile_phone": "+8613800000000",
        "gender": "Female",
    }

> Response 

200: OK

    例子:
    {
        "successful_msg": "Successfully added a new record into form.",
        "redirect": "/form/{name}/{recID}",
    }

400: Error

    例子:
    {
        "submit_canceled": true,
        "alert": ["Can't add the record."],

        "error_no": 1,
        "error_msg": "Can't connect mysql database.",
    }

## 删除表单记录
### `DELETE` **/form/{name}/{recID}** or `POST` **/form/{name}/{recID}/delete**

> Path Parameters

参数名 | 说明
--    | --
name  | 表单名
recID | 记录id

> Response 

200: OK

    {
        "successful_msg": "deleted successfully."
    }

400: Error

    例子:
    {
        "submit_canceled": true,
        "alert": ["Can't add the record."],

        "error_no": 1,
        "error_msg": "Can't connect mysql database.",
    }
    
## workflow 接口
### `POST` **/workflow/{name}?event={event}**
> Path Parameters

参数名 | 说明
--    | --
name  | 表单名

> Query Parameters

参数名 | 说明
--    | --
event | 事件名

> Post Body:

        例子：
        {
            "email": "test@gmail.com",
            "mobile_phone": "+8613800000000",
            "gender": "Female",
        }

> Response:

* 200: OK
    
        例子:
        {
            "alert": ["Email already exists."]
            "tasks": 
            [
                {
                    "action":"disable/enable/hide/show",
                    "field": "Email",
                },
                {
                    "action":"set",
                    "field":"Gender",
                    "value":"Female",
                },
            ]
        }

## report接口
### `GET` **/form/{name}/report/list/{report_name}**

### `GET` **/form/{name}/report/chart/{report_name}**

## `GET` **/form/{name}/report/table/{report_name}**

