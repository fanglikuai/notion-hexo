---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRYLQQDZ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T100055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQC4kdRGJTO0rWPP4O2KtukQRphmFMgEo0zBjCkJWOC9zQIgNjFwzbt7u5vZKNZZVVmUjTEf9dtf6YSSxZm%2FSy%2BfAaEqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLc5cN2iVUAtddmqUCrcA09bSyFqf0f6jxOuOq%2FDUp1vxlxAvGuJDP9Iur757eohKQafH6tYd9kp0vGxyiCcPuRuihixzVk2c7GBM9wrASzNqyZaDpYRyEl1j3DxfiUc%2FJ93THnt2YSdcnicZJC2gCVqySQPbHswmvVGXlMR7zM5b5Xl874MkEfRPbyJOmf07BcmuhYO3ZdbZ5E98cbL%2BE%2FDPgeepmxoS%2Flz6e1PZhKyXtXcXpEqIi02wKUN9yslfBwaS8NIpZwbztEdS7So1VvTBpETUgfdiUwhr%2FlCGN2rx7MDdjXdK7MScMjNyJkraP5%2BdBkx%2FHaeIrQfni0RFwZHQIua%2FH4kln61VbXAKRvjH2pGfXaFZuT7tLWxx4EdiSGS8H33oHlEDug28aqPghFqwtkbaK3ZEv2HAU7bfDhuUjkkPIZPupurx%2Bui6MLILXRuF%2FwaFXlG4SmMlDwOa9tYNYEQfbIFnEVwv6WQFF7zJcYfv3%2BNg6eZWX%2Fw5fBYLulF2FZhfC1GH5%2BoulqrpnFn31NQS%2FlNaQJ1YEUb5x47S3cdxIynYHg3MQ3HtqFdPDTWaAiiQtWbxok%2FZsfrM1fUkZYTrKtMA7AMmzlMViBxei2CaY%2F%2FlJ8hungKGpM%2BcVOjLe2aLQxRcIScMLfi3sYGOqUBGHLWMgC9b6McP%2BH2Iep6R%2Fa5g2GF0zyCArEU7uIOBeq%2B2mMjcAUT%2Fo3ZK83B6GVBngsmo7EfcJe3iYczDuBchZ7LaUzI1Oe1kQujXfWHjU%2BCenUc9v44MEHfnRS1jfJ24ICqAFNvKjBo3tOE8RJJdPaKcn214xhoMt4RxqfyRu6VajhVEVkjvYoL8shBG70OH5SoCkv7IbpSrNi4hhrN%2FTCZmYF6&X-Amz-Signature=21cc99a2c7a1bf6d05dd16a063107f5f134df2c0b436389d992d190429ce7033&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

