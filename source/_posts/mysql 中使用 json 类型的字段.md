---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XYPJIGRF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQCpXhTE0GVr8sL%2BIwodqPHlIGzHF10zTEmjRQxWA6R7ZQIgJAaUi90OtmbqTVii%2BRNoIECMHO%2FlYCrCXivqYwZNk5gq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDAffxJC5O0TlHfV3QCrcAwP2lJfNyBJ11G44Prpe0x6shbuy%2F2gn9wAcoEQIj8L55nMu26WcSqi9MeaxW328TdwgcQfNbEZLqUC1sDXrdDk0nHBYO7vs8kEQlNw2q8%2BWkYFriQ2OBJ5kInoh5Pmz5EOBGRTLtmXSEWNbdOcbCzz3occXODlgaekjZvSoDxRDFOHWP2A0gCw%2BlakDL62Vq5VgmgD9M5Z8Dqci7UIGBCGYYy1oknWSh8Av%2F8QXH2TLXjAqCXPQhw8548EAYt4Cxs1%2BqlfuKw5kyYHJxV71VM5GXYB1VEs3GatCPjQG4P63O%2BuTAgQhQNdq3gIiy8msPN8psSSOUzIZlnh%2Fh6WiBKTwNIuEMN35GOq%2FT4YXqSm4znSznYFnjrOJwpZR8bXfHavbPrOrOQVhziUqR0gEYiMvFXN5ijQeZnehIe4waHzfR1iXtfYGJt66YnOqarfifKDsMp9YAte9Dy2cnp8bnO85s6EdGmygtEvHep%2BkM7nnIAfFoVMogOD844xwZOJmAIbeS5nTqMgNNLB21t3RLhghpxoGEo3OQiwYXvxECYpOFe142WpNLq2Ql3rHT6McaOyMR6FFxb1B8m4WeGJ0Ppr3M6zFS7ZMmDxSthafzEy7ySOQbSoim6tWMZRLMNL4mMgGOqUBZZCn9YUhx%2B6k56ox750bRMIdQwP%2BpkENjEhUtW%2FCVB6ZVghXPzR%2FZLDNhPxeuaTDT%2FH3sORImhozHWQSRFcY7%2FCmMqTZ2KjLdsdSVmAcfs5eqTrDMLxIScXFR5cAZwxmeArwar9or2IPivt8uj8Xbo%2Fjmi0IqqoxzVivfQxXixIuXgY1Ap%2BhVlpJaeaeqehkA7Rfgy%2FSveZMRls%2FuJi48AuIpEBE&X-Amz-Signature=9369c46fda73b57ec7242302c341daf1a08d1abaef8737c7eb07770eb318d09f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

