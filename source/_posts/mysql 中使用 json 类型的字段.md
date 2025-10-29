---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4XIZ35G%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHajrup75yOxHNoFFBK7Tzutvys%2B9QVGkGQIho9ABUHEAiAz4hMv4g7nnbq5oRkDJFLY6KMWo1ZGR8946Ns5KCxW5yqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTktgDAEedjAVmirJKtwDpePfm4A313CFh6S582rOoZAWre2z7gYZWV%2BQWnZ1Syd8oN%2BC%2FX822TwDOTXMXW6RKJJj45YXbHiuh0O%2F%2FYUT1tp3vbkD669hnkMRR6Xx9WsDgppTXplTAPUpE7PQUiSGGbvVg9nnCmDaHQ5%2BWdebBljtydyaAz%2Bx3xltj1diWVIRcpVgjH05Wo2yvxjlczUvBI2C4D%2FafFYungKyU9qkBmyYiWyGZTA8mE6VmqjiXnJiWS2Y5aRAXM12r%2FKkaoioQGcezBKHTlrIXDznVHSV4%2FqZ3OCbUx%2BKDZbZJiUYKZiAknlKPyS9QZ8Hq1xjYfW1rO36%2BVf3m8948FfnPKCIs6%2Bg4vpdNpkZsOgXfZUYrFFti4iMjv92qgRahcv%2F3kKENyAOsSPgQA57dUeVvPi%2F%2FbsKPnW6rJi8L5STZ5qvnjE7hgUyk%2FxBeAEfEz3TrTQE8nRuD8DqP%2B1WDYfetT7W5MFipstAMgWPJhqsItWruNGXW43JWNZnXd2zQN%2Fc6PybDfsTVbbpvgNHoD3LrFxY%2BSRxK4mW62IJcdHB94%2FiaSZ3bs6UPuVhQZ1qdhgm3Rdgp6N7Ubl%2BCbDFTHP%2BDz40qIWhIo2ewSJQqHerh6NFAo4%2Fu5cw0T3A1pmMF1MwzZyJyAY6pgF3K5UftpWKGuJdYu5CHFBC6bOTK0zWegBmrj3Jlfso%2FrHBO6XWzNFV%2FlQf2wmMc8zt4Ffewe9L7lUQkPDsrkxMZs0Q%2Bwo3WF5C6UdXanRxTkAkGoqE96xRPgBrDNR7FMQDjmpdOszVjc86lqzzGbhgI4RaJik7cNFTRQA4wBmqSMFw0tmKeer6ERE25CZjuo1VBkfwn5pJm8NXpoort1Q46cFynzc%2B&X-Amz-Signature=df2d861bc828c7f0c6ac3cf175fe2351f9639eaa5d8802d47e24e9c830058512&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

