---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667X36XURC%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDNQNRG5YKOzcc0fR%2BviyjgNeXi4btqudwQWb8nqV6iQIhANXi0X3Dyg%2Bquq2lAomC9LqRvU%2Fl2I0wFqajs9nnkvFWKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyNB2zodrtkxzMGaNQq3AP3p%2FOKxSPb6ah%2BMbqlSbJKPntvTDjXa8vIRBSEWgtAHMmAYPau4pf3GIYzXWXwaKGpI2FRo8WTjjdzwLmK%2Fw66tMQa4G2VAjBC%2BdLQPTafDB3EBw30N6unS98MTpkhdtGPUcJBh3Np7v0hoecKHagJu%2BB10wVHkrmKovn2riE2SuN%2BbysJ67y4ksTdxmJ5p7Vtq7ZTY9NCg4h02SR6R9XD1fuGfBAm1jNAduzmgMuANR33Y%2BsW61Uds9NhBOr5egZml%2BJrArJTly7H%2F6EuIFqCWvNUa48ah%2BMBPQAPH9Mn0UgRIxLCE4Ry0Y%2B6NaBqe7h5ppLJsD5JxgbTqaqPPXsXrEAc6XeXT986u4v93zN%2Bg%2BaE%2FEnMN5GKujRS9S%2BcZkstujTb2GdQGcCY8aJw%2FzMs055rAy%2FbeifPfe%2B1EmEM3GaJ7FVLxmRvUzqkXvXquYxkyxVbaMqCs8cJPl%2FtiHvoVIZW7c2muySUpXFCD23vxTEhT7F%2B%2FRqN9jYt6enqccRy2arHBKmdAT2nTXqxvnXcGQgwL2viTqOGQxwwm2yJcQFzpIQVc8t%2FP3LO56wdh%2FrtjpQsqurs972cNbr6IMYEl%2B0CRgZ8iGu%2F5hw8%2FhVr3keNh29dkEblf6EXijDJwOrIBjqkAYxCrzwNDYklXGVejsF6b1SpRiWn30R4f7uOLushPZGcK2aFa%2BCHKVoo%2BRZJVqQWpLflHvKM7qu6KSx6zUFvo6HAssokpvd16jpqmGKVhLLvybSgD31BlsD1fls7%2FjVqYWmoXoBP2tTP3F6ba0xJu7SM6vzL%2Fy2uPQxXbk%2FGDDSbea%2Fi7Pk9oflXIlQs4E4CQ7vLko1OTOMLIfQ9V4w0TxR5yyEh&X-Amz-Signature=f7dfb5cde68805069422c982e427349ed6bc8a630b81f2f06368c5d9bb8bf8c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

