---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667KBIJHCN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T130038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDsN2%2BBbxvsNyw1WJ7fec2E%2FYBpAr06Ozi0mroUz0DOjAIhAP7Cj%2B0WsK0glttbyoyffetJKbEqfdIZAerBYUNIHXzYKv8DCF0QABoMNjM3NDIzMTgzODA1IgwyxCuEUjEtt4cTplgq3APm9tr4sGSLz2Nand9PSg%2FxUCr9okr8T1mHXgMd2Fc63xDtpKaXpyaIZoitugwHyGOvdWiApduYwfVpYU9ZEP2TnWzsR90WFPaZRxVGNngmrntRni5rhWEKP0%2Bh%2FI5vlcn88fTnanrEuCK44EQLMupdOPRJVPlgiGQ%2Bv60GZSkv%2FfbI3CLWrTmZNS%2BsilOXZ2ZPP7qnZ3fHNo1Ohpl4g%2F%2FkxpmlW%2BPgDwggrclOibuQfnwvGx5xmiay9lm1e3GaYDTW%2BgiuvIDTisAvcjEBv0bOM3bneAwq8bbqPit1nB%2F5%2BAAEiCXt%2F%2F6aLNaR7ys2yd2lGVWQ0fLWgtKKI1SV7YlnDsAi1RCN%2BiCy7QU43kAuBDhZ0yMTUmyDaU8cJqUJLeILFkxLwY9XASwgQk42SnYVc31CmegBMyeM%2BbCztyzJtjbeAmbyKkZoHC0sVQitUodoLOEUqWcLn%2FPb2n2sjjbBBQ8etvTfoQ8H3oLZCDH9VRxlJ9cc53j6yE4YLrETaBdQV78x4EzD8Qa115f3z%2BpP9FaOeZMqzw4tZ2EfuqfcchRhRIaGfqBgHQkG8wMvvwIs76ajXxqZC6Jmlmwot%2BryojkvX%2FzTfPcCsF7dxrUQm97lh0yopMxOGqpIgzDm87jHBjqkAQVqFFkut60GBlFCnE5ORGe%2FT8a12mc8vpcg%2Bqi6qiuT66IMu8xeUE6qvnb%2Bbz6AcS1VNHEAFbkYpmgSZDUHLAwBEY1%2BV6A9bombFpmiN3ZVLEAGXM176l98VFEgWBpUg1APH04qoSayDI1mbhW6jNZy%2Fk8UgVTVfz0UuEhmWQwYPUSuptKBN6r6xOBRisWkgSD7aVtPDBBYpKSmHYkP7BXnvryH&X-Amz-Signature=f4c1ca14367527e74344d5eabc5e6170eb2aec224030a607f9dab0301e665e95&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

