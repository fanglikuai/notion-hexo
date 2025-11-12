---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOHQYGWU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQCZN%2Btku3QtqSUJVty9PsgYj%2B5zbIQbBySX005LmIBkQgIhAOm78K7HPxI2OQXjqpEfH0tFjzDIHUgM3Ts43kWSPlchKv8DCDIQABoMNjM3NDIzMTgzODA1Igwn1CxNRbg33YKvRpYq3AMM%2FWCrbVlL53uGpQ3xG%2BdpNIBJDJ745vIHfiaxyxuCc3khg7EgGbgoYgVcaI%2FzR%2B2wuPUQrknzHSBQCdh8cyFW4oeLXxtFquAoFdZz9NLOjdahHyZ4JNIfqDtpmlX3e7CgtZCEh1Rh0jOK2B5kKOGtl3KxWjQJWnni4dLhsdr8T9Cjy5B6NEWj3rO3fCquNap4%2B%2BKV1AKL50uIX2IN2Lx9MXUNVz9VQ4S5s2GxyUiQ1XF4LCGerqvKVXHfgnztXDjoQ3%2Fpw2LLX9BjXjzCtizRjG3gqdLFfZOc19%2F8HaWTtkg3DmZP5%2BCnMaS9D9kRl9kpjH6OuM5Z335G%2Bt5bGS5XjZe8nD3rxrENr1sMMepa0DZE9ZmloRURG%2BHf%2BfnNRwLrZZxIsUa1%2FoxcVDtFu6rRRUTWTsaZwT4tIHED4sWabnU9aCs1J72HTgcF%2FME81UBy00fii0xUxc%2FElXu1VbeXV2KdstN4aqCrLfs5kachxwYKodBgZGXK%2F6qrMPxw3MGDKbUwDL2jEZj4CDsaS5CrvJnCxUsKgOzZb%2BkPwbUKfeLzk0cs9wXzsK0B3dNxQy7EaY8Nt%2FWg0kpy61LJafnRzms1dPTN4nVkJNcoJt1MdkU3IXF1t%2BgvKycKpDCrkNHIBjqkAZTeK38YvbwWIPKWWjc9VPMVNTWP38j6PAc9Uo23ETVWwPElA9zPXcktjvROmA1zd7VG%2FVKBGxfqcV4kRngI3s55KTk9wnxwnUjqOW1NxlI%2F6Zad6FACj3HPQnF%2B7Sesj8eoqIryMiTLZWt%2Ftess2zozPZ9mTnyop21jZkuS3Z%2Bb0EsU1n8H4UwESPSf%2B6Q4IWtjbRRdJ%2F2XePkg37JbQjp%2FSWMd&X-Amz-Signature=fa3d306bf3202ac15cf1667843c3c5bdfc8766987b3b1feb1f5f6fcc87c2d5da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

