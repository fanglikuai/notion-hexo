---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RIM23B7Q%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEx9lC4915yce5%2Fx5Abt2nRcSqFZji1QqYb0pky40zexAiAjiNGq6Rm2Je648d2OUgu%2F32AEczZKmY%2Flr9K642ScVSqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FJMWteYToHr8voCyKtwDtQDLVCZt8RyXZECfXF1louGHO8wBeeBJWpXZHIKSnZFV7e%2FPEzeTaGTXPViFcas98dUeKiuHNGRmtH50qE3b%2FD985rN8V93tDVSUAPDiVrgBamcnuMTkbjDdqTLvbJ2bl2HRUXjS6Ae7DJoFLH25n%2FH5tGOxEfen4ij1pfPe3CKEYb4fJYRJ5peL2EwcnKOj56PdV0eXRHIqrHLfNt35znRqS3BE3mEfa%2BdFgYBKsCLo3Jrr9W569BZFf7XpGKnWwzuEkpIPgl3NIGbKs0Iv7AI8eiewJbpFhMvp0yCnVUNb7RR%2FNdQxx5SeSLFYgotgyiH3aLBhYH9ILBlQv5OeJOWh4RPA4FaCMyTDrBz%2FZoQAt1YV9RqmzeFJQ67ATdxz4QhWRRSnabyU3N8RmQb%2B3pLoAHHJAwAeAljMj9SK44bU6IspH08XJ7LZ0i%2F9BJQyQN9dSG62s1C5b4n9Ouu9%2FbkDI%2B20Gm%2F7kmN0xUH1N%2FUac3GEWB9sAZXwiN1pxo1GR47RlHsAPU5I9NLZp5QQF514C75hpOeTYVUfmV2WIFtwC87BD9WKtiefNhHq0TM3VsfbQvXwgJhLEm3V7ACmm%2BGpWmMKSNGgAyan4dEtk5fAzaxz3N1%2FWmF1k9cwmt6jyQY6pgHVUV6Dvo%2BG8x0tnxoxLnATPNS2H8NSUeLn5jwde%2BkYGnpVa8NG%2FauehDj3aWIs28uxO10C05CtoNHoeZpd44eiwGtsBTBKx5uy3Wb%2BtS8OtX72%2BB60TLRtVWrng3x382UU8utUbcIMSk1UkVWdfleJLACljI82MnALIISaqYFpFrFWGEpYVro%2BU%2Fkrm0xr4E1tpsIUhSUsd9aM5WkxYMPHVQY0O4%2BX&X-Amz-Signature=642322492da7358bad4b1321ffd37c3c9a4b066819d6eb98b081b7e745e58888&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

