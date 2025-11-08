---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7COQK6%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIFAfx1qVqOzD4vqznSgQCvcCE5Gxu9XDcBgjx1dnNUrhAiABOy2Iaj2xWhLF85pA5hZYGwKtgfxIIrrQKR7ZtvOO5SqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMGQAtBONpdKD6ChJSKtwDNB%2B3ox%2BSaejV%2FSmefzZmTXX86L1bYVNIBHX8EjMpHPnmCPXglOUKZqcd9UHANPOhydpGmNE185WKY53yIcEDOLUdJlpvnic2TYxyJ7v8GmAUWf6iUiBN5N2dW%2FyCn5nI6XKeL13%2BAHi91ivx%2FMFRXUltIN0VcTV0V0%2BMmAMcJdnnDNg7tKc1TDlJab0w0m91Q07fIkgzRRyQAVqpbYe562PDoLOiCCW%2Ba8dRg4Yge4zn0j7w8M9VGNrAwojdRII9WkUu7sm%2BtFChn0Yng1cD0eoF%2BmEU%2BoDX4gfrAvWShaARjhytfAONST4B97B98XkLBMISPclCLil74TYzkv%2F3wsdTO8vSoxots8zvdQ4ZlsLowbH5NZ3uEnYNzTEqOn9767pee1aAdPvUEjmQFM46K9usg5wjZ5%2B%2Bn0Km9u9j%2FCYt16iIqdxl3eo%2Betd4fZ96Q1MF2c2wppsjKRiwegTiE9oNHfvtXDOyQUuwJa%2BhjJuQo9foIB1No9CN3FW0sNYTwre2URcYf7vEcj3B52pXd0LbQLCynREWsb%2Fls9IA1dU0fWjxGXfHk3wCt8cyKpwdy1bNzkP%2BfaOligdT3wQETJ6Ic4jiizAI3SQ5w6DCAW%2Ff2PyjxhZ5G%2Bq4FqUwre67yAY6pgHQaZ7pIadooyVPBskgaY%2FXGlTmvo9TjlFHhEPYwGp%2BRJZlDDG%2BBVqK24WvkVHjsdGrtpJwM2imT9Gn6b8hESgF9IX0pvlxLVr0fMVi75JWfZJ2ML7jlMGkQzRQvRHb4HFXUpyPCSUjCc0TvnSBsOs8PrpwCCgqD40ujCRkmRSUXZPhy4NZrCln32TAhH8%2B3bMqSWrEbBzsvhnzpTf%2FR0f8zEsPrcKZ&X-Amz-Signature=9fbdadf4d8068cab8a30bbfade73886a30e0663c0a00b89de4f04c4b57e17d43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

