---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS5JKU7D%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T080101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCT78bgqLO0YywnyagS1S7xZhQGQvg%2BR1eQcch8vAbnCAIhAI4cxxnXlBdYoz%2FHUknIJHh3ueXzAjsPYF3%2Fp3monT5dKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3P8w4CmLcWxBXbXAq3AP9gVMzZj8J0dGmWcgBzpXy4nDR%2FbrH1c0pVYDz3dP4cfesnJ8Ucd%2BaAmPMtMjx0fpwVYKfTp5gtYkwIgfxjI5qeIppS1aNid1yCKTFWsxTUBkQJRSS1FslF6gOGvbZb4z%2FSzgyeaeebQBfDoobqRzYedJT%2FzNGdOLYx1EhjxKk%2BfAsqQ4imrdjE8%2B8QYnvJY1CMfyX9aYIfPdVlq2OzJzPRARX%2B5XfjwGC5B2TofwYjUPC4vsb420wnF%2F%2Biwrnuq%2Bz25wSPXJj0NbvJPl4dR1zZKkjQiZ%2BzGWe8lp1zoL%2FTHZyfUBe3Fsqk%2FC5uUfd69EdXor4bwKq%2FnJkSBdFeFT8cvBPhGqlYSTgX66Cn1GxZMItzJW31vbQikQB1U5ytjgyGgzj1gAFpe%2B3PrCacCTh0iwjq7lqW88M20kosGteiKe2cFCI%2F%2FuMalKJJT4OuatvAAj3TIQfShcwy8I2SayI1VVzSQDml8AjJgGCYsAvdR%2F1pEh9uGznO9tCymC2JyEPOziWBBH4JF6yhQCT5%2Fp8LvgFTFr1kvc3gvlGPtitj9phwhRUWQ%2BPxfmpHGbqmaj1v0LhdjSTfFrAiGJTQmNIGBHpDrWXjdKCuP1I1Klnt19ePG5WKurEpQU3QjC%2B%2B7XIBjqkAbaGMg%2FYU11lgEkW7ADjmZgNTDX7k3WNVK9aY5vEwdpRJI%2By7DUX1gMgvpbacIQ0vkv7g3fdUb8yd2cci9hCm0O6ElkOPHC3AKOhFdV99dfBQnpZyzLKNhT%2FtGHZlHvQnqwrad1YT5Io%2FVztfdS1VjSkTKXRRuixC2ZHthsQrp91w%2BkUTt6OJGHyJXlGuMfZvVi2ew5msigFWUKI0hpHLoe3k3WF&X-Amz-Signature=34748cc0a318e101345186b0cc6fdadd0aece04e91a47b7abf11a7b9878fb8a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

