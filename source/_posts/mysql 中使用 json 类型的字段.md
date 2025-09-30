---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MRH375V%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T160105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCICxJJkA4jAbZJxOMr63ad35pZlHo0eph9bkub3ErqxghAiASxzNOrMJTSGojHx0CNmwCfx2Rwc7PN71puUutb%2BHCUSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX7WErT2%2BMn7H3kzQKtwD6mkJ4yLEFAabILonvHSdnQXkwQOtMwE9AwEedL8Uu8OVjs%2BFffK%2BMrD5WFBANTsJniOzVthljFzXf1YM82RxaLGiyCnMRV6VGwi99u76G3yuIZVrQszmniXlkzoNv22y9H53R61lnYsUCgw%2B6m95X7l5kvVWOeefB42PDuaSvh%2BMCorwUCHMR9spETWesICfPiyVvJkaF2PVig8Bev%2FKo%2BFmnZfDZV7uQ1plFsdHJqPMoGhhaLjWYHoaC7rA%2FtLe%2B6I1REfbKRe21LrHRgMoGmhCjIZgEefX1Ty2fLZUJ0L3CYyVuZ%2Bmhe965mQ67P2lIVKh4gB8VAmfJD1uDtZGmWsidMN25PtIZ70SH5CF7v2QJuJsXcM41wCDeWDKPYK%2Bom9BaaEifi0dyKOzWjwDEdvrgx8yMYThFn%2FpYhFsMjFz4M90qsY7DyU6F4okUdHHHsVcZF0GKIqQyU%2FRDLaJfXdalrTrmUQ4535UjPgokw6q%2BeIkEQbj7R4KKIveUH3LuP9qtiJowYmiOiMlS3g7cFQxY%2BvObGZMZslgTa6T%2FQLH%2ByMvml%2FyqZAqCDw9jwnFZDTB%2F4x5YzYBmOTpRIUhRVeAKkVIZLDjj2SvDgtlDAmEvFM%2BnPb1LU6q7NwwnvrvxgY6pgE1mGQabyBBn53og29iDcoaIkmu%2BVwsXhM18HBMcGBSnptCiIS%2BUW0ErmBUb%2FLQ%2F%2B4V%2FraqNm8qFOhPzfEO7YzaNAV9qPXKpJlbkDI8IsaXhyZVyLGIXyFWxAiGlacYZmWx%2BaWfZ8TFEW2CLZFyNZFuUFgWKWAlLp4QwOvf%2FzAUqoHtjAqgwRj%2FSUKoOOj4LeON4GeWK1OlgPcQ5y6jEn9%2BA26BtEYQ&X-Amz-Signature=71ca914bb7397d528973f1f4de3d0000fedd5650e5792466566ec5a1a309f454&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

