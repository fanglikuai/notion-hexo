---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HWITDHK%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA8cutE8BAo%2BtX4Jf0KCkVB5BUYe1Nwt%2FaWfldOWVbv3AiEA27Kf9ewKYpHQoVzl6H1QHD9zwB%2BzUcDPU%2BWGxiSS45Yq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDLKsy0sFQP%2B30oXguCrcAw9A4YzyfiW3Rsj7Iszqn4lgXjksan0ORJZthODpjvPWJEUxFl2Q7SVbWmwa%2BbGQdjIOxW9psV0zb%2BXo0bndLy16WyzzIN6KMXW%2FB44M%2FBf20jvX07tTLXUjlUn0IHu5%2BVDQbvRsVAbYyQqu0F1xG0FnIZVqpcq69iiRvciIU5NFoR0KfPoZxcSJfijaIGCF3Vp2kZYYFxzo1BEhXpzVxHnT1CsXEe6SREIPqJsWq7radqLWf6ZFIaskmXJWGJyJqpPUJJXv90rPBcwJ1WmZK%2BPvQRxJRThnKwJzXHNLzho2WlDIRCaG6ntoKiansaLH1GUH0qDWbdeZziq4ZlWR3QR7DuXSzR5BnGHPzBwovafGv0FP5SnjPQzmTgJDRN56bl7tJRbqTZ2ZOPqqNdR3EycG6Fhc3ZNFnAgYi4DXT3WYK9Y67cXG0T3Fs2eoZHwM6qtvwD2RL%2FpIMfSsXWLQvGqaBqI23yM8QLyAJJhe3NPWaJaoEeAHAuXLhDJsh2WNnQKDI%2BipM%2FzigMptOjDTm6Ja8hU4saujhVSRWB9UIrkSiN6v1Qfi305wZzaouiKSxwbl3yodTmn0C0vcy85gNuFJGt5jTHW2daheKgXaYPEYpi75%2BDI7KUQIxbZ%2FMJLEu8cGOqUBWqe4h0vRKPtrL10ZUntb5TPHK71jhrrO3UUJ6GWGkRvIwL5%2FSWKbKs5efCIc3WFA6wfoeubhWva8gxbceasv4%2BKNX%2B7AL9aKLy%2B4tq%2FdYNijypC%2FgRgeFzw79b2LMkx2oc8foVXQmDZJhF9JlxbLh8OX8d4ChiAUGwv304EHkoDoNDP%2BqYUh4cchTrSJsKhX7sXNr7FdJ3quFvE128%2F2FmbeRH9Z&X-Amz-Signature=9f1df6bf0ede79469d8bf04d0325337a9961a74634cdd0ff4670a30b22c99b75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

