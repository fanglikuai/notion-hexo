---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBGN3DOI%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDLxSzgAdeSCJStTMx4BQXcWQIpII%2BxJJ5DdFqYxUy%2BsAIhAOSKMNA0JMFm4EMZzZcp%2BzErwwp2kquwD5Uh%2BHDGS1aUKv8DCAIQABoMNjM3NDIzMTgzODA1Igx2qX30h96Wd%2BaAsHUq3APGUq6aiEJsiTcFI7%2FEYhYi0TV5CPblZpF1bWbL9jEgUEavh9zHlaPbe%2B3gsV4v1T7%2BFZzOr13bNyk65v4FXfNLj%2FDARxWh9Dhey%2BSuvA3OwuC3ymbRUWsLqRX9p2ba0D76MaY2wMUB0BSLxhGCFscx8jlWdLM%2FI3Mb7U4ZmfJXebMQp3Jxlu%2FhoEELzSkjOPpN%2Fuj28z846rqev%2B%2BhA82DNowXggr2SXL6qqNYwTk5Tge7dCoV%2BqCUuaP7m20DG%2BDnFDYn%2B6PAzjpCHbx%2ByLSY4Kwc9Fuu0G%2BkAxBig9WKRNnbA%2FZfUPX1uzAj6zrBOpzHmajlkH35n%2BQtbvtZvjoOqH1qmaX2zQa%2B1ZZYLv4edag5AN1o%2BIyZ%2FCGrt9FVBqk2yG7zKacTn8TFvrkmZ48VHhKmJS1T1ex6vGo5CCDy1XYjE1yadgbMzf7fu9zHGIzLORp4dGbC6GpTdi6FfeQpV2sw8aIOmVaRHHGdCpdb%2BIL1Rv4GWgC9M1r61f8Kg9pCqGHfaWNP%2FVxBcnx1Rq5YjgzJw%2Bgwxrjnl88bZe1lkyE49brIwYM%2F8FWvae6y92MdsvLa77XY%2B3gVT3B0xzG9CXtJzhG6SjDEgaZtKK3TR%2BNzcX4M5uoNHeX6XjDY1sbIBjqkAfbuIASCcxsRHGuCztfVEKnE4Xb9HX6iimsHEXPmJ%2BZjYX043ALX2ekkUlyMX%2FkVm7UHelUgVWTLNR6Yibx1iZNDKROQ8oDCpocMfmqjo2su2pLTjnDcVTACnp4BY7KzeUJ8w5kGNmXiCYDoWC6lScQLSMb2vsH9STMAlEM3JOVItKETzQEwmmJva7uNUzqXMR02jXrhh6nc6EoYc4m5HTO2GUop&X-Amz-Signature=e4033bc5ab423ac7edb3003661f8905f692ae82df15997210d057973b286e008&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

