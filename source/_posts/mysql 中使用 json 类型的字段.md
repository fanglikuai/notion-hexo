---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U56X22FW%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsnZornaDvtYZoOzn94XQ%2FAqU3gFKc5hggj0c9oNN4QAiEA%2FfC6uRE9GI%2FfhAW5q%2F4Tu7i7sZNx1xpXtsrObG1xQ9wqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKWCRHrooGfFeSlZ7CrcA1rk%2BUpD1Yv7hRruOXZBA3joRumhWH0j1K6hu0ph%2FOARUxW5cH92fOn8ypCIHH0tCWc0EbFSYZv%2Bq4qpxCprojT72jg2BGAc2uIQCsQN9dIfUwZoYaNpA8bj8VGmGw3nd4%2Fx01%2FEV8Q7n3aBm%2F0OWwTqJkCXt02x%2BJAtArWZ2BY39kMV8NV9gPH4Bj5gV4gb56E1rGQPqu5KpIniO0ibY1FLBsXkCTg%2BVSDlvZLfHhPIL1V0gduMickwCtjWA7u0e9qouKSEr8OV2IdL8QHT7ZQnqP%2FYk%2Bh%2BVpQAAQboZ9v905a1Btaw3wZabDd6YL8ZKs8OHKR5g1JRlVjIGGfs6x2mI91eb6NNLaOARf2Uc1xvEqu4FAXIO4jtb1lPrQ3gDKd0TiiBzf8ZmEBzqx7yXMujL1YkZRN3Dzzu1QpUxMqJbYBVYTb1DfrFVyptdk0PK3q9bw3njSf1vfcWqfkFq287WiRlSF%2FaD05iVyR0zFZD%2FihbQLQIy%2B1xHYRNKf1XlrnRxclQdzU8Dri8NR7PDqzTRDMrnXrUBQ77A%2FKdPNGP22kZF04v0u4Drxz9OIiujYBcSRh5qFTwFd%2BehgRDb%2B0HVP7MwmR%2BNfI%2FUuAnpu1jDgzKzPgVtbe1D16wMO3c7sgGOqUBcMnaKs%2BaOjWASNp7reLBLQ9zlvGKgdUlF9J4tUfF5WPpmSMHcGKmiWhrMQi2gXK3EnPzLq78i3ERy0XnvlAKxQaqb5IFGxD75D1kteuBQpthQ5BBn3TviSMf5CaghMDsYSyZdxt4LOD3dLahTYc9xs1fitq58whBEcZMkmNIaItmYYfseNTjYIuRCZoOzno6TQJWwljiylRia2JsmlTjaCMKNiCs&X-Amz-Signature=11c78fed018bf649c0a56644a10407500b50a49c9c3a321ade9fc69125b65fa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

