---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZYUYVYK%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BlVIaLBoQtzeThaYr3JQIHXM0Y0LmgtE9a2FDTQZUvwIhAPfQvS4UcQNYoGd8RhrrNE682vgQemhhO8BZTGQJSopiKv8DCGgQABoMNjM3NDIzMTgzODA1IgyT8KIt%2FtyXGnrSclEq3AMKXCAjEnxatBBMrrWjmcIGh%2BMsH8nZiW1IRtQn%2FSGrNS4nnRe2i2pD3WLg%2BqURRenZl2En2KPycCpbQQ%2F7ebVbTDo%2FSrXl8O0zn34xtvtKSqozx7PlL3KFLKLql3maBt8N%2BXZPkAfCHVT%2BEh8BqHJcNHAYmxiBvVMriBeV6rfHPwG1%2BWn6S2ZwUYskT%2Fl2Jhq8EJNbwLSo7cMMAgUMWiLF%2FT3%2FhYBgAInOuZQq23tgaPJmZGlEPHTjLo6kNY6Dppo3ZMPbyRc%2FaOqtTACRBhggZ5LV61T%2B7rBqYXhJ4xir10CNE0mvmAzuzDw64oK3K1EM3iblugoAPFpuwP3FC9yUoK589QeL6uQp7joQI%2FxzzcDuHLc13b5PyK0nMCrqcPDpf6OuRCrTm9GDH1mXIw%2BxUL3TPxmgUIwK1Kz9U5K4vwiy2tcFXy8nTbxvhRTOrRQf0VBhHlGnX8Ula5ObZgdc4Co0REBCSrV475HXgPzHmREXTkJg60draEz2x3Fq2aIAd9vl8Fx5F6JjuKNzf%2FmLl3RaIQEulwuiv8gUd2AYQSHVZL3KS7IRwBilj3pfuP6seu1zfwLFH%2BbAuuVQqU4wC56YS%2BAn2zWLYK8ClDebQhafTs2d8kkHeuhC3TDG6NHGBjqkAf7M3SSgsmvc7DfP7JD4JLMBUReGx9%2FbrQlMZ14%2FI4r3DzzxQ0X%2BxUoAw6EXRRXU%2FWJfqrGg44ZIGc8S7mqye6nfrZE4x3hkPhuQVRSZuRDfvoeTbi1G%2B2NOEKefMlSa3Apf%2FGRhRvXmHFeUWMSFhwO6hjgtUkcfASl7lEkFjZYECJiBcVi20ATy7HOjWRpQ9XQBGpZ5DSn3IDqNEH4eDWMF7WQ8&X-Amz-Signature=09b2f8d1ef7bcbd2465dfaafbbb3b1695f3409598043286cd36cc2b3f1a54689&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

