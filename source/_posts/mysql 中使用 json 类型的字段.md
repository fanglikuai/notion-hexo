---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667353DXDL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCVH2pmgatYQKA8Ts6EGMaY8XrvbSpz3NPIBTTaHt%2BLaAIhAMbyCKeKcU8vKLQKYZgUZazoNyeGEUK46Rp9Hg366ulUKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzpgE5osWrXXvwloPUq3AO6r9%2Bq1OCYYLT3TWxj5GSjxDfonCDHZGKb3hyvOE7hfcaFITD6vlqgBenC7BuvOQ0ziJ8RhI%2BdQb0z%2BBf%2FFUb8aUnPI%2Fd57jchzWG3kmMyD4%2B0pORmkXlZuS9ok3ymioYiRRc1luYZm7CAqfFtSEDL6nDdeSkejC%2B%2BPefGaUCBIsrE4%2BUZzzdgp4W3p4D9g33by4XRJ4QbUn6%2BplQaVGGIrMjcs07E218tvdM4eiVUT5Pd6sVOnv2xGSHf7fpM20ylrJUB%2FlGj%2Ft6Vvu7DQ6jnHy1ZI6ShAZ%2F%2FTB0qAzSozj5bl9Xn%2B6XSz%2B0eiXqcPc4q7H9TnVcgKzPAWKbtXWBbMUv5Zw2tjgf1g8sqFWS5%2BS%2B2frruLApBHLxsKYXkB4IXxD%2BMTDOov3nQibw1IyIjLUE4lwsz9BMwVJvqoKAKy4C%2BbyeluCL02zRCwf3xQphq2tlKpyNHoCKEFEPqI8n09GWT123xbKORpsePQqc4zrBFGvkI7Fkk%2BZdiA3e4ZOmMMkvuW7rWkIwv3CYCC1R66YdzXnYtivSs1m6khS7ulJuW4kPLLWLai87SGqKzXQEwfSNJAOnkam7jId1fzlINlxU8IPFYF7PI3gUXMBjUtd3GsYrlkugv9h9mwjDJnu7IBjqkAep1t%2FNeoNsETp6xTNWA5KMqvZaYmf5uAungh70VNmEON%2FLP14Xtkz0NzuvZKifX9sk6kFqUkoaki%2FcyB4YUjT86ki3akCI8QaUDdJ1fWN6Ax6uCRuJhq4b%2BJke0rpa6Nadl%2FWLTf%2FTnmlD40GYbXWMgCEx94M1sqV2Zokqg4sEpKYu6efJU%2FYxxnUeO9qNeW%2B98vroT0CQnktOF%2FUZnaXs9hW4F&X-Amz-Signature=c59eea5f17d93f150ad6c44d3b3ab8c5b93bbb33e5454f71dc59c076b1e392f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

