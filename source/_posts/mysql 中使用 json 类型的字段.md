---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7BXXP4%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0xXVN3YYG2ibkJW9bwo3pURfh7zPJmPn1SSI9gFkhiQIhAOpX2PbK%2B0h8%2BfcBEnZAaIVywzyhBMq8K49Ty7jTPu9fKv8DCC0QABoMNjM3NDIzMTgzODA1IgyQmUr7zNKC%2FLp8rqsq3AMKLpvAdlPIVZpiD40jBZg4bXMncpsbjTaaCnPPox7O%2FmzGEruYcGG8y57Xm1W5eeCFlSOhfclfWYsyM6GDqvLO1RkYihKBH7jykVzhcAT1GzOZULttNJxic%2FXu9vWFhWKOtnJTMusR3aRnVID%2FluUuma%2Fo8R4kbfdhOxkfjdpD%2BmwtVpbr3rtyq84FawBCkJkcXrscS07b5SvJ%2BYAfn%2FU3LjjRH8QTXMnz5%2B1ck9oPRpn8TEdWF7rPg96VnT2thQ1n9zxTff3lMr0DIYtsTHJ4KZX9dUydHNG%2BvxRI7z%2FFEo51IVTyUt4hS5YDvUKG6%2BFGn7TjSlB6Oj5jScIP%2FInA3wrJswTP7hVtPJKnkJWl8Ifw%2BTX1U2p0zOmbO553K6gpYmQMWVp49O%2FUoPGs3OJUAjLhGbRUBoB79Nsyt24CyTWIdXvBEdVLmtf5LBHkJBbsNAV%2BAAyxadJ4AOBDH8QImg0evlXSAaJoE5IPnunrlnwgoUhIWUCe%2BrPWB58yzNgth54wkfb11fKJMccnhoduJl2sxLVuhj9CUYKDlhfz6mGh0M7hbMjrV5I3SatKo0uSaQmeSV%2BvYQ3lwb9HsSqS2QL6FJjXV%2FL66RpErXeM45S9YQVbMX6%2FXN8KXDCo7sTGBjqkAWnR5RU5lIfxd8JKCeiBDSO58WitAdV2oZ%2BTfnSGUs5YvNkZeyo%2Bd7XIPLENoUap9W3dTbggST4xhuKRYpAJjBUN8VHB63JOsfUEkIwe4MAdD3QIgYnuiWqJLgp8FteEkFz8PLV4z43FOP7%2FcrvBK1otNdtiw3TEblup4%2FEb0YERKbkY9rpG8i8Luh2pKHQi7CA7JCtsuZmivm%2Fu38wIQ7w1jK4l&X-Amz-Signature=043d8b6b8432404d11178879f45cc3493d2e653d77f13a6937661f9df6bf9b06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

