---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ3MFY27%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXHNYfnlEUCoTYve9O2fRmLyZzJFOq8JydxrtA1vvYpAiA3QLf8AC9SiMrNmg%2FRQmRad7HSZb5eNq1K4xfMvpKj%2Fyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMyOQzT61Z8Hz7XHpHKtwDqru6ksWW6%2BHB8TK0Z8TIPpu%2F9BSh%2BNteD9nYpvxQP%2BBrBrXpBSsMY1qWXl3IYzskTVc2U829tJ%2FDkmLZ7pp4DXxxNwBrjHXuKXQ%2F%2Bv9%2BGBEp4WSeYOBZhgR63y2Yt8NPBtEf0waDPBcyEH1PMm4Kb4%2BTk%2FU%2FjvNRNmn6LZy5M2LGlYDGuvHojq3VcIwjMbrq%2Bk5B9BRRDn%2FWuvcb0wenNlUjPoZS30qYlCXRUEkU8ZIR2nWHBJGXNjSbqhvZ9fKKHTuUc6F5ejKUnL76c6I0Rgqi%2Bv3EB8E11UfYquMhuCiZaARn9YzXwAAJdLKKZEEV2wd%2Fy5iRvTRZ0J6N5HXbTfonzkIwlLFb9Bp8ngkpomnYvegz9BsxxgNa%2Btx2YLTxH7jp4EgHXqMLbPGm1nUkx9dEicQ7PUk6G%2FRr9mW3A4wU7V5c5UZvW7AFF%2FRknwXB0qDLKXBEoErxL8NpglEFNfhRBnRReHenagVMQthjY7Wvo7589Vt4y8gaPMu%2FlkFJx2d0doRu5ynSiApjXnmiOit4roSNdeD%2Ft%2FoRheuwTkTGEK32di4ri0zXeN%2BTCt9kDMmjKvl%2F%2F2kn2K7Vrx7LO1UBXsdkNtFAPRg828WQfi5SyUWx2K5ohtPM%2FFcwwLWWyQY6pgFt1JeB8KDLwEWOvcrkJvtfIfIjB4JJkzDnooS%2BF8%2BMNZWe7QpMjO696HYt3960VLfgj5RJXbwYlWPls867osAjbykO0uhf8OKonMYcMqbuCmpqJAUFM4s0pasq4lIMmyyfrGhF57lzXmpg0IZhvSZQ0sHSNKegRPNuskAnL5iHQZyB%2BOJIePitAJmL00babwfCq1GuXyjICv4zD2iZTdJNJjpT%2F%2Bqw&X-Amz-Signature=6ea70b7fb3ecbc9fa608dac7383123516f67b3f8ea8cad4164e2eb935e4bef7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

