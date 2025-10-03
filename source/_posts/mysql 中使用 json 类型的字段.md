---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYIMW5CW%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T060122Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQSJBwMBmj%2FRTTAb2cHShLD74t1byXa1UgmgjUhCFI2QIhAKDf9CHloiDNGSfqT5o%2FQYAPecuTdVoMlKY7mJ0H0QcMKv8DCD4QABoMNjM3NDIzMTgzODA1Igy2e3qgbSHLcIrJV3Qq3AMjP8oWSz%2FX5VZLTixm1YZXU0cWSK4%2F8p7CWSUoYMBZZpTUVWbhKcyBiGPeHr7HTtaNcVHLoBVjyxQrIxCB%2FmGFTcrnMhk8i4tk8EEYf75zoSMqVQYrpeY0juPZK%2Fi%2BH%2BWCt0AGUYfRA06IieUzixIDFcw7wkD0ItahQzunaxQpdUB9f0UKeX9niQvVVLI1NXI294Uxc74KsGLFwdY4bqMuqiWLlvx67oqrwWnvAOcP98UxEAGmFYaA%2F%2Bp32VLqGtBxbbb8oJFxj8Cd5Hkd9MuQq7uJat5EQDT03T2LOKaOVjrgz57LEOugF3j%2FnnYTF79V8zNBaBAP5AVXlUF5RsIm1eWowTbaGzazCDV77h1pYsJLV%2Bug9z7t%2BMYg08qMy0IiYhTTk0Q7dVX1fy9dvQVWSyJPECXFVvaOcEwVM7UdyYw3VmOeNljMCDk%2BmB0dX%2BY5CjEa4Mz1v35xDvJbuDV0paJyx9KWlDJ4hM7BsSVMtIJhIYHayvVY%2B1V0A5zYoZvcKQJMXlz55Oe6hDs%2F24jDD5fgRcv%2FHp1YSvgLcn0IGJ2VW3WlZ%2BreHUTWlzdohzs%2BCgHAPfSVE53BCLUkChLehOYDR8sHWuIFsV%2FWKvfos60IdReymdgEjR7yJDC%2BrP3GBjqkAWoRvBw8PEXfcKYVQhnEkP1TuQhf5da4xytggw69hCJhMDC4lEUkgywy%2Bg3Septdj1BiA8GGQy2AzA%2FwFTzcUEZG%2BmiF96K%2B%2Fb6pWH7jubtElLLB3aIjh6DTLwVzv%2F98d1nikKvdDzrBLNI1UrMdI%2BbhtZFycc3SyoNIZqqyTWcJkYioBlTbV6Q2UJdMJICspfx1i9%2FYlz4nq0zg5JYpurxe0uO1&X-Amz-Signature=461d5955779bb66f0c102da2e7c991f4cd381537139bed38d5d60a66d38c6ff3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

