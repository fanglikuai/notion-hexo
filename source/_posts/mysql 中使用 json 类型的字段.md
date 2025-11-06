---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667MILTOEL%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T120100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEO58LYt%2FFFzUydgFUx81mnp%2FSUBASGi9czkfuksfGIkAiB7ch2OgLZ5LOP5JpB%2BePAHBZq076EG4wTRKCuG1sqTtSqIBAik%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP8goweDB5vTY95CNKtwDxkUjHvD5%2FtQ583qzGrTXDd9ZXHlQaR87CqIiU4wqmg0H8GxW9MALHHFp%2B4BarTU6A1ouJTMeTFgdUxa7B7z3Y5vjjteilra%2FZNZB%2BpzH8KvNuPi45Q%2FiT4Tcsvzch27lo5nW8FZGp0%2BlgX465x2Ox5%2FUAs9%2BplMGsCpkKlkg6%2FI8HBXKRLZPabVQQRoyWGSK%2FjKc3lXPih7KUFj0rox%2FtspcRdDh%2FLxodL7kFEDTo%2Fr%2B7sGUbzKMN0kLh2%2BQ8ZDQ%2BxcnHwLkFSL9g7ru39RvYESBYsA8GdmJkIO2Uxf5y%2BDAcM%2Bq8QbOfdKv7qbprAd%2F4LZTZ4nyRjy7iA47NIpljIrNjZ4RBRIa77dnP4S5TbC3PaCgz%2B7ow2irPL8BcKP8%2B9kofW1SWSW1AV9g0x6WpmJJADaXK%2B5SH1JIxPkN%2FCacsrDuIUCS4c8TtC42atrZs%2BoVi9p4bfeo189K6sRqZqTQo0R19wUZhKcRPL3vM%2B9gQ%2FKrUJAVjlUNT9W2sgQvMltnjB29JGJ6ZoMIKa0%2BLyNyYj7GSQAjAzFL7Wg3EBfBW0qvnYbpLU7Nc4KFVfV%2Bh42NyyUwX3W2fZyVfF4PfA9Xp1CS3WQHGZRwGXwD31wsuEc9Z3ncFcvDEdcwtYayyAY6pgEHwtEhXuRhmrzrHnvc%2Ba7dv0HwdCvBJ5B9uiUzWDV%2F2LaysggS3BfV9G%2FD6Hs8vKC6UVvdlp3ppnxQOJ%2B%2BHgZvmN5l7KDzTJ5sxslExzZQwggIqGEAp7nHty2xtBGF7v%2FzYpp1fIZm9HgHuKebQN1rVurS2hWg0%2BrD7Maca4dTH3HwVhbOrXui3qGfacsxnw3FqvDVqcjWMMTKfzQrNJVL06ibguiE&X-Amz-Signature=ede89fece0296b03eba1c41452c92bea1a9aab58cfc35035be64eab40fecdf73&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

