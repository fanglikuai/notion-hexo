---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VS6FYRIT%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIAJB4wJvtTsAeHyYxi7Pnh8fDohWhdgWhKRd%2F102Qm3vAiEAobn268fdtY%2F3v%2Btvn0u%2FubBa5k%2FkQU%2B5UVSpJ1Fq8scqiAQI5P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAU9%2F%2BoaHwGm9TLubircA5F0gFFQ0Em8zxgi0Vu%2BMCk6n0i5ufuHtjmKfbAY0%2Bu2dGN6XzQ8KFMLoxIxzz89HyzUbS2jsKeOa0Q3tbvMIIiFb8wrUdFIjp3alwE0803kU6LJGu1ckqe5Hql4CKLmVYht62pvbIxmV%2BKGpojW2L5krl1ciRuBR1Cywa%2BSgMyZhJxHjd39I6HrBSEuU6wh4w9ZOvlp7f%2FFXE9E%2FgMdcCh7wrk63sVbMQpMicYRII7opUrVfMEeSbHhE551opyuakfNNmu72KXTCR6c6RFyMPJk%2BWf9y3fS7QdVwfjMnVGT9beNSN1xmsbKa8pHCsyVMAPFMNZNM1gfSb8SnXCdqW0vpVtnHC6uEPZsIvcRPxXIVI1cSq9PZuNo5%2FpQiUTUOhJ3h1kDQSh4NjCdYaxrcsOkxmtMsPARX6qcb26gIIiaEXI5ij4Z8kLM86lbToq7H9moiPZxQ5fCZNGDe7rCvh4xryn4Af6jgjUBfs7WwBard35oIW1HkhrUhf0zZ%2Bm2Wva%2FLwi8nXS6skPmSHSoakSeXEXH42tzBIDy7VAqFyyE9XdKmodMEE6gPkP0g4tSbOj0vDp9fSXLwh3%2Fs19nvJVMzPTURvE7E2BgUbxP6ayXoxQDtLmmMCIgfQ%2F8MMue%2BMgGOqUBwWHuVEXKzyYHXwyZAV%2BKxWoHuFYTWBSPW%2FGwa1JFM5kFG7XA5a1GGCowcSHR%2FTKZRUEDVBgLSmE3gjig4JdY%2BDXxStA956rO%2BwT1RpKgm%2BGFcm7who8tWAGoRlp%2FaaFcg4si6kKMjHllzW2Ybh%2FeQ85dVPPTOEizTKd%2BO5jeGwHy39VN5G8Ng9lgihEKvqPW9406rDy9f6%2FgNbjNHTdzgKkhpXYf&X-Amz-Signature=426f78e66460a6eca5b7d068c2be7f48e5f3db07fd448502cd4020cc2a97c98c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

