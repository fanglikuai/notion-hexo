---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IP7Y4JA%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T170040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCoDTS408ivHuZ07LKNPy5lYbTP6qCykVUU5QjDF82GLQIhAPW3VZHJauu0LYrMIdiPMSt41%2Be0CWvKp4CcfFhJw2TeKv8DCHoQABoMNjM3NDIzMTgzODA1Igwvpx%2FzOjf4V9pkL%2Fwq3APZzYYc3iE0d%2FBZTLNSxlaSyzyNH%2BhOWZQ3WuW8mMKH%2BXEGJLwyOBsQU6ziJcIy1JY5r1hLZWypjDYUJJ%2BPYTWrEoO3pqgt1qOaeooQqoekMh84LoGpEvAcmUNSI2%2B0GPLPoUL2sGnisgHXVF4ZEffYFUBbbcXGC9BmPWw5mh377vCbzzpUyYjglC99QDZgtWifezvmcW%2BoaVZAED2EYlk%2BJRsxxtgspO9nYG2a16HXUheZA4ICmwKVE3G6LXhimjji1xG%2FCHKGF2ErbUoNYoSwYlTUJ48SGswczqB%2BjkX5wNz0QDn5Q0qMt2gEG%2FaMsgyaauiucsNo3t1G%2B4iadRTsVVdNAsgVjkonYj2UwB9CNBcettlS2S2atmRQ1ysYa6X1ydpxjIIeXYeHLpmwf%2FMP1JT14%2BFzw3oEr8ZsdNdY1g5YUTGhBJhlqcXhS2qDCKbJAICZ55n%2F3%2FF8eL8GHxwYUZlUAIIMqOZXuGH7aPLqBvNLDqzZxdsb4HPzjFXhyOhF36nojiDBDUPmXSsYe2Rpk%2Bv%2BoQHntID8rQn7dSQm3AQaKjpKsPlgnFSZC6uGugMXw4iO20P36dh38ZQdrY9%2BBIJFyqOn9ztQbJawKZR9MktKsR9LlF43TggrZDDG36jIBjqkASMBXgUWuKsjnvwtf8ORh6yL0X50xoGBBko8O%2Fmy%2BSK7qDEvQtqepn8jelmRrTrmPJ1IBbunh0xaZGSi7u6mrxIp0kHf8%2F4%2BcR6Tck41DFy37mSbfcImWt5JzLwyi5EQp1FT9ucVv4SpywzC7PIDojm2%2Bm6IoVzMAwtoDK0KEANoBuRHeqAoVCWlBBLjkuS%2FARoiEt6QhOIezz2eq%2BOUIETsdo9T&X-Amz-Signature=b6411ac36163ae8ba5821a8f75c1c2f06dfd9e638500d5f94588a75fa51ec72c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

