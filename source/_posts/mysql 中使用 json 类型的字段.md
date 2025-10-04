---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAZOXS36%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T130118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDIYiOi8gA4oKBcc2y%2FjLvtw%2F0TifxUjiTLUfQP05k1yAiAJKiBy5jFm2WAhrJKzSo0e2S2z3xafrf8vA4A8ZZ8hISr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMfb6p%2FpDNCYwHiyWrKtwDTpJ4PwDK21NNbtcuFL6I9iCdKoakLpgQpbTFs5icrSWtNTaV0EvfYdRtWeDefVsbFcbMle4awynPGc4xhRZ2b0THYe4uIyn%2BguHhYI%2FdmpU6TFPkB2xQN0f%2BGMhNxEd%2FVbCZ5xqNBJgBorgNS6SFmJNzAoJyj2AlXefqd7TGj5r5%2BR78qrEemhPPaq02Dc%2BwhTowRK6JRqnrwrGal%2BsjwFnRs4wf7n65DMHb9mTNWVhJw7KtT3xZsfYoZd1TeyPvsineBXcSlasvdCswOvEfVItJpZXfXmm%2Bz6ovKW9%2BMluh8R%2FCozD5dnx8V7jwd9Lr61Fl%2B16OVpij%2BVVzrr1gItM2G500FYQHTHtEl6WBPYmqzu7EYWKYGk1xA4J0jLaWcrtZGm3EVKHuWNJRZDD5YbbuKpnP3P4ln5SpFcIFZsRINc2esx4zXbFbG1cZTuF%2FdMQ1BH3xRnEF0n2l%2FZ6EqUfFri5VA5Lxgl%2Fqp%2FVOrRoDVhVeG1t2xRjechQIazB5fSgzHfcKFdgkaKmBnRxBA3EkuYHw8KyJU73yfosvU4wd%2BxFJF%2F%2F2vGTWr2T%2Fyq0gYIpDtHC8hknWVhAXZKk4iuysdc%2BLW2ymuYHXjvNV7kMQxqatMUKMmzzR%2BJwww%2BCDxwY6pgEfzW4bbgS55QlSXgpPZqKGCqhpAKnSujSfUqhxhG0wrhtFwAsaFq5EDT9V94s1JKvSxBlJ7CFIocW12lhG0qeSmq84NqPy%2Bh4zfBAI375B2KrqYS68ZceJ%2FXe%2Bi1HyJSRYis6tIjrVY6fJpbIbLBi%2FrhzEXgselPxdOIHTT0OWBLs7ZRWjEsXhI7VO6leqqCsj92vI91phpyVXi7O7R2QRQYisJasU&X-Amz-Signature=86d7ef29f576d078aeff52624f4354a6c487f9b8e20d777f43eac1bb20450b82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

