---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666WXQW7MI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T160110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGnSbvBMxsZTk2NHSVLzjmqWq9Y8jvBMUQJhOcxXxEZdAiASx54lX5PTyaKmnNe2sJWKtF26MA7VQ%2BOqHvsmRas%2F7CqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMIvJQlyKFHdCHme1EKtwDG5Z0MhttGPMfCoIKBRb4iKfPE9MSteQq4jSkmt%2FvtvEIh3GylW4KvKQcOsUnco4afE8vLywhdg10NTY11K48siRweN7DS%2BhhhNuZob%2F3GrQNEwYlcMo4j1aJmhswqMrwsonkXjL4bZO9kETcbfvJ5sSLiKdqx%2BgV0KtvPkVIGsflhJdk3htfK2pDcTsnqUhcTt%2BwxxkXT0F7TaDKTCMzKRK3ACQ3PMkITDqil5jixLKRpzyni6z1wB6tsh97LjR3I1OQ0VoXDP7qfuIMGU%2BnyqfgEeRiUqP2kVNi4mYxkU%2F3uSuvl8ukd68vTp1G4iAFHZykGpVtXi90MvwY3hfbB7e1sxsKf82xcegXZNfaFkfDmAUhBJaCvR3HW2oDrH8Z2Ds66UvKp2DAlT5xXms9YtBOnAzSK6e%2BKwhNCfCGsW8USQqjOxew9Qzd%2BUvT8u%2Bz%2Fon7Ab7f6fa2yGtgQyJMNooql2Zs%2Bb0JGB2ITSata%2B6WYk3HKlHhgjMOICNTp3ix6L2zLn1PPzPv%2FWFddOzcD8QB45TTcnpYJOsoRQqU0YgkBC2RTkEwgQ5SoruXK9Qb7AhXU0%2Fi1Pu0%2F5dckZHZ5n%2BelVhdMgOkIIQQJBidaZuSWs%2Bse9F3LpnzaCow87K9yAY6pgG9VXEslSKwrGzvi3gfQVPZAupNo9AMuVFNUqTntsnX454C6cJ2FPZuAa0xARjEvrusA3%2Bsz4JIyrhrUc0TXAZ3lfGtWaLO81BQz5yQ8JHSXVV55z9IQAnuNAiNMLfYSLPyqCTfKxNUh04PIrepzCkk3M2TnmkDdpNE1EdDDoohbk0wxUGMi6GtZBdOzwcX%2FLoEOx%2Fl8YzH9BDYWaGsJz4OuCRfw0T6&X-Amz-Signature=167e8789f05895999bd8859c4ac3bb1936aeebe0e16b35b7b0bfbe1b6301fad7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

