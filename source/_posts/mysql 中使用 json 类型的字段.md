---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQQD6K37%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T160109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIB%2BZ8N8BQGMs%2BD6rUktrJ1ZcYOaJ09SAo%2Bn8%2BSaHW16kAiBXtU4lAQhCvqCvLBK5NEjQoLT2HNq%2BnqwkkqhIf%2BI8dSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1AmCIl47vybo3IzcKtwD1%2B1FdqOsF77DoEtR7jqB86wzvLtOXMyK0IBUwPyZug%2BxPdILEVXg72g7sWbkosYlWskOFFvImwaagTcuZgXfmlqWDwoaY%2BvL0hsI8NRk2leDks9Z%2FHTFJg69OB3Usj19M9DfUZKSy47PbkCvJ4ci2E8l2YIm1CFw0XQFVmg7rgSOqy2W5x0NYAbk%2FKDzt%2FunM4LLmqORjkSt0EA2kqKjg8nBErg%2B6%2FJ1ujHAcBmKgBgMVoydMXK31RqJrz1ypVxoPr6KjUd8YXCdX5Q5TE5Mz%2FyfL3pB5VQiR9UHAK7wtVKH5koE12lXrjStvuwFfWQZyTeD4gp77hxDgIztqxOBg%2FxHaDO%2B1IPKHUUPK0Q7wJEAqTRCRBMvQSbwIczB3Sf%2BhMdCLEuY553xHFFnehEWSCw2kpiwK0GzawIYKLdLj%2BHb%2FhMMEssCQNlxSpFr9GyoIuoC2g1nfX1XNVKwCRl8ochG7ca3QEvcrxLyFNs%2FIUYA9pDWOAKvJowKQ4e7HfJirVbxhJad08CNBizCqIAFji0q51QTHGcXZDyMbTVsFaox52T7Owx2vdIhfKBZ4Y9LV0h%2BraWpjC8hv3tFByFXGpffI98P0T7ldne%2BEbBSEF8QsuHawSiMTZp5Y7Iw1dqkxwY6pgFopKtqdYeYe0ybe07qxQede%2BnTsr8onVIDvIT3hCeQIGPumrGys%2FNdgBfWLBq68u24yhpUPmPZE4EBM8DryvURuDI%2BHqx2W4PmeEpriMKV%2BddiiWIWZhMogpjPO7wSqKr4Om7mRncMXjYIaMfZrkK%2B6dyXMtYXDZWxiWDwYQewW1mGuSlN3uGSGT48x65zJnb0okYPSavuRLn3voU%2Bri7SLYtkUzrj&X-Amz-Signature=7fc29a4094c76afbe68eb691f4b7355a89ea92a617d22c0758055bb43b7dd5ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

