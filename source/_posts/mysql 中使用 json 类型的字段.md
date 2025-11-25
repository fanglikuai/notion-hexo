---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SC6JIBKL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T050107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZcgsKtX8bnSiS5ldOx3kuZo6BB8ycu2yGz35qAv3POgIhAKyNtWlhbjBy7J9feJoZDilFQ8Kbe0wAeHL%2FOYxJAiTXKv8DCGUQABoMNjM3NDIzMTgzODA1IgzBaEmSKKn367GzmOYq3AM2gloLkjn646PUild3OlDkSB2GTo2TyovmtKl2uxmQNCF%2FBXsFjme%2Bu3eFpR0OAhfWEfbqnjDsSVzynRc58slCLbhJxXgMxIA9ro0tepCd4HmQgep5m95SCP9xE7O%2BPKV4bA0d%2F7tjj6eptyMLpdO3YhO4HMM38%2FGmEflZn8kx%2F%2FhSS73wsvAB8lfF6ukC4eSiA2yhQqjA3KJaMehBD9wmW33nUt4la0%2BekU01hTrS3ttDahJYdS15K5fmMzLJ3jM0K8r4tfGVgnS9uDBeuXq5yijnvTrI1sA1StE%2BVshiJjP9TFnxTRWqvQAre7lLFQ3GJyMZf6Ro5rapn%2BfFvVZZchS%2B6fkP3sB2dYCY9jIJl9gZHhbrCNgXX4CdA%2F7nLljvcQcNVXtqenUPjqq3pU8LQN0eGmLeal9rkFlPsko8K4jy09t2aj%2Fw321tVUjajU993Las2C8KGBsfNSTijXFCCKM7qfWUoXpBxApBeAIwf5R4SnbCCvX%2FtPnqfeboBKPFZv8h%2Br8IqakE1byAkHmWdEP4Bs%2BtiiLFkpdUwO%2BU6F3oL4qyMFCDg3hh9dezfRr5CUtS6m02o5obIq6XyL%2FFNrrgYpxAM4cFC2Jk9CgCCgzeKKWgIl0wXAOwRDDi2pTJBjqkAYYWS%2FlXtvAMAowMRZEgp1FIhy1UTlFPHdT9euhoi72pJ%2BfjASB6YNlBehLjvEMJAKxPjLHQTWeU%2FfjCbP6ml7608LSOK9mhSJ4gQdaEkVGRHvUxuH98nxq55OA1zJik8TYpEFzEDNAkBnVjNLKFUoBavflO4wRtEZjM4RNYS37%2FqM6txGPrIj95Np91pVoMKf4eDbsS8ZcKWXck0PkbbqzpsFkE&X-Amz-Signature=dd65e455aee1e39c8ac896bda02dda5c0505378815dfbeb63f45cc6dc7d13b3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

