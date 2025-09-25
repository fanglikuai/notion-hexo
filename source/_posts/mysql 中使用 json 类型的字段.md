---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y23NZMMS%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDtWTGz5Tgdj0DvhBWhwzHiVJQ2Awaqhqnj9JO%2FuShUmwIhAMsY1xeXucfz5%2B3GBkFZBw4SECbRoscJa4SRexAbeIlfKv8DCGgQABoMNjM3NDIzMTgzODA1IgxRgziVL3REewBAHLcq3AP3ZEMqWUF53ozGiNJGBuCuzEIcLBVGMsr01gvowtt%2BMOKYTG%2F9RATlP7108biTCYWiFujePCxBCXiYRvOF7BCp%2BWTuaf5sgm56zh%2B3UTruHdJMrGJRQPHsAcTT6iBek7iLYfKZGrtYE2NgLsjDebpvjgEUp4apF8BZUCgKcjVFa96gBryBpg2vpPqcQveqpP8wXhgpjkxHPoDZJ6UIkJ1A5MSwi9d8JcQqeS7r1FlEYMH3n0bzbuF15jJ4zslhkMt86zTa4MtPE8XN9qnk%2B%2FvhwYnLj7705LFqCrcFDXC05asz1ZuWpoR%2Fg8aPbTGA93ABxSnN4WG8Zi3L5jSHwLCwsa5%2Bu%2BogJGG38U%2BxN3Fz4lwVDc2W7lw5swVxm9vqu%2B8ypRyPsnZs0kxO0G2dWLwbJL8NI%2BSAL3qVDSo0KN6u12Y1In4uzl9azCZ%2Bhb%2BNcC0wMd4zfQU9zhb1CNLBV3j%2FbZUIgURafD7XIK8%2Fq%2BO8kQ16w5lyKd5mH8o8aImj6sj2I2jHkCwe9rJVyWzgkAP4AjjKd2an9qV4BNbq5cMDrrXV95nFJLzXkctKPk%2FnA7O8gF2gE6%2FnrLLkhCNXLRIbDhjlzopqrNYUdm%2BfaVq7mdTzMrRUfvyU8XyhWTDF6NHGBjqkAaRjGN2q%2FmY9wjB6YwNNJOyo0IzBaImRY3VDYzYEmhkkA7o96Q853mhX8VhypNpWDcVLFoQTrrWHFk7OcoiN8kJb0yBoN7HF4HNG9cddXQVpb5V1PJzukDzqYGY77IvcTVClwoPMhV%2BVWumOVgku4jQMtjyQkn4G6wIc3ytE3okVj4gDBuWgqcf4NCD%2FqD4iX0msm2N0kWTocTw1JD%2FlYr6leB7q&X-Amz-Signature=38dcf01381dfdde8ff8a82d480bf7afce475f442aa7c791a29f4c21ffbd6cc6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

