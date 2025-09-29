---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HKQBBLO%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T180108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHWkO1UlxR0Uzm4RoU1SEY3BOdzmktNBp3WNwzVZWRmxAiAzDNbF6peXjSnWLfqXPsnzmG%2B%2FAXgVjGNvysvR2G%2F%2FqiqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrIGVjzXRUSQUoQr7KtwDwIELMTPYpLVxWi1Ae5cPJsK8NYZwXdq8jOTOZMhtsLAGgj82HkNm8aHA8W3%2FKGVuBq6zmBmRZQi2B0etKrhzlAZwl2v0oYc2HcuGzMAv0KRjomn1NMQ4CjraKCqnx4QTp2yqZLdIGr0M9a0Y%2F1XmF2J4OF03hK7bOaw%2FuMGbaoP3%2FmFhbabkEqC42IOtyvtSJN6jLZEMgmXPBEFB%2ByZOq9GjPA6ghvRk2Z00Ajn5CrDHmL8xVK3EFWGcH8Le7220k8bhTlr6POBilXRgvrDnQO6eCwhP8MzkwdUo2fM5F1tlB6MmB945NZH8CTGs%2BMrnF5OarXmW2EVwMultYTPFLvJyIzaylBUM%2FMXml4uDa2rZQIbyjLNbkMAq3LuIsk7id4rFfN%2FLnBt4xWtzBGgGm8zVeaNDjxKJkAy3okfYaTRRkeqLQOKO%2FzhX%2F7W9z0xYbTxcxeckc5ZQ9T4mtmmO7QImLm2X4rDckdhV4Z5JSStCON2um1tGBNozcDlnG83ZnSP5OJeuDYWORXqS5VpR6bdFhLUqt83%2BOFFPvLP2BLVsLKsBSBidMvy2HiGJn%2F4cHKIs5%2Ba8UVpZQb2WPHdVEXEBe19CFSDXLSDF5k%2F3L3vc2dJKcImVRMKbslswwdTqxgY6pgFtxHuZ635w4U0gKmXsU8NV19Hz7yKtFV3s3Wc84gr4%2BvG1So%2BDbTwkTESEpFYH1%2FCUvcQ%2FCbiPcvR%2B%2BVgeVVgQap2jOEx%2FR9tBaiLAueQR0bO9LdIjGqAK2I2W1cgaSSVmfSWsT84hSong0EovzlnJU53SaTfWUgPfRJt8ASnXxd%2F%2BmXDuEpWPHzYthR4%2FijwvDJgMl%2Fx653L134LdzUFaMjmECuLS&X-Amz-Signature=236cd9665e63906342d3a21282a9ecb64913eeb6ab8f683f2924591a9fc54853&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

