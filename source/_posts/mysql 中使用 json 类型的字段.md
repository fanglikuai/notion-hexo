---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZA7BIIW%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIGYjEW57zbagRZjpjA4y%2BPbyK0rSFLd3EGYU7cA071SWAiEAnkI86fwh2WVHZIC7F7GhLPJO5E8OTMubuMAHej0H5EQqiAQI%2Ff%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BtfZeH5BG2CL%2B4WSrcA70xYnS7pWuK063mis5Wkx24IFfgl5HmPgR0UISGv%2BdX%2Fp2FgchNmAk9opRr%2FS83tBoPX0U8onAlBfefF4ltAKgo7uyPQI4iqNIS1oBDmZSwGLbLehB%2BEfxaC1Rf24RUoudeqbYqPuS6L09d0bmlUbkn8%2FEqSYX%2FKjFURG8b7IGGqpKFKnjCu%2FnVMvCdjr0g5AKJiYcFJRPD6cwugMBK3pr2ykwBtAWRR6giOh%2FU2O3VjhX4cT4mxM1NXzZU34gc7ZeWKjqGhn4ckULSxLFOfwkk7gWUJniKNlSFnrDxUmScQb60lMH6RlED4ZxoSHMZIAwuytZTiJfEdeWCulWHqzIiuWtVxt1L51000LaNDIQn6matkH%2F8F2%2F%2B15L%2FAJwYOWRwyukcw9tLF406JL4qsCDm%2B5zfuXhQty%2F11s6wyybpY8uNpxnQHR6P6EakjrLX7KDIw1cwGxvK12kda%2FTmIqQjgxmINlIFxxUY%2FICVyeKRaIVY2XsfQHsskCEeR2WUvH1w8ERgTVNtc6KGLWb0fBqYPTx%2B1%2Fw2Gj3cbM50%2FirEJOiN9kzaGlKyzb1YwoYJXlglfH2jNcg8TEgsNgCgoPFgqdfLY1kC4GBvDJrEMkU97rjh5y%2B3fK0udwQ0MK%2BN3McGOqUB6vXfET5Yv21P3%2BqmQWjIOdqDUmcxBWSCtqah62u4CeM9He5QLY0f0lm1jgSW7wAgm0GjOcIIwlFhDM8RqaHmVdvDvjNhL4HlBw1h9IALYCeCUuQKXW%2FH%2FC8HU1N62LAd%2BCzZBE0bEFoQsMSR76DxLFmk8I1guWAbSk8TmYH98wcq%2BmiM%2FGd9%2BbUnQZHrZ3RqD3en%2Fu0Fe%2FQpU7onervJgX1U9Jy%2B&X-Amz-Signature=7abd55c8c792c32e6626cd98a1c266b0105c9f1817a7c65c242372da36f13c1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

