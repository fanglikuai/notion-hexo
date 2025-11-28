---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVM2U6OD%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BitBZlb%2FVHZ23qMjzHhQwoYsWLd%2Fqe4kmWWMPMRqbfQIhALHMzTXHBKMFeetEZvHSf5dQwEOGkPGMsBXJ%2FlzBw%2FV5KogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy1V2LjVtBMZswOorUq3AMFfyYAKpOAoZwr9rrG95AaZ5M%2FTu2D760hTXI1qYPqLfU%2FP8jH4jlIFKPH4EKEcLQPKM5aCFTimlIjTaZ%2Bm2TbYBNMIvlYR%2BxaHR2pvZFp6i1bhXn62UezqOQoo3mcNDJNQQdGDADI6BrVnAOlOXj2hWTfHsmvhhfOobVdZZLblneGtzTCDQDtDWQ0Z%2BShTIMJSJp18BZ0n%2B7ILnCV1NY4a3%2FQVp3ODQVWRvr6%2FFcAimOKR8WOg%2Bb0887ALd6j9Q8OIxHEP5k8sxL45gqbUO6E0nuAOUZeb3b%2BIEgkRoBWcup96PY4QzjgB9oQzwp56LKf%2F%2FdfuMlmG2NbPfU9r%2BSl6S2uQHriODzHqiTf3n9QkZLkTbf9IfKClqRBmkacwwsIR24f2xUsGyvdbqyZK%2Bm6w7NmZJ7i9bxHhIHGYhAMKKR6R5%2BFX8Ao6h7qWsAAULcuYAc0XmWqI%2B%2BPt8fv5pp4sScPVkrKeazlmzfl8u75ahRf%2BjwunKLga%2BmjJfVjwHXYuUT6swlAq8%2FjrXsT2DxXhEbVcnLJLHVz3WY%2BqzFN4xKRzrVnrSwaPaNMOUa%2F8pCcACERl2zjWDtxU3HIKeY5O764iECQLazNXt11cgJ3KWniAuKX319Mgk2SSDCO2qXJBjqkAaTwxaqSw0iZWcSSwHD%2F1lW3tEoaIh8Gfo9aeuwoRaEcG3LG1fb11feIx9J2Z3gqB1GMUA2UHx54n18Z4vctt5dngS0VOLk4Bmr34Qf6vUw20eJ%2B2NjHZeekLWhh%2Bu7CzWtbtt51cTrWIRpuGfVUs1VTREFxELVGUWOafj08EJAlzWiwK40ueHdmYjZvjdHpeeYA1dTe1d6JZX6ebXCTq83VOcNT&X-Amz-Signature=72b9c200321d93a374623f0f950d7bee3a6568887d63fa742e0dd016c99460d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

