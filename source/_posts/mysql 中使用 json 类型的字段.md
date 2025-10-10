---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVODHFDA%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJIMEYCIQDJFbq6J%2Fr3wT1Lw7fzamNUwlgLEeQ3RMYKuINnLEh0tAIhAJxykXW9qi0RY5d6LaMRXZCzvgcfmUhzIKqld7b9Cua0KogECOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzCTFqXHThoJEG1F3gq3AMzdBmKf6ZJkx%2FCDhoCrPIX8BHctRJLpiUkNp0aGTrtqXu7zLR8gzkuyCFW%2Bg%2FmdL0rhD0lMWQtjvjrclVo66mazuOa%2FnAMftfgPpW48IWEuMGBpbS%2FczjV8zfoAri5jmoLOCFbWHJItRT5y%2B2ioQNlxfXJOlwsTP1t892I61Fzc1PIlcUq2%2F%2F0hihLrJt2%2BOO3Oe44gsqivNdlSxKy1WFtX8tsm8kSQzKGuuhrPSfA9gg4P9rV3wznT90jbABNTeREwoWgVTEORpiwHNBBOUfbvECZJ7lw%2B568EgQAUQ%2BRqKjb5JjL3XpC%2F8QvnvTSvBHApcf3i%2F3zK7xE02NWLEw2biO5L%2B1YT%2B7pXczJd6TqTcseT%2FXTDlD6w8UOA51evMjzandRM7eV4XnW5Ixmjzs4usdFxTZIpBGLrtOb6JeAK0UBinlOZB%2BmTkFyw9DsGbPGbL718nee3WhM7GUID%2FgQbXwowLJSKlZq%2BKJgNJhzxd23WV6dfwoNesu0P8JX9c33QR0C5iDk9JErqOykS6pQptjWjyYHgkM5WFAeMY%2BrKB1n%2FKydvUCtl%2Fvs5BBE7b3AqCHEhxH6VZFlURImpA9X9SLN48gpjRW4QNQEDTqkLgmzWd0HglLGOf29tzCJnKLHBjqkAQNocUfKdMu2SBSS06G7KGP7Me5zNVPh1FEMsomV%2FlUcKMKvU81U8pFJrNU4nf3LbvRqONwoq9bn3NLP1NXxqhixw5ItwEVFMgO9sbxuRbl4dVJg3XAQ3zN2SzUo%2FaGY%2BY1%2B04b15oN1JUTFSMl5htwGSRrlNDdJ1JjnHsY%2BJge8dy2p3NXd1C9%2BOa6vBx9lE6w6tgqi7JTsLLrlajBuOoGzsfGW&X-Amz-Signature=16606d198a8b17fec79596a898023bfbecfe4d54bb1dd7954660633741dfce76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

