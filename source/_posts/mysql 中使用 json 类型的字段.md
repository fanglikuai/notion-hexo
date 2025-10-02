---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVL6ZJ3U%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE%2BR9%2F8palb5TmDS8w6jf7jIbTFRQ3Gkbz2ADGHe0%2BHuAiEAwg8kEhI2G5rFnkjX0CAuhi%2BcZwQgzSMNu4YnEyU6ZDkq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDIZVi%2FCyi8WOKdrPQSrcAxfV9lcQktcstPwtWnViEhSU44ipx94VSmQH1up45CZtdzu%2FGj6sW8glJCeyFheqRRGzupmIrsDGvFX6eGb5q7ICAp1QNR8b3dUba%2FAdwlApIliyiyEFK3cyuOLLHlWjAFgU1Ot3Jgt7SN7PiG2TefbROCtTh0rlDtvVUlKMQgpIt%2BScTJM7xtn%2FvuRKMwht91beyQLJrXJv6Fo5QCC8Ft8TzQlO05kpBlnP9d5bHx3DzMbQ16P1sWzNcY3de5KGVtAuTgb3h8dT6W9%2BifKCm39mzJnbbSVEjRuOUpDmWDD3Hbz9cG6h%2B9vvVp33MLtTdvfnLManXK%2F36iu%2FtNjvHZ7u481jDWKuaVHv7YGPUSvrya3nsoKC9Pd67WYJ7p5QnHBw6dkHSsLLAVXEtJPprzS41gXAC6B5tiQyIyVAHTyFh2NlQ49zxsjqrAZy9kGuQCg%2BQNySjPTpps0daHyd9CRBuVEI7VycjWVdRjN0TdWySkJyiubKFThyYRui7T2FrShWvnCAY5wOybyLZG%2FIQ0WZNu8WTI8t8B0U4DlslqagAYCzAmYj2ZIstvqq5X289v%2BV3VeAbtAjc3ldLihzfi9rXIxOmaibuJx%2FN7N3PlMvIr%2FSgnS42qe8aJ7FMPq6%2B8YGOqUBMoMc1hszaacepBd%2BYkG4iSNC5fPJb37jzr2QZazpDdDJabjib9pTKKODhtJEPI%2FBmX2rf39tBBJgMUFz7xqdIwLt%2B8TSZ62NOykKhCi3Hw%2BAzFzYgsQ8GG%2BuckPHwDe15dvHBulKgbNxFoY8DZXlI9iO4DNjQuEk8jR00D%2B1KKOCNX2OOmAiKLmw8B0P3UiKI9GOQ8r3jFNhsasDH8nrO74%2F4rCr&X-Amz-Signature=65631f8929f9eebb44cfbfcf669241c99bd47facbe31604447d135728289eec6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

