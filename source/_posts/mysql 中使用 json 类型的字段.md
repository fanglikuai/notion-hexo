---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2IF6SEO%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8psOubAN5pc1s6dN3E2G%2Bqu9mhuuuVV5%2FPTWPHgDwbgIgDH04a%2Bu4xy7byRij7c6tuWBJ6fWGJE0v87%2FXjgZ22EIq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDCsvoilSbS3YoG%2B6circA3S56Z%2Bj8n%2F1FS1%2B%2BV4dX83oUMmew9hczxcVkju6ue1%2FuB6DUPZEQ%2BsYpEfkNIy7pKz46zFvQ3awlA9aQqZ7XuSaif%2FGBlnhbbbyBkgq2aseG2GOKD5dgC9kxEjhT14L5K0BmP8R8qVZ70rVhzFcD3EpXLSZGnKXsBSkdQ0E5SN8y5jk8TBF6Pa1S36xiBu2JjNgOe%2FIGKfVlo4zTsrTlo6nUI9eWHIFOUBqHhCm05Ss7ZgaBqNPCFw%2BxRMEW6cl3NG5L2n0ewACPyaKbjV80xKjc%2FGU3iVr%2FSaEBk%2FapUFjsFBJJYoJaFHQxM2E4aZftYkVJTrA2SZjIV4yfPUWxd8Y64emf%2Fw5u88Ksaupbnkq%2F1rdvNQpMaM1hjjzpcyBahjnJJrhS27IvQBFby8J2hf9Hgax9NsiOYFHzNNM42qKs6KfO3yYVxgpcYFfOPZ1Pne3uKTHGg28e6sTJIVV%2BpYNMs2IzHn2jNlyB%2BzFRGMK%2BWVi1hNM6GCw%2B2WPWpGPsr1ATHU%2BoxqPixBtySkMEgKgUhxcHb%2BTUVJULUn27gLuQ%2FAT56v5glzIHrKd5gFOEJuM10LyO4B9QkhVH4%2BUzt9POcpiqWgQpSMG7NuzQLusTIxgNEOi65M51%2FdtMILY8scGOqUB21Rd4qwLxUwCQvbK4KU%2B%2BelvLi8r3P4Qe%2BeJJCStunpb%2F0wl9nc3pA%2B8tuSHME%2FdcX%2FyWi1FgJze50A0sXwrUSDeiVSBFUX01dvcXi6%2Fr7yW48vvTb%2F0QWV7GDJquqX%2Fz6PnnWp3oUs4SFwjMEak5Avxwd6KT9GkEeIgO9tUhohhMi5OY%2F8gbH9jaZRB32lDG4He7vJ8SOozxHoqfF8R43mpEo3h&X-Amz-Signature=92058d4b3a6367dd59b2d1521832b922b46b94a87a10ded8fec87a521b6fec72&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

