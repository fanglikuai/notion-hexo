---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624SGGBZQ%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHW2gMmLa2uyo7FbIHnzFfn1R9aAOiQENWilVTsDgr23AiBZik9JaHOkBnwWZDl9Txexy8vXuQqh%2F1rYq9tvtNkNJSr%2FAwhpEAAaDDYzNzQyMzE4MzgwNSIMuwcXRq6Whgs41m4GKtwDIRpRmYCmp%2FG7mFvB9AO38D3HDYmQ7f77l77zHdJ01HKYjbpNd93CsR9qeIOgBCyg1S%2Fe%2F1tlv2dqbWdWr%2F3JmOXT59gNfHTT%2BQ2%2FiyAH911VSOzSkg04yHOhWPg09lln0XaI9GPt%2B%2FeuMJUH8XEoHdQkcsp2ZxJD21oWIKfO2OMB0fqdin5%2FLw7N9Gyw7xTF46cP8gWlVcrAS2pFyxXxTfutUda2Ac9S8VznFOvWzNKSqPIYKZEEFbUNRFf2GL8OYdts6NWOBBdNUeHYUnDhS%2BFAYfOuVfj1wQNy2iLAdRIMoLrD0s9OJ00J9NXWj6jMvHTN13vvB8Tp17oVD4Pq7aw6HEoJ4Wt5PiIHBz8Tz%2B9qfA0bviwJt1rPmZx6o5yZx3ghDBc6ylRPcHEl87uJa0zjKS98kYr38iI8PY5TIXXXVldDgTnIxmTqZu7s%2BBnE0aibpVo4KkUjvbYU%2B1KjW2UfP9rT5F5vOpsZGfPg572rhjNfkr2kE3FzEM02QAKYxttD472gk6%2B3Sar3oankuhYJdXGji7gphcCdl0%2BfKGCkHIGZzfCyWM33HCrgwrXtgHivlM9ZbfmxFwgci6llZMIGkfXPcenYZHnRzF6%2BhHtnIM%2FIyPOdfzE02tIwheGGxwY6pgEPaWsT9v7tVYgouS7KoiPrgcx2oihcgqr0BDPaHaAYx0XZWjk3TvAbPFQ%2FGeMpJRiys5qVJTwspk325oHUsWXLT529koKxHawjK8jBVksRDSJa6y77vLkYWsLTqeR%2B4aGxufmdxsCB%2BAl2bSbmCwq0YUohZEocMohg5kxYgIJEyZD%2F1YR6Qy%2FNk43xMx9W2JcH3cpRhSr%2BNbhFVCOr3h5eyynr5dHK&X-Amz-Signature=626d324e4d105ab971d1c60b2908f154e91be66deff8a00b79cfb98507903e0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

