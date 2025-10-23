---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B7JXDTF%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3%2FgEE%2Bvbqrm9b47DblIxD6n0Wd3BqTtqAgnxgJ%2BSnqgIgQ6LCxdR3Vi5eZyCMTzn7qnFbT9v4yrLGvFJ8O9Y%2FpQ4q%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDJQAYtk51HuRQisDGSrcA5Z0jHORu1%2BZO4s3EMdObFEapr0UwACyvfhgC2wiiVFnv5K2ciUisodf%2FkABJ8qEW2Big6wT1vAcHvI8LMMKCG8dfMtpO1qvHQlrFFjjPkbaQ9GpZEm7B3MuqxPmbRLiimzSVwTODPKjuORPwN%2BkbrC4RcJV44FaN10UecL%2FA32fEEM9NMMwx3jztMmZyj5HO3ZNl%2FWfIfdkyDdUIROU8P%2FgkXJrozK%2FVRF1UtMRjolFUv%2F%2FYHSCUC7szA1ErKdlIQiVE%2Bd8PlpRc0IOPqgV030md%2BpR5y0s18yFr9BorVKIo59VHwt%2BsxitQK7DNXM9CmPYisivzXNrimXa3WHpbg5z7SN1ZQoyxYMmfZaONbQ3nmvgM0Vb%2BQyMkzBWmano4pGAd1Jkqel5uPgvDYlS6JaOz7T40BHoKS2yEcucevdLST%2BS9RWKbDF%2FWhedMvrNy9OGgEW2TowXK9qjYB7y559DbkFxIBBkfvYtl2fehz8YI%2Fm2TUxe78%2FBuXhznlJ%2BwBU0xOMvJZPjCr6lr90kdE26XUmFgV%2B%2Be%2BfCu5t3jPbY%2Bal3k0rAyKxJZGjSke5Kq9LbSO4BGkx7vRIJB7TFqooHfqFn68Du1QGYOWONfPyzciJZcEmVGaexDbZBMMb258cGOqUB8XCzDnqRTKyD5Yn%2F0glDuLW4SQlQhR1Qz4cFP5pVhl5uBagUrvtSCZBiCJMppUBxHxeAwCSz%2BZPr91FGPt%2Fv6H6gJemP1lhAI1brb%2B3xJBiGh%2FCYRjLFipO4Jr3MMU%2FGtL2wNvqoWiNbVn9L25PTfmgCGvAL%2BE%2FhaIvIcMsFLCf0TSYN3U4tpTPybh4w9kprLySjTNx1YikxnXLnWJyEZMVxIf6N&X-Amz-Signature=403a22ecdec9d993b3f2bb532b0521e1cb20ace300c1587725ff341e634c1bc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

