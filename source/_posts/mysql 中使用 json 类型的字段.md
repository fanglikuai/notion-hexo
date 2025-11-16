---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYNDAFRA%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDqHplyP1TrMCVgrSpaAAjjxytjTia9HImvmutmEGUUYwIhALQtGPNF1%2FLNOyN5IlEfH2NeMGDxh067mzARiH0QLS%2ByKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwhxyp6KPVUSBvp3Xsq3ANGZMx8kWBNYvscdK6oV8HWnv%2FufoTm1YeNDfzbjPUyJAwicBUiWlWt0g1wu4EBdCENQE9ZNKcRJaYT%2FaWLS3dDX7L9vVq5s5ggPq9cn9ygzSf8pmh%2FOVAcd03uNAX1TuPn3ekg4rjSQbM%2Bv8OWYXKmL1GpotickKaFsZXftXekQikfAIw8hM%2BEtgvuq4uiNRzI%2BrxZF4buJ2x7TpfJiTMd6sMW%2FpH0kSxlWr6gx6FE4MhB7T%2FRehBhu2iXw53EQbZyi5wlw%2FJWtm6gcPzUgYGyL2szbeysFKqSGNtYNlp7p4zAFsh8og0KA1DQChvh07tsC1Peds2QW2X7EhRtC%2BAOXzL2IHqKe8DlIePbV2mT8sXv%2BTIQ6HN3nbYfK3wUaGrhACrWAlYG4QSlpIPDzaWB96%2B7fSqsh1Ofo%2FwMmpavOWpEEhfHQUfHNW%2FKkQnhJxEsHk9CVpH0kLmVuCEGGW1k02mq6qBnS2cMH%2BsNAgyM3g%2FSZx9Nm9fCF%2B%2F%2BKbbWirnRC632Ikv6TYA84BuGuREN1I0vlaSGIDyrK3OZ3UIFd9ZIqEaAaBQ9nXnDUEuzUwR5hpYTXhnNM5Kb%2Br%2FewZD%2BraQd6neivg9utZ0KtLIk3Vd%2F3SHyTOcr%2F6RSGTC13%2BfIBjqkAVk7YoND%2BKHjjN5RNsNd%2FNSeWfKtOikTeB0%2FKtZ%2F00sV%2FsMgoNLHcTM3PQSJEDk4O9ElwL85BbW38ICFIExH8H58W9f6%2FW%2FlauNGlXibP910cfmfQv%2FncSA4sGtVhCZAdgJPv20pHftdl9LO6eUjIlmmKWjKcKl4H3I4F27x5sPO1dEoufzPAc8ioGNCXEkvsmGpqUW8s7EoYIqDSO6oj3NYhxov&X-Amz-Signature=da347d91dc86a1e59bec69b5c949459ba23ddb329b1173842b827f76e9b9f5d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

