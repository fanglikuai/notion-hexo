---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSA4765Q%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJHMEUCIQDr%2B2etowqdAADi43paJUoSO9L8gK2mwin9923N%2BMyWhQIgHvUtbp%2BYSeJRGNJtJgmxzsWnZMmSmBIM1t%2BD8VgfEVcqiAQI3v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPm0NKDOz9HZtcY2%2BCrcA8JVWxJJzHUPDwXn%2BvD9uuQTcK2cxDZcjtQtZetp%2BpoWVXl5m8Z9i3eHAKPt%2BnuJwKBKzlsOF1HRYYmfokImObGG%2FqQv4NSF6hZ%2Fst53%2BnZpapHrIKEQPsu32KcKcdf5gZIL22SWOnjrNs%2B%2F4okeSn%2FE5sz%2BazUx6Xc4cPcomQZVf8gVcm3S%2BodEBH4WGttAs%2Baq%2F1TtkAYQGaPj9Cs7YsusykuB%2FmA43le1t57bKcuvOFk21bNy9xWBuIoXTgGvBQSTWGH66tIaXu9M6gfa73aTS2GCl6fQQGo%2FqD%2Fdf%2FOASy1vLtWuKTjNMrS5YKjqraKYf0Yx5%2B5yANQXjBU9L3ZWWvAZZ0d7OvNPTK4WVQK14PS95%2BwX76TEIUWWpiSU2SMG%2BabQUDaChGEUqvUzR6bffAsSIjBA%2Fs70qx%2FHthci%2B2%2F2qlMas2hflrZLom0jszpXgtqfoU7D01rw%2BrJZSY%2BZ2EdaMJQhLGqhzulbzd9Fp5jRMO7EziEASpySRIGjag6F65dDdrvwEvoCCT0IUOSk3WL%2B7vzpIE0ow9ImAzxc716XfTybsyUaP0FgFO6O%2FyusR32lbGYvGKTb%2FG7I6QrAoA0BIp2IgTJ4GoFjWZm30pau7An5REmJ3xi6MLzz9sgGOqUBxOoamyyL3vgWvE9%2BJGzfBYlf%2FGbcP6xomGynzitlb9Kxn7meH2PB4HY4Sr37ihi05jRAfPP0kdT5e%2BV%2FT%2BXkouoDo2AdRFzrQdN6iq0t6Q9vM9IxoCjJ8%2B7pjXDTYuGgJ2j9PpqZGFwYVZC%2F3S%2ByOCN0cN8CEjEIxTR6JWRUbAU7D%2BPKrV7974Hiq2MUwsOCHsN9SD%2FPY5tUB0d7UFXs3VAJcK2%2F&X-Amz-Signature=7c5ca4bf79ff8562bf2747cee89dd856d4929713205cd75c56edf4124a1c8402&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

