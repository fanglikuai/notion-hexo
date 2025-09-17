---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKYLH5U7%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T130103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIQDKtIE2nDcuxdOI6ogLCAOsBTndUR0dSXO5hWmQNAx8vAIgfqbXYK59RZKcY0UOS%2BnyE267XvgXs5TkQu9xWdOClgwqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJW%2B3XwiZOsK70CKOSrcA8H8VD1VibYHSIh419U3IYpq27HfZSPuiqMTnpokWdPTB4yHlXCwsATR8LqvyBu7nk%2BKnFAksfHd3iTdxXO2Igv88zPF7kss97IL47MiJMpIcdwV4j1080DO4r%2BC5dCoy%2BLTlyviRgjJ5%2FolSatUq68tbGxzDixH3FyvDb7Kagvcgw28Unse%2B6aQ4rDZoT%2BxYFWjHcIEQyoAkXORq%2Be7xBk6lrbZMJa1p3seXuIRWBfOr%2BxDrXKxQimhlWG7lmsBH2OtsnK37192r2dNaQ%2Fzevvp9yH1CAfbotg6dt1yibcNXOBk9f9uhPjig9qgiDmfmMscIcRkRxiOWxXug6XBumm052F%2FzMQKZGYXVowJLpaKhj48PIbvNckganbY9y1NT4EBaXgEmxeGguZRL96gw%2BTnjL9s8v6HSJUbJssNzGjE%2FAyPmyuv4yN6MD6Qvgw0TEQ6SVgowC8eZ1VHZklMdVKwElcsnG%2F%2FhPz9JApI8rUbg644gGrgsJKwrTYw9pK0c%2Fjc0v%2Bxc3HXR9crSIgtBIy4MFe5X43zNT9UByyMHnqtbRVHqRmxFsBxUiVINV%2BFzVv2Zq%2FrkrgW8nKVlFpVFnPcyHficgDb4JxOdfPzWpH14pxVjCQIwO2Hur1UMJTVqsYGOqUB0KENV4u7fW3Bswh323xaHte4cTq%2FRJDz19TGHzLt3bN3JpjV2UN1uKYuPNfFUOm60vCq%2F%2Bw777MrUnK%2BqDYjP6v1Ti1Pi%2BU7pWa8P2s44QRRzzyIieeDYEKOVfb8RaWvEb6aQzTX0lTtIdL6THlAQPBCUPnSFTPmjBLWo3LrfpofzGaIFtqCN6p27O2yfgyTd%2BxP4s0J3ui%2BywdZGgqyXbSMG8xJ&X-Amz-Signature=77ce1089e05c276dc2c4ad4cecb8e8d0ea059b98c0263c57afebf7e23fff4f2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

