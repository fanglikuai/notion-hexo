---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5L2GAT%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIQDwob59HAtk7OE3ZifVLoRZfmhPaitXAjHSuTExPo%2BTewIgDWhuzlUsJiG8IVHOFkuBu4eHcqZ21MzcT62XCij5Q5YqiAQI9v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCGwqWxJj8NpTLMKLircAwxHLY7M3%2B5wisboI%2BvJ7mivXMO8KDmnRQhMMugIZkaXDhf6lMiIky4tA0E32lMKQejndEWqCnkaa4NiAJ2danBtaZA%2Fxtwe2alqlrj6TYxZHTDxsMwX3cwTh9X4%2FMOoB1CMkDbRKDlIhJseCOixOwX%2BI80ND1LoH1P%2FSdkm9azYjnTCZIv9sqJwF1iPB7V%2BDs%2BFdYvu4IdtrCzTniNOGsyCw4d3SKHLw%2BWBIbAZ9DmV1YMLq9hdhy8J%2F0tdE5d%2F39c20ce1U7wdltxj%2F5NR7sLAaJleDkYAzfjtkL5TO7dfvoDePb4ybEWZEtctOKmPramc0UNXttgaIRjSRIg8L5ebrl0KwqdqDJ8oySOsjCYOyjDYqYmBky%2BuMG46t1cWSy8Jc5gl5B2N99hFQwbrDDR1eLpjCWTrDHjBSGJMpyzCZZEzrijmq4PwPEPHbdnL4baLi7R7jxTwvs49clXnRmsutVGCpOXhuld6S%2FPvHBrZiokUDJL8i%2BvXrY8UJ7nnIg5C8otpqXXzo53%2Br%2BqE9g6EmwWd8iQ%2BBI0ErG2MiECjwHMn6OlgfWXUBd7VWDlfFARWs7EEB2gE4hpmtCHohiQfw7lSK2QLtoopveu8xXOY7gO1RJengrS5yWByMJaqj8gGOqUBJ5n6t%2BXIEXwZhwHCSEoxWgxLNYHS5N%2Bak6t0wCrZQOvYWAy64Qz8c3S%2BV3BnT%2BoBRQVTNNamzCMLnYQwz4RMQ8nDHaUQdHWM7YAvpZEWVit7hLT%2FceheObCrkHvtMqMRf1moXp%2BuzNw0e5hyviztF9ZrxxL8haukXc0cQbPnsiyZ%2BAubZtKC1zfvSz9VP1iRjJPQrILqd9zkm2wu0TB4Cl0IWyBA&X-Amz-Signature=69027ed6ea79f99a8e4a6453d5aba129a4046c60085226277069e9613e601f81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

