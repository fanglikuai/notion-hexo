---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQYP3RYT%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T060042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLXdlc3QtMiJIMEYCIQDDwR7TrvaHFll%2B9gOdZ3sUTgjVVDtllty5Zp4Qda4kbQIhAKGeGZJAWzAie3tD6Iij1lJhn8eY%2BwX%2BVVgXidc3SZr7KogECJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzuRHoQbblXoJB71L8q3AO0VWp1mlhV8mNLO91BnyYZnzlNTXkMcrS%2FzxnAYkWvv2oKwRmrP22wMhSp37I7Z6InQFZf9DBY5JGG%2BKKcQKjB0wsc0WHcqGE4JZOyJ%2FyUThZWlnPM%2FhNbYy629P%2FOIhp6VCb186j6ReC8DIvrMd1CHIGzzbdSCN3MU4Q1OQNXVkZjTxQcUoFaoRHVs35oGJ7TWyqbidPPV5sXmngJ89zq%2FzyWCXTWHC%2FgGqtSxMplTwmt%2BwxRmnBIKTV%2BAPScEZOiI9JrhH2nTc3uukqD8rk%2BJsVhGN1oXgh0zt5CZzHVUb6OMvOjVjox27Ij7LUCh4UCSZ3kydqRne8qG4%2Frjq7Zd8Zx4vO2o8kb3OvVKJs%2FNb6uBeJSnwfKb3HtMal0JMygMddsXiXZUjydkTuVoOry5fZQ2e54djdICkvZnCsA7Ff9Tt02cowZi%2BLOoDiG8UMjrlkdaTHvdGum1tKYH1pAvw7%2B2MzDAWtJiA9%2BFHiRrZgDnJa6L7o2ZhRsnHHzkgWKav9Yrt36nf6eUFfAqrg6lMi1OIpKv7wPdUMgIm4%2B%2FnbONfPB%2B%2FBI%2FCinRLKduMHZVVqydSC7QSoP0hMuY%2FwV2vnrII8bMk1ndpbdsOaQGcnLrYUnOWy3w8lTmTCJganGBjqkAVGTr1QtMt7Q3kplWhW9V6VAdpMx4xavceByQIQ9%2FFIIMTmQXTjRFlEHHguOHmp9mC2XQ2mKXzLlYKIxEHD%2FMHenj9VYZ9xyYoyZnMK3Pju5GPv19OLGdPNK1ZgniqmQYuFYCjt5LNXCm00ulob3C04x4Z9i7C3CtXO2MFooTPODOf01SWA%2FGqdzncSIQABY7MKAD6yg2Xwj%2B4FG9HlNSl1prIDM&X-Amz-Signature=b9baab66ad436b178193e8eb03bf8fbbb58a9ef8b97d7c70270f91fc86049d8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

