---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNLQNTN5%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeOGFKs%2BGYEbv8zif0T6%2FbxLyYaVoMhcsrvBM9h23m9AiBAgFIYq3ZYGz3c0ekWdH5054A7g4GDrbgkOeSb9wal3CqIBAif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjd040Y9kDEQHHQYKtwDXl3OJq2VRovpEgfmC21G9XsAGEeqGxsVSruqATLMXqNtzimB28%2Bp7xiCBqqPxqBooFSPxERQEOGs40a7tJeZb1oIIr5Ub8Oj1T7LFlytURD0BmsAlUMMZhIMVofMWXOD2jMUPKtatOOo%2BDe9kCNhchs%2FzF7RCdgaSDw8i5zxILidCzQIu8e6CEagC5vCgUBExR6DqekdWzNSaFvySrlw4%2FAV2b0KHeugWH8ykigBVmOaO2vOnMqAWRL6ksOh6FIDzv%2BGX1XnjC8X4XwyaZTkd5FJrjSyYx5vBhJUstQXRIQsh%2BVDr8SVs9vzWA1ubX%2BU94%2BFm9IT3eyGMjyKVpPBdZ8GcrR4bY1YW9lR9la9u7%2FylEZkifJ5lzhmkfNNtrnedE%2FtX6hyNnSuHyK9QROLlV3uCPyOmKLT2g81FLL4lT3f4FdewvkJQoxTt1ROW917e70zXDYcyPp9i86PhJkZQV9Ll4%2BvtN9ksA1NysyOe0u3VA%2BPtTlweWccqKhOGoJqbyoX87XvlASBxmM%2FVMR3mIasvOBrzy9%2F8LR5nJxkPbKQ7Ogv4yGDwrdStjpps2o9HQt2RFAEFSu3K%2Fzq9x%2FG8bX%2BETWovLhy5OChqBGHJP1uXO0ao7kNvCZnHTQw0ZHpyAY6pgF4NYbzgYqZ%2FISo9I3kRiX8%2FSJzypn%2F19jxwyYCPGcIs8yQUeaFgnryuwFSqqelh0n%2BmeuVNCFznbH%2BwKvvtmrKVXhq%2BHxseXP4pRJT3YgF0gOvUxlCc2%2Fp54o7Y98a4JGE4SAt%2B38ebmbK0g3YzaZq8W2kioSX7CfyqKiVxc4vLgFVRpHZQbwKQTe%2BNsAg%2FCi1EXKYZBedsOxB4L4W1ZKHb8OY1yI3&X-Amz-Signature=595d2bbe856db5e48fdaeb93139046bebfb6691908e36611824bb39972433458&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

