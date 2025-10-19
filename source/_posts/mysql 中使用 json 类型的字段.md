---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DVWLUJJ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T120042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIFMwbVJzZ2mV4hM90SglyQre6CUxlhpMlkUvEo%2B7gH9MAiAN8dF%2Bv6UwRyjVnoGeL4593I2AYJY7Ksi%2BBvuyOwE%2BgyqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWTf4vfLNUNPqSmdkKtwDFgjG2iLPi%2Bj%2FgX2ltDYUkTE%2BCWRjmlg46nfVmQtOQSJOtKSKdJTTYxU883xCD%2Bg%2BaSJO4jBjEhFbNMqkqirZ4qRrAiZRWLNgi6Em0IJvsoQ%2BnsEclndARJOeEvJXcqLRTwEPrR%2B2VDCmRjJLg5%2FDVz58oub2YN9VOdjstmVzoVRriKt9K8EY21NtLKzHCKfd7%2Bk%2FX1LwVdXnCWKi7jzyer3DWEC%2BMUjZomtCtN9NGTqIhEOg1kdpKZ1nS5X9opESFkNwOscchOOelVL0i2YHyeeRQROT5yz%2FjnS4PhNhCcPPfSSuCZS3lIfXmvkaWoXlVQDfaEzKS6Fn47xdwhUdECJvCvDaAd4a%2BR2h63HXAjYJPPMzRdCXDIlMS2AGP7tJYGeveyeXbVaNONAhMqLvR12RQPRwxf7NZWx7FfrhuRPO45IcCSBPKLeaCISHUU4RTbmoQeaS0d%2BPYzFk0UQ2eslQA1hqr9bxXG11SwqH3dwCKM%2FLVetq02q5suL2WctaQFSvMw9Iloj4%2BisIb18BjhJjnLiXe%2BaW7oUmYcN3HP1lOgE8dl99P6uBsa6qHq4gSqDcdgYHBOy5tm8FnuW5Yl9AegdNeTwH51nLaH%2FvAsmYE0Pj%2BPFpjZef844wr4XSxwY6pgH3B3cZPJKNDMl5qM2xQ%2FBPzHS3RpYR%2F7BjaY80fNmA%2B2Xg7KO0jLGu%2FnuH7gmRHusIPc4nf8q1Jcxc3IcKezVHGAn3JgCbkFo2gyhDa%2Fh8P46QlxDso%2FPEazuhNLAtvxAt%2BV1fRKiF2b1aMhZZedC1FgukqKO54O7cOWEabCqwQHhWmXyKAJwL7pdxSYXwvba474ZELe6uw0v7%2BOu4bpKSr5qUbjQk&X-Amz-Signature=50b40eca934156b29d9eca430bde1b5414a93ae356f81c53d4ed53364f09dbc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

