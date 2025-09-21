---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666EZB64GZ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjR0VIzv1QZ3Yf37Gxj%2F2uLpKe3bv%2BBbDIm%2FvMpDHSpgIgYTcezxOPiacycFiUCW5kVIv95WWLg9bKDkBTELGTmwcq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDMIqJhaw%2Bam3LlUPdyrcAyfyF%2BtYfDRzRQtpPPF3Zof0ppghHEDyP%2BzROqrGdBQhbyfgMxKhOFSGCahIny5JpU5qqIJ7QKlSLuBjWmR16cQOuz49nzDkUXxIww9b0QggeimcEEpVCtnaEzO11VBv6QyYm%2B4dkZ39Ep7%2BY2nBJYrKgwCy1IPkVyCFIRZJyuvvBHciNyVuyMMTJQu18WhTqef2ug6XrnI3uQADm1kkoVozq%2Fxx2a205%2Ff2ohpNoKyaXV47mp4fLwerLJodBw%2BuVIUqqPeBnuhUzRyxjZxnOJUEUUMjj%2FtC8xnwsf%2FOpsYrZRbJ1H1%2FAyzjeP6QSSn9hsAOapLwRgArWYwoJ7Pk33V19qX0FfiaWtILjZYQ6xbqyOpnAaZNC%2BLXgXHGfl%2B9xWQYiWZqJQn8VP9cyZ7y%2F%2BeVtSJWXhQJA4EHxuSvloHBUWKk0snGDrMjZzzgF95QUzlpjfv0%2FSPUa9aspJbWfdN0Vmn8KlXRq7xjUYZ1M0OCEizz2cY71TynXMSvaYzMCv9fum%2B%2BhwGraV5mwlPRjFJHojAreQEicc80sN4RPmT87otB%2Fku5ac0bRt8LbzORiA5qljYFKo4d2X0iEus%2BiPr7U%2FvAquayECoQwnC9ZFh2n7dL7wAa%2Ffpv1YDeMMiiv8YGOqUBu1UouOHNYDXFy2HJA2NzTvvPJglhgqx6OAqR6q17IjMf5wmd8Y1GouE1eN0fcL48GBJkp1NDwNkbX21jmVbEKak4f3JiP2lwYVyowJcozHcxkQdZdPSrL5HRCFOBrwplWebFMRjVf7mXO4lgjIQllAaw7fRvf3g0kiqQE6QYoIdp%2Bjav6Obp46gZMtWhqaRcgRLLuWYihIc2pGJXN2t25UOI2dSo&X-Amz-Signature=c204d515b9bf7dbc334686fce29c8cc4f90052c6ed263682148d7b49df9ccaff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

