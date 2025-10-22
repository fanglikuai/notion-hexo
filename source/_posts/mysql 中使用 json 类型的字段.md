---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZYYGQF7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T060139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDeeZMPUvh7EDWZlctb9rVrhzQo2yVVa2r5o1a6Zrr97wIhAKNYLnOcjeCGs%2F9HpPZm7QBGYC3N9p%2B6KHt6ekTkFXTgKv8DCCMQABoMNjM3NDIzMTgzODA1IgyRzb5L%2BIbC8%2BTtuPwq3APmIYXXo44R%2F%2BPBe4YDzUJFJRxMpC1K76ugLDY4B83Y0btvN2IyJLDbmBNN3hbVvmHWbYzifPGcLRW5Khe1BNtkguOIQO2feN4bbvnLjeXbLZVStdMBngAo5XshjuPiHgWKCZIJky7IUCYLCKvcNjYmxplAjCYmQ8N%2BNMMt0rg0hCeLLfKArtzSWboTRk86UaoPSFV%2BP8WTGivlE3qe6evGJbGINJkBsc8P3LICv6ZX6STE5FmWrMOBZW3cM8hTMVo2OqPL7SHP30GlZbgT6wS27pK2bP5M%2BGMg%2FvFkaPfU4Qz8s%2BTkqdqsBcZX9qWojb4tCY8Z4neFnOCxDhw4upovJpRoUtVvD6e%2FQh9BQWG5tZanwSaQOY5N05RfCY5BemWTQ2YOkhmYBBc0n8TFnKtE0FODGZHGQRwy3WH7u8selSD6AWKhQqvFnIP4NlFaqfmm1UBRF4usHNRDyfiwCEHKyMdLgdW5afs3H8tfzVemLgcK6ylO6cUBMhiA8XZN2P3nEXXM8ZyJJ%2BDt2lGF9WZHi8WwsWiSmiR7IeT4S4eZsxeofL5aQN1eIO3o2V%2Byzks%2Fe2IKsGbAopsXnL6LwrXR%2BKiX4mYxNqldmw8G3kM4T%2FR8kYIPPzOpFdWHqzDQ5%2BDHBjqkAX%2FLCb%2BSp%2BAb%2Bpj6z5osPMuy4oyuoDW9jhENbnCsPeIasw8Od2h17l7v5H9%2FeCCWwdCdE02FRlMQaZ0%2BDm0Gh078RKgB4xnPdi3M%2BDJxWGnzleluX04csYYkfyo%2BYlARHACsjreShKBrybJbG6zGVRBtQJGNa4rDA0oqvsBelzllxAadYpEwUWBkIfwfOybuRd0dT1OYPfLqX7BVG%2Fu8eRPymzc%2F&X-Amz-Signature=de71d3f1b27a4750a1d7bc853685576021c5c5cca03d367a16f3c9c2f2a379fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

