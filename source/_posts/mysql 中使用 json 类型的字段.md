---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSI65AOO%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3zoBsk%2B1dOjnog%2Fa%2FH7Hdc6%2BJf%2FAEhtXM7xrKsIf%2FjAIgM%2Blr63mI387oggPq8Kj%2FFPz1xY2mlw0xOd7wQICly7cq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDGeWAsAX2VGfV7v1AyrcA3IlOpIODm7WeuxiXWTM4P8RviQOu57nHHWZb5IlGs0L1ESluYeaOgAuWmW9Pg0zDG8CJrCaM%2B1bxhRiuLzLgz3n7ocizJbqPMAYUBwvuAGS7%2B2HVC3fsO%2FS8QAmK1RIUp279fGbxXVD3rNFsIEWksf1Acx6Kivg2qS6txXOepdGaCvV87yyRiIDDsrjGeir8KOejF9i2uvFkXgsgFgJtGjlFSBwQAqlpbjqvz7aEuQ8yfhHfcNDvcg8Zd8ykctLri1vNrs6gFfWKE0Fl%2BnU9rfR0%2B3s8%2FZd8dhhWyoCjPdEteToZGLA276PrguUCUYeeTSZAJSrpssaJt2arQDXgas1jlJsPb63sb7c9UnAiCHanEXA64k5wgPeDPtqMWP2wemnfAbQSDU9crOe7uiRuPRCM4cPOeAhJ0uJpDa6kievwxJWiXhsJ0L1qxEpk7ANlkN0xnK4%2Fse05bruDwsYs0dx2Z2BslUaYEaXDDijhM4DBDnpOQGorU%2FnmzT4Q%2FYDBrRZjZtv4MAR5z0efQAqyJzgCe6ocTYSViay7SpTJmU2IkfYprg9KcmVtEaSNcctxWTTc8fpXPwVBGY2exkaZ9W0rHzKeiD8SqpECLfEXw%2FfhXX2%2FFYf48f0wCmNMNSel8kGOqUB%2F7YybjekT%2FEXtVAPqoEol3U1SGFzHI4oYJFX00e%2FSCAwHWmf7atyh%2FBwnXDS8vO9H5A6sp%2Bek4fFGoJz9xfASLrtgIgae1gULUA4%2BPLRS6p2KiYwc%2FdXscDfF6iFW5N9nYXUwVMNnwH0i0hMrV9AroZXfzLBgg3bUZj29jm%2FZDE73EF%2BfhN7uAGiJOFNmEIovo2Ph6g9ygOwtxERYtHc%2F7wxng5j&X-Amz-Signature=dcacef50a4afc0b1eed291e9d762c7d26b952201813d0e677c6af2cc04ecdb89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

