---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKGNCVB3%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T080046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH48UClHIk9o6uLn9aftRK5WVaiicLx37Czph7UWKZ%2FAAiBMgnMBiZo0MTgLWMNqZ7yj0qMYBHbnI22EKgOeGBi29yr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIM%2BziwFahvKvXSMJqdKtwD%2BMRl8iCT%2FIvOhP9yaGMiS5wXvoabG31TYAkSwHTw7PsQycb97mUbMGmJdQkBXcRdfjZOPvntTL1vTXEgx08j1aMG5%2B9bx94YA55HLWOsvX6Xe6gu1x12eDyisbFfqJ23tikr1NCrE6lBLb0m31BV5zKJM8gFwz2Bxy1XORhz8yqrQYz4%2FGNs4%2F2UYvBQqsnqR3K13mXyggZyZE6AHBt67g0zR5z70Aq3hP2C1XAdvM7ee4Ee8l7Fn2uBeVAVFb9js7L1ATWbKsxjJWOynSsuzjW42HCyWmiRg7BErLTVipYmqUg89qe8YitvG1L9TnfjiytDy7q5Pp0enyS7m%2BtWi1X7j%2B7OUb0iDnJu42m8jF2Cvj64ehBef8aPwUgbBdp%2Fa%2BTG0Zbb8XvR8rrFkdH%2BYPWLorZoQGxtgPfJEeN5yUnbPKD6pc8n1XiEGF6cBJb89sKlxPWd0HhyzrmqsWNDBBlE%2FQAJy60i6O3KOAyrAm5%2FAfaCH5hYlxJ0y%2F68%2BEKsgijqP1Dne0JtV0HVN6mKVgJWRrWgNqP4br0OU%2BTbW3DccwzUqi7sgghPOANb0Bpi7AzfaXQAz%2FtDg%2Bq4L0V%2Fh9IJ2NHJN5QPwdTsz3C7OwWDqjD67bEUWL8hcA0wiK39xgY6pgFEEiSpmz%2F03yddonFMt2tl0n%2B4%2FhrdXRb5NhqsEWVmGhaAuwzFV7OshlIyovzyOSMsteLT%2F2HSUkzz84VLXJzbPvV1AooW0KQW7KY0bPBE6THpf5F7Rg3KzQ2GyvT8F14gwPlv%2Fpwuul48ywgs0hN03JIPZwG%2B13rj4ZIZp1C2tRNi%2B5yZ2vLbD5MPlkROm%2FY3taqC0%2FmE7mKtGbIh1bv%2FeclI3tMi&X-Amz-Signature=cfa44b9939aad79c27d88498a6117a40d3f3f564ea43b917a33bfd5fa98005e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

