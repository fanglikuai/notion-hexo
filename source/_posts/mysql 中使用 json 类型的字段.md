---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V6GVIXI%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDEA1TRigjg%2BhaBMK0KqW8uuCzYTf9erfLSSXVLF1jRGQIhANF98dC%2BrZ7PAF42OlUV437b8FfvLGXs%2Bx23tKivWW8wKogECKD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhGjDCrdIWk7LCQwYq3AOYNPy78L8ZWbtKr2l5z1Q%2Fc83arNjujeBMoZ6iOnr63CE2C67Hp0wm4bqakwKENIOG42b9NLgfHnrKgonKCW74NbZMOfUMIat5VHUxAs1uMM%2BM5vJ9Qi1xVDi1AidIHy%2BDnJhfdtOP9TPMvZbOkkP9tBmqlJnZk1zktqdYuDLIS%2FPa7Wo2WE50w0YVn9bJ6oMgXNzijFXX3twttB7IVRA4rY1FeNPYvJFeETCeyxflUteXgoUmNUaGm%2Fs%2BdeETbYDba3IHsLOo8qpvx1hb5g4NDebhG17ZpOH3wJ%2FU9HqYRr702IB%2FMo1P8%2FP9XAD7x8DuNYm9nrVxKdQ%2Bhr5D%2BGpOtnlsgb9jDO0Je47wyJbUSHKOCKvWcpjMtEdjzu54Fnj7NQkfnuOvKv0NHJ8Nyh5B0sxOs%2Fbkq88DlSfxDRY93r8EFPq3dzxacTF2MkjnYIJKEIlXUwOQt9Vubfhf47CD%2FB%2FToBZ2xuBIyGqwA76y3KUKYg8jCV715EGkcZSCuPvXtgiJBFycgUaRuJGHCgQhehQT6gY%2FPqa69OwjCnJ9O7joGCyuDfnDhM5Bp8etb6Tc9l7Zq0gSvPTNBonhCusumK4YqCM7sAbbOVzolKQ%2Bhjz3p4pN4RvGI6XruTD33cfHBjqkAR5Ot74r0138C3YYn4qrXsnRreqgFNTWqM6xpNfauRpD1GZUjC1s02oR08iUOmONkT%2F325nk2dX32riTJjjKPZJ3ggo1Xp5nx%2F5w%2FBhrGsQIvUpENRKv8JxDqKGRMGLPnfuu%2FDjaNQktdnv9krIgYaEAxgw%2BsE3otD7WszQZc2hJaGfCsmuz9pBXEtJQkam5%2B88HlZ%2BhOzHvCYNNLRpDrGS2td7q&X-Amz-Signature=6fbe96a2204425b24722efee0ddd53a7224ec213eeab388b0ecb56625dd20ed6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

