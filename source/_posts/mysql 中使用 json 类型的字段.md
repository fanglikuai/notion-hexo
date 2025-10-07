---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPIHHDK%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJHMEUCIF2amej4iy7pJBLXqLrVi8xHwdfr2VXgbSfpkU9EgoEWAiEAkHocBYp331yOkel01QVkaRO5KPAgAEiQVJf4COWgVyQqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA%2BJHbJbAKaHEzmdtyrcA%2B9NfcmwjcXbAz%2BMhSFJvCBcaYf7TUU57Iqa6jI81YuHNTwdjcipdVJo071wf%2FZ%2F6ANuP2DTjXjNfT6LIP8c9ay9d5iUKOCN3c00uLl49rn1uz4r%2BFGKp7VOsa4Wi%2FUKUMINzjkeJrvVKW9lV2xvt9jO3x5RMIRi3wJrvzukd33I69gIup9D2AvIZi0HagcxGvNlchZLVnS7AFwz7RWb6pOPqsf%2BfK9BdZdQ7l8641WFyEnhab0TWTQiPB1ZD2RMtKJVVS74GeDkZqDpVn%2FoK46LXXGfgrkwK%2B2zWNficuQIEY0gO1LL8I7%2F%2B0GJEU5o4wnGBAkwjdv79e8TRB2CPViIebtnvCrq7ORMv5BoHo8A%2Bwp2tOj%2BO8J1ACoxwbFmMSxFg3J5%2FFYOYTzAbM9FV4cf%2FBSwg58588tko08Xvkkcv3xrScRfE%2FXS8GHNdMCR3Q3KRc%2FvoCI2pl5cBF2Sq9CkV4R0EMda8bF%2F1i23asletL9CiBDKRpG%2FdQ4NhQsd6bN%2BqgaPF5Mt2WHu8jCFcla9qoMO5up7so%2BdLeGl75XXPprKy958uGhKT4OyKH1Rsvt7fltYrjrzoWOyFDWuiDLEtiOrT73rsQF%2BmRnqHIQhdlecNRgCn9OMkzWOMOyvlscGOqUBUcKB3yNcRJbBCTKcwHBHI9fkq70ks4iTXrPuxNFFqqNJpYdpENlNGhzRPf01%2FbkClV98to3Yrq6xOgWTq1RYJZgvcuNKL4tcOdkxvnj0VXojnAZ%2FWKlCupXGKWrDRobvd7NqNZHBZJPvVQOWwG7c3htnC8MbyMFnpJpBgs46ykMTO5Ny2gkNO1IYArLIJ%2FiUi%2FOp18mx1KLB8igTMpIwDw7dS5bz&X-Amz-Signature=12b100679adb1ed7f8f1443089cb4cf17a58b8f20318b1507fb35a5d3457d20e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

