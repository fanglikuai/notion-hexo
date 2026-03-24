---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVAJBQHJ%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T125107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRRESIxO7PQJFnAEgCj9jaBiN5kb5p0Fm4%2BXIr8Xyv8gIhAKcnIFF6iAqseeUtOLoz0KyY5QajkY8aQqfj4P7%2FlxYzKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzkfEyD04%2B3fnm3qEAq3AMOlSUsYuV8UD7gOU6XQfL3ilkdvcr1U4O0BewG8RTUqf2kp%2BrM9sZbAv0ISMuYexNtZRJi7Ffh8YLyyIZs%2F3JWgLmO307XZlt2DGmfAV3wDcIYsn3ymy427sW1fwdUGzUU%2Fi9VSw3vTb%2FAi8aypbwJp7uKOSaxp1mDgjr18QA9ASKpXYgPMfIY2u2VZvhh6%2BvdQsGAXDazQs5jH1VBjxPyLqo4dugTELNJpz8E1ecFK1XLRnUz8sE4Ink6nYWBtthsMHPYX7%2BsOjJuoZgLQ9IjEZGfGtoCKS83mooH30TfQJobLB110h%2Bv0yxjikJKanY0eI%2BupnpMIMBMpOo1ldRbZKlVA1TgAQbV1GhklZ7gwaKiSkfHN6RBVrNHsG%2BmHVOjgrnWAGqlJYalapMxo%2Fwp81JaUv6OohIkQNSIx0Ggb1xspI9oIQmigUcnnsvP%2FdedJF3yo89egC5xj%2BHrOUB4QbwH0IdCAG2mam8S6IgU3QNEqtp6NexkC2lf3IR33aVZ9Vy5LiDNQ4OwYV6uog9CAcBsXCmpImo290c78UubJlZLmxnqlD33QysCi%2F45B96Td%2BB1%2FeuG4GHszqqDKvnoJF2jXXkK%2B6qMbCKjolj29Uq%2BldcJKcM%2FZ7%2F9WzCG8onOBjqkAWsVoK1AnJaLPCxmXmHVt49JUF9aUdgFguxK1s19CipEE9nQn2t0uKWIZJsSEm3wHUEBvUNm%2F3zGVZzJ%2FsIB8mpt1weB4qwRCzIP%2BacFtO%2Fk7v%2Fbx%2FJXku%2FMoa4IVo5P7bD1cZYRFNTAZz6%2Ff6D0fLG6xWcgXK8b%2F5wAhoCxUPYnmiNueH%2BNn%2FfMjYZ3Mh%2B7e4mhsxG9AzN7l9ROJ32h0NQCUGfr&X-Amz-Signature=5d77ca22935a42509ce0760bd387b9e87ef7da4f5bec52b64437ac0b0768bf1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

