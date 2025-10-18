---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EA6WK4M%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T170051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDI9akIpdC%2B7%2FUmqehFQmBpVFMWJPjjPHbDsvtHxxMcwQIhANoGEBtgBH%2FtHl3qIb3S%2B60wGc7OX20Daf1Etdg6e8HWKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxJBFswWVvi0qbqNeEq3AOZthziLuN%2FRw1m06MYb8dmHcWkJL8c5PjRFujXWMAt82b4Bc9fCDVXvpAiaJfXgw3EBDNa8WwoU2zgLCMdUlY97ko%2FYm6%2BPPG1j19QTmFY%2BNzOZsXkKwLFEw5zNNyNkVtHIqQ49Z6lGY9s6xgFTB%2BDRpSiJg6%2BUa8x1mbKqkVTdcS8xhsnneZ7VFW7Sx%2Btrh%2F4dRHMgTXkEyYSaii7u7sQveUSbr1AWbaOwz%2Fvli80akSSPOwXLIZftsWfSZPCkTtZXVgZYYv86Zh01hkZuhbhPamMw5BkWOMLqUSdpbZGlqj63sEyUkuS4v7lx23ToaqoznpeQh%2BFJbBXs8WjYc%2FRC3ouP8Yk2MjlsPASakc1I3paGrM7%2FSdI3RHvhxSBhfPSu9t%2Fyv8bvuSjt77dNgOIuxQNWTFRGkj3SEdf1dQz3NDWNnv2GXcrhRuwRyL2wFSMaELRgAcsoz2DK%2BfXGlXZ3gfod59KheUAvXVs721lLfJ2d93Esp3dQkzWlqEYNqOlbBAnCjnzFE7kcEwlY5iun8vittTCQ2wrEtvYTMxP9plyjupVWDFBdf%2BTL%2FyIzZ0b8YaERYu5NSVoomCq4zc3MWnZx5SQYmkuwPFA2PAFnuzh49Hu%2FLEzAp2hRjCEg87HBjqkAZzEdsjza6gXvryLAA%2BnyuhwDievDPfmI8Ik9sdRRbTaQvEF3hxR4SePmXjPsMBk0%2FDLnQ56TYtSQW0D%2BOAnlGzgQHI0mPGAA%2BczI9Oo4jidtu97WjuHX73WN42sw5WH5%2BZaCIURj6jqt1h%2FLI3CISyVHvlELupKzvsbRCX9xbHYBDg5Uq5qroUjTk3ailORB5Gcxmzfd54%2FWZtrRnbOsPEnSXJJ&X-Amz-Signature=36f8edcb54ce4a2740ed4fcdb1b187c8da725f5fbd948a4072f68c6ce665afb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

