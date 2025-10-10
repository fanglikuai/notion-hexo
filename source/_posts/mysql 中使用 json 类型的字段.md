---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KJU7KDP%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T130057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJGMEQCICURU%2BtS7MA6c3ki7eFwWWs9EHa9jQnv8hUZE3a7jsqsAiBmNMhfe2s%2BuANCvUH2dz4LPHiu1dmjg1ASJ4kCmsyhxyqIBAjt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhwW4LoZTQIHYFwklKtwDvWstsU3G67zFA3q%2Bh7TZGQmyqsocg2oiT9PzSuK2ynOLI8cVgyJKQO%2BOksDFlSdfbnLBLzak8uhxdAu%2FVTCiTbw5ZkXfo9hLJ8PuPjkiPmKRrjhUo%2Bjc8p1QZjmyBT6f1Il6eCLyEMbuP9LCG%2BdMCXxTDKl9uqBlaaTZaVGN1IIlq9HGec5G4hlkYkDE116CjI2kVlIY60l5te64B%2Bk36IBPsXRNvC0g9jwTCAHWldmf%2BKC8tyq9FA89JFb%2FAlhimaV43%2FbtT1misdzOI7ePIeT5R1blP0QaPg%2FBRFttcgZ4j%2FtFUeH%2FX%2Bw0UmyAK95qnZvswqFZHT8DpdYHPkSph4yeQRxPDsuFdVMJx2BqU4zSK70iCrlUYS4RaZbn%2FNP4TW8WraXTaza5Ds7v%2Fq%2BiTO4CCtallRuOBaVPrhor4qiyCdL7SIwDS594gRLGCN0Y7%2FSDUKHTyCLjrUXyWuuwtJeQJk574tlT%2BtxIHFoK%2FozptsL4NqkC8QbdVXp8zjPGvFqwKP67wT%2Fwn7cu9Ay7TM0tn9aEL1jWyyGWMBvBirZM7GTDdKZIm9l2NBkJb023RSm2yH0HXnFeuKhToP7Speom6k2YNeiVD3fQY4OJHdY%2BFpH9ghZYK%2FgOC%2Bow%2FeWjxwY6pgGDCBuPM1RyNE0Co58zgJiT771rDYzXKSqAv7mVgUrJ9YzFvFE1CjecDQGpH4Sb4un8oWdguF8b08ylaqFzbmtJb1S97SDlnI3yKVD59C6uzgS47L4mra%2BgT0Ase4hOhVytJq8XeP3blYj%2Bz1qwGUmfNn7OfGpwU8UYiNKPY9qsno9%2F3F9jyMBaLe1z%2FZ6wuZqERWkcwCjlKEMX3THYbmq2p6%2F74O4G&X-Amz-Signature=09326dd702fbebcb82445d6ed0f74f140521b1b290808127bfa51560bf567a9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

