---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4HY4B3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICWuks8%2B63%2Bs4ZMMd93qGMdQAVBlA5Az531mGeg3wUi7AiEAk96vETE7%2BZAsqouef148Ux6d3aMw4pyI%2Ff0Zxgt295UqiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHR%2BX1X%2BMEDdfI4pICrcA4Au3r1Si%2FSUhETG8Lq8NiNTLp612jXuGsL47Ec%2F21PMb%2F%2BQZ5chE0JzVK0j2n0%2FUetMbCBBPXpzwj77OZ5P2q7m2VDjxNWcO7Vj9jnmAxOy3U7iAJtidqcbhjpQXJx%2BW81001Nl1QD2Sky76lWkFBikLkqrAc7KJ6E44JGnuP8TgqyTVD6GZXDPn0mQiU6PPoFuAJvZK21fslp0JsrUkQFH7t5RZZyabA9vZUiu3uLYD1YTLKIQ2xKlDMoIobbSFYnd2K1G%2BBF7yH%2FwHYLrM8SpzmrmQs6oWDjFCyLS640SpVC9Tu12MZIwwRFha7jTs4M1LJ%2BdBbDMqU%2FdnHepdnnW%2BA4qWxT1i8PHVXeqv39Fv7OZEqHUyrmmSaYkICmuoXCVeUGKJ59wko7Lh5u8QDSugKESFqMxtNq74iLktThs7g5mhfoFJCzA149TWH8r2iX6TrCe0iMzqzWebSnTjDL56eSBtlUimctw3jGoeCwA16MOn%2FqxaUgdyv8YPU2X66Ffgf01rJ3vYRYQabkcM0danHF4XGiEKSBteSByxgI3PWa6DCARtqcXMH5TgltxMEqL7r%2FU0E8Dn%2FkOive3ifBiQKpz3VopgHdKFn3ntqc7lRsdN32Mju94n2%2FwMJrO%2BccGOqUB2jFOWZOIStCaOhOs8BsQR%2FYVjW1chLtjLu8ttHmqiGi7HX83eWq9GuhrWCFX2bxlxc315KnP0U0FXzlOiCKzKyC%2F3ukyVNqVcOL6qibR90zsVRwtadqkep06OxyPm459uapDLos8vGFwcitGns9UZmBUuFvAjV7R9xxYqc81VU2zBa9onrAzxIsi5US7XlATZtk5jEcYO1AJqLX8StFjoop%2B%2BPe%2F&X-Amz-Signature=383725aec7545872b15197e2ab8bdf3dde036e6e78a52c460e0b507fb6aac2eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

