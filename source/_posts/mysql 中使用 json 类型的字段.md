---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTR4YFJ2%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCpLyL1%2FV0iUD03ftgSvYCiweuLh0WUN1RiLvM4yH2pawIgHpNN4lWKXobcacB4zOWddtTTBZUceXu4%2FTDY0aaYxcMqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKbbJe8zykNIIDnboSrcA7p7cMEz3kbBAT1duCby3BIsWH3v%2BbMrsJjEwgLd%2BSJQuQ%2BksVQHFWT6zFVwUbSBIY9RbNqz7KratHpKL4%2BAGj5gEO%2B0Q%2FsXzuF4%2BA2mkBnD%2BQJsH47u3c88hcmWab7PXfDXzZUwPFyFPoglBHxG0CIUe5XqMzEI5tvtDGMk%2FD6d7o2y0Ao1atM3%2BotI4jY98JVfAuFeyP43s7ZmFi9KUT9%2FAXzVZ%2BNY1yXREj0k%2BC6VQvOvZKr4x5OdzYXPnJ%2FsDb%2BqYHO5jl3SD6P8NqBeV1luwXeIwZEf0bwt2e4y60dQ%2FI1qE0G20OZrcecx0GbsQS9s436tc36unifXCkZqGO7%2FxFlQkM8Wwi%2FehJvph7qFKMI8nwsMm%2F5u8otTTY7HxrmpvJCmKro2K1BUqmMIl%2B1DoOar3a61%2BtYWf9M1LWo5%2Fb%2BlYeAQIKhXpEHqVsfc14noU%2FZcYNbCx1pB3Tf7CuaaDBmnaEtdBNlJOw8J5iO6me7N301AwlTU2XxTHXS9zAukrhG0S93nvrv2peHJHO3O6D4sBdqMHTrLMQBzRrvfgFyXZ4%2FoObTA%2FGCFa1LSf9wa96RDB%2FRSvybTKhSAxzjMdCJ2KEs4aMIIhy7il193TGZ491Ca00MWe3JPMOSdpccGOqUBHNFyt1lg%2FP2gp5tBIVB688yQ9nAoxZEBeYO2BW7Abe2bK7tttARsmgfmKFbatlEXF3o4zupHUuaNPFlGM%2F%2BD0%2BO0aKFSC7XSBoitkMigIHSqsc6P3EDVJ%2BQc3can6OuOGUQbRHNyiVFRda7crjYZOMTnhKqOEdv7UR%2BUPwYV9IEnbYqjp3Vmx6x8Hj8gAWF%2Bmx2uQwQkFMGNbUa%2FWMAjSazD7ai4&X-Amz-Signature=7a4cf8e902df3fa6600c2dfd40254f0604c42fe7f2126c07269c71f4b7f97a52&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

