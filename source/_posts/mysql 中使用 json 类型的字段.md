---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGZWINKJ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFyLwsgm4GfkEyfsJ8UYpmp1EL8KaqgrzwD82znUIsVvAiEAxL92iFaCe6%2FXVJ7mIWgaq4VPgvPeJJHXoGUye6SkEjQq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDJyW9wJtht50pzWMBSrcA1G9FRG7dMPcROEFl02NU09tNuWa5rj7Bwkf5Ke5QRAiY83Z7gccg4XJkF0aFefA341CH0kHzxv%2FKgK8F%2BJ%2BrdqC%2BGnlIphd2uLxAgxcVj4kAO9ll1ZBR3aiC3ipTsamAbHo7yHEt4wWM0tQS7QwdlBif98RNYHGMzGQDy6aKWCqqbFpAo2EOZOrDl2A%2F5luc1uwuxII3LJMFj6PF8DZt4AKwX3sInUQMy2vXeqIIsoy9jhuV07f5nMyN3jpsVWa3L%2FkR7vQCwVdhf1fQkTbWIaXvwk8toF0DFhXlCT%2BFdtQTnSDF7mrZ%2BD0KQt2n6qnXWmbbkYNKc3S8rPEjGcqYHUDOEFo9S2AaojbepEcPag8AfcjamCxkXiT8mIDSVCyX1Hwq56atDvCAituDfs%2BKPuh5hHA8qezsq8ZMmZzBtX0HpyxgaPK0EvM1P4cBbEVHo%2Fmybal3Av25DCnYFhx3YVZUN6GKo8DofIyUWqj0cy7wD4ckuPWnY%2BccmIMFl6uf6nmdr2%2FRmMkKcEuzSjXACEmaIUh6Z27V3VAox%2FidMYpWX6yL8XMR%2FVEeR%2Fp8D3NEZ1gL0xzacqGtzecv%2FUfI3%2B6vLoFZdKkNT8bm8Ht96wmjDZRnQPJ6%2FjUFI9GMPn1%2F8YGOqUBnMrKEQF%2B%2F83H89WFliHgjDX%2B105xSWgeOblHTZear8B3x%2BcRXd8A%2BOFad9AaoclhKG5nEvKx4svcYTfcxgdzLoDkK6qkvRzyqhpb%2F1oKqXlEUMwgkZmRbnZ8wPpsQnugVSLLhcSX7vXBN%2F%2FTZqQosEqfbIvuBxv1Uvv2d1KUZVxWXZSIsZkEJkSQtjnHwm8IYS3QEcgpJtSxcxwrM%2BdH9jONjOni&X-Amz-Signature=196b635d5069cfc226cd5eeb4372e806b469eeb94b540652a0b5cc2e54ee125b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

