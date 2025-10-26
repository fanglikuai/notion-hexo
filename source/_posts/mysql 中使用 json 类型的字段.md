---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y5HQM2XO%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDsy7rWQ%2B8N7DlSin6AUHeWJoeqsQqYbn7qdzPpUtJd5AiBSxQ0qi7QcVR9qDLZbUxkRfKPJbp1pFJVQD%2BW9TjhY0iqIBAiI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO6fSpe3Ds0v7P7I2KtwDtAtZMl1wVOeRLiaPCTcG6VPYpskWt4OUEoY67iG%2FVclPDYtZPWZwPlH%2BChXoFt18Lz4O7mwOfGcvTKOqgM5WilyCxhr7somDoeDtskjQWdXliS4rkdkPO0g7i4hqs3T2P086FK8asmodkrzTy8fduS3Rtz7TvIyj3rlZI5X6o9xTrRUhlRwF3e5k9W6P%2BPdtZym8fp0BzkQW%2BLJfiDGMayLshzCZ8SYUBNPYh95wwZNZZqqzI2chYu5f4I%2FMgaCmEjL%2FtHwDvlnhrHqB%2Btkr3k7BdrDvglEwefkrucMoYvaB1iW2X0u3LhJL%2BTcETidDQhNhwmMkMKKUywZCSA2d5PsgKw4%2Fb2FJ5NoF7DsizUQhXQQxBZ%2BswP%2FnQsbUgJ9X0pUWoUmJOhFyytxOWHAKkquzf%2ByLNm2pfujEr7nEtVuV8yJwQdjs63jeaBo01A7n%2BGvVrxPZU1LJsKxnqR6TYVd%2FZ2%2B9qkGBfVZPGjloTJqW82e5wTSwuMsfqUp2pOZPI1%2B3v1z%2FHDKq5cu4d8Pg2XSk4tskjNPmYCUJPGyfuKeKVZKk2w9xcK7Uu6xXp1WxwjqiuaHNeEC0IpLxB3pARQShrIx1H1dDyo9a61jry5ubPTwKTBmilFLfBwowm5j3xwY6pgGv0stX26sGNByyQeFunAaVKA17MaUuMvv9RZHeEcowQ5cCXOrSZSf8azYTe83OIM2pQjEI6N19B5q%2F2p0vYB7uEFl1tFCnEHiV6Pl2nAIjuzZnOywIdZBW4ucwIuZBt7g%2F57GPSv3Jk7wtXlIj4cOELPH4kdG2qvIS6yipivomXPxzjY0A3WPF%2BxtoBI5%2FTumQODjCYYQWx0wGZ6GxZ2fMz1VtVuJ4&X-Amz-Signature=fd2a1a0ae22b30d10496318da1f66b95aab33edbe892b2b710b4f2b1020f0b4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

