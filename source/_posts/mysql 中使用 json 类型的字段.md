---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JNSEF5J%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA9WfXara5A6LmqfthNyKy%2F7jvkIItipM5%2FsOXI6xa8jAiEA5K2WnQttw7PrNLhou8OR3zE%2FL3cD4ZLIAyYIsQt7DzYqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJKyl56YneTmGGJfLircA02PNFJiF6ygNoyvVwPKbihx5C%2F4Xw6Wn2nzrb1Gz0eBpjRrl7JHE0WBl%2BmulzLWyIky02HUnh%2FR0nBYqhW9ix6lwYF3RMArKPDr4QA1V17miljRm8SSAlI2c2FEe0vDIngWKs3WQqZanxI2cWKois9OY9iU2ij5dCMBkZRpXrGJp4c8TI%2FnuA52CYD74wIF9SIYed05G7uS3ASTDrVVsOGzjQD2SVecA58KH1yNtNiK0BZF5d4tNxYuQYWJ0TCoRc6z6WmLZwjmYLgGszLuz5TT7Hv4a8vvfSO9ZToY0NoIdviF2%2FYyKGBf2czXnT1Wnt%2BU6vLSPow6N5fcK5OiC%2FT%2F8zZqbJQ1%2BuLD9u7K7bdJztGP4%2B0MNHJaKHDNHNwcCBijFmjnYDoFq2UGpenjnGjEbt1PrciwRQRbiOaPWvuY58ZTfrrEdhWow3%2B%2BHBgCud4JEotjI%2FPkCuKIl2siJ4tdeRQxyll9ZgLvvy4LUgE5lL1I6hQLCELIRvd0w40Zt66SS%2BlS%2FAziAG%2FPEvbqwE%2FaVjl2ZHqblczNVH7iRwTfLOGrVErY7YchSJ2T%2FSu0zvCCYWalAJ2PKC6RDmh68lrYAlobdlwoPy%2BKIVXEJRyesqSYYn1YEpSLaaSkMMCl7cgGOqUBCZYZOXBDJUDIW%2F8rnLq1M47p21diZyih0YesImLJm51WR5%2BCwc8QXtLt0352YxcZtJuizONuoFQcTzr1U91aJDImk9%2B5jrYsrSXqblTPLxZokR4dz4IIiZ6xXbscJZm0qJ3x3LLaN2AiB39eUvYhN1znYG8jQLUUHAfnv0HS06CL4x2Tz8%2FvVTrmy1VRUMSIclxbngdOaAREbBy6k58F9e%2FskwaS&X-Amz-Signature=542eb96cedff497e648bd1846f3a1c973bbe2701cb18bf66d3c7f6e68f6776ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

