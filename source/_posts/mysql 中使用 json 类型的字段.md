---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAOSTC5Q%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQC0PoOTundYwI5qc0d%2B1zBcFH0ci4XpMnpmceV8%2Bbp0iAIgXpbZMYwa%2BLUD0903kOO3BvCZQz0bQZljD3XiqJHEs7MqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2Fj1duEUBTypg1LuircA9U6LlXPz%2B6E8A7edILlvXMwG2AcSoFbEKKWYRhubwhNyBFBx%2BndWTATRARZ0zHHv5lkm1OJbRK%2FhK9H9pgB5aB0EfjGYyG%2FOOdzq8TOK55gqpyusadFwlSz0EGtymA5rIuvEMKrL77Vr%2BhtG9RISvG6bm6DUuaNUBezS142SbVmUmKUyyRfozUeJ4ObyuRI%2FW%2BIw8eg%2Fgu1GzQKgC5yDYamwJstDzvfSAP8ji9DrFjw5mJU6T1lYCvqpjWCYjPNCWD80icq7KpAm7OI%2BtbzM6RJstAN%2FHz8Ua6RB%2F4FeKPRvPjo0ErxsJweFAFvFDTOwHHJM3n7jE8EylE41VnRwZLaNSrfUClyp%2B%2B%2BZIZ0JDlyUyMCTWqr%2F6oVtnhmiPfblfIfsXNDjBszgTsCoa83xnWe9MEzeufQbPdmtsErq8RNr3dz4Nf58aoDCHT0u6d1e1pEtmWofGcLAt3tRt80dyRY1Y5Rh3kQBqp%2FN%2BaxKLDvbpfO0KNfQXZYIqZEF64aY59OdrYnHUh7oMIZxijc5CiIjKcYFaHRwnIt4HmzdRBpDcMrlUtW0ZtRXFzkvSkRs412UPlGcIU0O9KAMQxmXIjWpz9ZBR4zxJJ69nnxfZbMHF0m8aTszR4P%2FEAlMJGApscGOqUBgyV4UTdAPDys8NIedTDaeOzGeCMUVnPzjQ5HRZQvnTTeaHn9IqXWRHxZE7ICkOo4CWiQOgjt9Afp7nVxw3dgPUPPXtU8B8qMdyuTFjToBXFSKLcpDA2oiV6zORoTYpgzt65WLdcXCjo%2FJgUAQtdo1KjTCf0080TXAxfDDxVkBg2uIVdemoQcMuIxpprnXqZfvDyB08c5%2BOpLoFRU0b9j1VuwNE8m&X-Amz-Signature=ba9cbb302c28cca9205fc34ef480e24e4e9f4ac2b3308de53557c43d05fe80e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

