---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZ5HITMB%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICVs5pme6x2opQ%2F6HE4OcwOVjzi9HfWgGvWAogWPRtM%2FAiBEYweW%2FbH1BE87x%2BJJRzQwkjbg%2BGk0Fu42vGRYN2SxoCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMlp9Ofo2yxz07YmUvKtwDyPRlAKsQpBeRjTrx%2F%2F9km9n1kvVRzWMPJICYgwx%2BzWZ%2FZ13kCL4BCZs7LhEVn2AARWR%2BLTpN%2Fa%2BHrVTxVA7Ep816JdO%2FxAROxXL14RPWeNQa8R6RgjArAW48iKeJu0arjL216IAF7CNuxHl1Bsky%2B7Mmm%2BhSjNa5rw1bOuEs%2B6xYlchlhaSsl3rBLiovvQpvidySCqPIUKr6QnJrE7tw9WEj7GqzKdz7CKOwisN7dETm8bJMlnEvKK3WX4mbdvlNL%2Fwqd0%2FKmMqlDUlnabT0Nb1IvTc3cplM1%2Bda68IHtjXMxjY0AmcFgoO7fd%2Bx70BaR8ZCST3kRcYqLwA2mNERHXsfodALmGPgLobZBEylpQ7GMK9T3%2F%2FHPrGnHhY9NxxEM5zj4ZnCfL%2BwKLfhDNPaUr2duKKihODZnYu4PQgsZV3RH5VhoL6zGQcGWYQE%2BWDCn%2FW4BZqGAaPeuHd8Fss9vgby%2FO8L%2F1wjQB4p9IHGfRc2yzb1qqlMB129ySYEQdAyh8UfDks5trGNu04wAr8cIxbKRGX4LkPSJDiF7gd84uG7XZciR9IjGb09Ih9nK4btWUZT6BvQQpIGB8pTmHvEP7tll1vWBWGr7rqfuzcHn9vR66YPtfJ66O3pdywwr%2BXKyAY6pgGxraPqJiOo6XOz5Qfkif0P7oM9Pce7jB8piylLP%2BQQ6%2F6mLmeYkWEVgewKS44jLkAcIgwtNQqYLsUoG7WP5bWNU%2BNH8iG45na6OKoZMpVWO0Nv7FQCdvj2oOavfsG3QJCoBqDK1z%2BBWzBujN5exRm5oaocYmaOQvcoPO2LEphO%2FLnc5%2BVb1R7N%2FewyyEbPA0Dr5gdt3HPGP%2BKLt9agLA48BgyxtVeU&X-Amz-Signature=2dde81987e96a0a0317842c0ced40aa698c0a526decbc417328acc021723e451&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

