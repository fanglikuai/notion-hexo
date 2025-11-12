---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673G7U4TX%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDCOR665SU%2Bodwb2rfRAXtqX6VMMElkAVn8FNnxoFOjKAIhAOYSzJ1cskOcv3P%2FXTqLVgbfiH5dJ5shR74P6Q9o32oWKv8DCDMQABoMNjM3NDIzMTgzODA1Igzg2kLjh6j9tteR1nIq3APIplJwD%2FzdgP6POzlLKL%2FSRryqGuU2KzYm4Hd1Trag76pvjb2O7uA%2FgTpNauNbrS%2FK1riBiHwJxNKXp8PrP9zqfybsgg1mo28%2B2B5DoxWQP8adg81fdCIndb6vNh2p6FqcGTsKtLpV5oXyIkfaWJUBX8JnUjho6CBNSmW%2B7%2FHuisUWh4M2LyGoBE3PtbbOQ9WpLZNMKWMMJPYOS7q4g%2BoEAn8yYBT2Xn%2BsZk1v8ahrxNupHBkPAwuXQ%2FjkjYjc9v%2BBS5Kta86s4DIFGrhE%2Bo0bpSNP3RbgQez%2BkLsrVq6wzCeM1vDoDxGqYSmIBkha44ixUaUuBP19GVQmpzyEy0DYsS4KvJgejtkQoFQQ1B8ihsvmU%2FZHP3YwPspV9WmYGlXOF7TR9thwzu7buPSPhGK81QxZFDN8WiBGJAD13KEtBZZgYhGiPNJ6uDj93EDeU8dOv%2B1KSf6edLsQp002Xhl%2FcG0kmaMT5tMlRlsQhSkcQTS6VyV9NEMgNs1vnzilNU6gemoegQG1Y3ku3JjIud5Gp2fLZz5rhxztuvU0DAJsPx5EV1n60UNwLJ%2BOa2ItWGyk4Guol6NFLTQ535Q0hzPqXVZn0MqtfIB1D8GvyXSYsDBi9nNucwIXb76I7TC%2BsdHIBjqkAWyfhTdFapY9eOapyN4W%2Fzxr0nSy%2BKIgp7df%2B8PwWdnUnXipoipw3660YiXWbiB9RvUNlLvUVhOsPtI8Zo1FkAYktlB1M5fKUypGwQSj6wJlg9ePjbfCExDGPQEsxiCVXqAKDohe0FEsfK5kD9oEL21fWOZeoJscxMK71IAJfoNjnjgNQ2xfw9AbJXLDLrfsxqzFfcMOmfeHaQqF69Ylqj6RGUwC&X-Amz-Signature=d63704ac5e283a5c67aa3bffeb8e5ca38c9cf504e0f98112f5346d03a883d2e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

