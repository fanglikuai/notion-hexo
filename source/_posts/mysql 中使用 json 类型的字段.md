---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664562JJEO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgp79sQIHKp9T8El4BnVQh4PxfK0OimXCbplFdBD79nAiEAkJmjVHjI30sLuP5NepCdT83BksXbIK9hg8042NucTgMq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDIAjZyDZh6aN7sUMlSrcAwA0NZaNpvLbiKzqGA%2FAL6V%2BMhq8Ngpvx97v2QgqpJA15zlW2XaPcV8qXW6STlAuqbwgrD0T4nzuAoBpI49pZko7xA1Vj9cAI2YBKoT6CHrB6f7XsbO05%2FatPqEhKxPPXkmIrdE1VVHuLsc%2BI0iePh%2Fig078VmAv%2Fz0qwTyDJX0oVhUe9Ax3uWMBL0FojgM57SA8%2F5o2SlcIMULR%2BE2vkklRg1GpWj7F40rbe73W%2F59fFZvI8X6QgixOWM1nCnXfm%2FRxl6dpoNQ1oDG5qoBuXVE1hmTP87sV9lasZ%2B%2B620q12gwMqa6jyXLxejNVajjSbLBvSw9X1uwijZxx3bG63IYaoJDWFaa4uYtbDN1e3QS3JuAZ9kOkZlVTL2yD1G5UIHn6hL8%2B9QRvCLxEdlz2DMIKup%2BqmDUPox8rcar4yCo9UkYw7USRSfzVL0TDsonsHtcbg2Pmx4DFiEnLkEBCMbjB%2B5FurwP9lIPu9CIzg95ThlFvzzdkvjNM5VrwAnpKH0%2FSNntzGrmX4ffa2jP4%2FPVsBYJR%2FtFQq4Hgw1QZW%2B3h8dL4%2Bbh%2B8WMoA5Hwm6Bea9SOTt%2Bv8TcIR26%2FvjJXGQa0gMf15xjtuvG7xyxKJhQ0RNEQjCnNQWw%2FkXMTMLaT%2B8YGOqUB%2FTDoZ6xltUaal%2BZB1DanxY1rvnIpwSwdy57VnDte%2FAlKYheVUGnQIZwujMeraV706dXHA0hIGbQsbWFrYHUQxlkcgRGbgmVZu%2BpxcVy89wAZBxlF9zFQL6dzFg9CACnLWzzx9Gz7yGBzbei3mzxCElYgXlX881o4NXo0Q15JLmKUovIer2DRRdzTRfLg2c2BBlss9b%2FRoKzsIupjpoPGFkH3qEO9&X-Amz-Signature=6baa64bfbf7a5e10fd5f4ee1da79352860e7d34d3a79e1b63951f41ceaf0fa3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

