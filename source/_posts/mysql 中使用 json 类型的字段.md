---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYKDYGUN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQC%2Fp%2FLXtEQ8Pn0vE4n2rAtouTePQQ8WSBYaATAlRkyIaQIhANm09tiqXWMsDSnpmITUcIo5w4uk9%2BQ9wUHcat9dEYXGKv8DCCQQABoMNjM3NDIzMTgzODA1IgytHA4dWze57D0ZlUwq3AOluFP2ZaMhfU%2BqnDF9QgzVSdgA2cH%2BILrL4boOjqBZJyuGiLFJKqjb5oscd%2BPHyVPfYs9tRgPI5AOgvjTvYFz2Tw3ZD5JPKoT9QOX%2Fqc8aEcrv8IWuILL52OesF9kamtjKO8o%2FrQdgbF1nABu3TcuBbBgoo0c9noi6Hzs0AkQvMGhx72JHdnXKOL4qQfmaowR2Itv%2BwblmjgpZgUY%2BOAuQyAAJTgGB7G8XEBxGMFsAU0UjJRPriK8gsa0dcByM3MKpBhIHdxR34lcxkzjZoxaTa1A%2Bz68RPfc0Qdv79t5IzrXZlxcZlk9Kmjof5dVe0Gm2ppLGVYnPqAMkvWXKxoqp0j0VLuRwSvvRHpFO3gI5%2FHQZth39%2BJYcMeOmWxnZ1bMZvCpm5Rw6jQiOlPWFUWUySMZKILsGWejyvogJ8kQ8uTl4A6RX9%2BzzGxYz0cq7w94ZJpC5%2BbbX8u2CzcrSC6wnfztweyvM%2FaYFs73wL9l%2FbX1AEk0kU1Y6lRSF7A10cwxHrepLnHSgyVyiVAkyApmwRdRVOQel4EeS3WU4jqtkqkTr%2BCuVeJNOt1YI9b%2BTcy1SC778ExHdIetmg%2B6oSXPDAi77q6pzFOR7sNfOhuwrcOceIssnTxy3MyjhAzCVoYbJBjqkAd6uLCA8EMYaw%2BKjKjTdMVB6%2Fu4xYr8w95LjH5oItjT0pKKrnGvvsddlFZyic799A4KSrzmffr2Tzph0OpO%2BqVHovKxJcs11t3PIKWAEqwSfG%2B2WKCnC6zqcMuR80DTTdvw6T78LwFtUmhW%2FyfibAlzLX0%2BQmLjk6CrTIJyX0BHacagtYmNEyb6v1X8PWQtiCxcaRoGKZ3YbWGdr9Ke%2FBAm0bGVY&X-Amz-Signature=7f37436aee91a425cf448e8625ab51f51334718d5b4739cd88fc36586c9bcc48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

