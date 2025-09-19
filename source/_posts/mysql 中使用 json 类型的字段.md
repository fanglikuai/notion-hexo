---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OADEX5X%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCkw96V5cg4YP1HqNzlCKyyrOttvDNqjOZe7LKalzimbgIhAKxV%2BKJ%2FMnz8tRzspIE5qxypQ6ah5uHFyF2yMe8mUlf0KogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2Fa8Z4qT8n5ufhgDAq3AM111Y%2FL2T%2BfJqjm9Moyjr1sLw891FPCaccvT%2BQCJ1FyMYwABF2fq%2FARhK%2BvPOqGnBckjTtVVMEk5wnumQPmv%2FjpFDQ3cTmxq9qaYRxzA3qVHX1EhQlgfUod2BethrTcxmgb7djiU0O%2BBTq8uu4tYZkZE2Sd0bo84KwHDJ16dOdd8sZQxhJVQWTAyUS0jXbyA9b3upCsatLBKm15s8sHBD3%2But4tIN93K9EBoUEBy1EZH0iKFX2Sx568PjB7TAL1oL3EVv2QpWX9KmNmzv%2BUwlg65vZwJvi9CVDzd03QD3Bxy6HSNTXtuFGiuxW3GPm6YCqzx%2BD%2BnEW3t3%2Bkr%2BiEeBLFsb0uP8zsGKc2gzk85yZi9D7jfyeuLwrYA6MLG5qOuplHhbjc8rkCogd38HxFqOpTWgoiXkaCniYcuBZ4P8XvEgQN7KUXukM8zUVz1lSND7xrOsFV31zYkmZXYMI5vQ2Lf%2FKyrnnUeuhYeUhvSqWh6a6F6ZDKvkEJ9Xw3fJzG%2FIMqJLEe19xtetgjWyEoz30rMaLw5uczyCX5ev%2B8jDluuCF%2FBBrYrCm92qQinVqtFgRn7FaJzgWDbOGMSD4d9QMy0rfaLXfrC5QwsEnwhhGzX1LrekvTDHv6%2F%2F%2BrzDSx7PGBjqkAeQmoj%2Fcrimbl8vW2YtLiDlN2JoikDUP3YPI5LR9ndaekCKmO73sEXKDIGAN137N2ib1J70C%2B7NFOy0IoaxII3UjDdC%2B6JtT5G4gruWglrKubpiwx0X5Rmpt07zjgfYWhVXb%2BNbxqmKNPxz8fefjP2B0DZ%2BT8yKMW6GsFl8mG7WlZj7m4jWAJvlfniePvQcWvo54wU5nLLQVTwNMiUvIKt00eaV5&X-Amz-Signature=9f071176775388d0d0ec78e5fd2f63f59b18dd713451f9b7fa01771776b96d54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

