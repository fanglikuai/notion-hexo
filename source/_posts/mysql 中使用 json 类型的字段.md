---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QASERU2Q%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T060055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAx0gNpaOqTTbtb%2Bev1beewfJp5u3Mhv3uDDifXsrNGeAiAXHP1YeImz8WZG%2FgCCpFP%2F8e8KlZYpi8zq7SLNSQh1yyr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMDtYTI0yGHjmCDgrtKtwDcCWDQEBKRzE43MNpaq4Dr6oAp1RQni5MpTBdHK50UWNJ%2FnB525%2BSzZY%2FOBo2hO7qr32zggThjLbTr2vbFruafbdEg%2B5NnV81Xh83dQutmpDxvv1Ac2KuP8EtCrniw49kJWvsjrTbsiwjR24oRXFP%2Fe2KOhcs%2FJi%2BNB7y1Uzuu6oQdlPTliCP0Eko7G%2BCmFKc%2FqKyWVvoYENI2C1oxoG0Ru0H6LN59T0itOq29Burt4nlbNFzYm2tjHLtU1AuaKtKa8vKPjEEjQTTxYt5yq8G367k7AWyHRbIDssH9Pn4KhXcyKv86DrrCPdxSJhgiB4TYtsaVmhhWraO8G5ekcUf2LCcA4f3pXGmie2D%2FrgdRLIjZsNO3ao43hOkMuouX11SHqY%2BDpei9LqtuaJ27QYxRrBgrYfmodiU6BPdkrWl6Y8IAb2Ct5CaNI%2Ft6oFYgSZT%2FThFCCSf9BH9chErRkVlcPly5dUmEG0TJ3SNen1c%2BlBu1wPZIWmR7YcjY3vKKgIBIJNYgDwz7s4B9zm7d0%2F0XlAc%2F9Zg2HyE5rgfWVaW7Ius6cWzuwiIk0uqfmI8730XtwDUUt5XNs7hpGMYMqKHvSNeHnrUN55CG5lKMsBKiuQu1IaKGCjc0lGTgaIwi4m8xwY6pgE9Ue29pYdhDAyyhGRhDmajntdAw5VGwA1kly9KKmH7JWayzEgmwhq7CJa9SUf4N6gLtA33l4HZF3mWcWx8877U2U3HCU4V573FnCCRd7x2wUXLf5rVH9zIicfzAVS7i9VLdATrwg%2BKU92jO8kczpmb37e%2BWhwuJpkBZHhFJyDwz0OL83XS6rON2GXEDPnNMDeU3LpZSIDDiSuP8QxihteMJLbIMboe&X-Amz-Signature=a23b2be0a236352260f9416df055f38e25c5e50470d0a51170f83a6f3e444091&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

