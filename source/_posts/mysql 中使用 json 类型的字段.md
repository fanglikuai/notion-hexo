---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCKJEAY5%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIHzjd7A7H0wO8S5Jgi0Q0Z6og3uvQKavfEWUnybZc1QsAiAQNIdZ53izto2kpI%2Fnxqh4CaX%2BvLFBcxocwJuu6DV%2Byir%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMuDm7JaII46DoJ54UKtwDOu4JA%2FlxDlSa1lvO5Cg6dcVrlRRYW0Xu5U9UFQ74CC8O9ezwg%2FTgbJCIZRcEP5hOg6LB7z6mPInVGkvkDpntujAZZCDvPzYUWQcjhOrG6izxtx7xpmBrT%2FlHFHpqZAkzl4KIg47lKhCUAXSXU4q%2FW5fqMj8HMvT1gvriE0gvFgqkNhqvSqbA3yJZbE8nze1enaOBFhOh8LjvF8Ne90zzUxwn6eOZtqdVCzhDKNXQ8LTtlOsDYw7Ioa3ZL0QtZv8BRN6EQR8n1moqHSstKhRVvRK4yPy1XRGtFlD399EIMZ5A6dQl4adIGgK7jYef74EYR%2Byivb8prbiKiEYDBt7sc67PRs5hoCwxI3YMNKRJ1cxM8aTFUBdHN4qzWbqqS%2B8hCiVvHfyB73nD7qky62lRw4DBs92EivO4lrwp31gw4NaiWRV9lD6OMSRLw4f509HcTrA2CgWA7aW%2Fm44QLMuwotI%2BQJ1clKjD6QrQowvXYK1qDSoWkGlq0wE%2Fqo%2B1%2Fljm7bZEu2i0bj1s8cl3JEtbatteqhwXrjybRdi08Vr86TQsLye5ivZ8kAYEF171tU3LqtmX%2FldRWx%2FblLlcHs%2BbzvdjCMRHePW9REgbe7qJ%2Bl2OhEddHxrSh4%2F6ZNkwoOiVyAY6pgERXxYmf%2Bz0yz7xJ1HNtJwleFZHnqg9sYUappCR6BLPRRFGPa5FKvJOViYOBpYVGv4WItrugYRLlWMqedktTcVS7hmfE7povpUTcYfrB%2Frneaip9aeVz498ZMWPA0CdTXLtQw74ctuWLlRZQno5kRS3hFaMqzEQxmym%2Fq041bdLkMf9oK3dONCqN51lbrvnBia2JQNnfPAhYmoK85E5396K%2B1tn%2Fbkk&X-Amz-Signature=ad1644be8f7581e07affc645d620d73c559d5ecb6d2707c6a99e3d156596a9d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

