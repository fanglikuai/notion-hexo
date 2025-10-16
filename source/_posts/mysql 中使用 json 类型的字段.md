---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666AJFUAD4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBpn8sVHqRtkyi27VehIeVyNsqon8%2BRF%2FNJuCSwDGA6vAiEAtG9E27BqdMBf28KiV6yy2NyjYuppEpJWSQ8vqIkuTYcqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJo2%2BmDLuxrezUwP%2ByrcAztLsTDOf%2Blt052FPOIoB4DqdnMfaxZ1nKYKBLIyy6G1LSrolojfe6prIrmNASDOQCuA0r1%2FKdOog2ZcDqw5YI8ZsCOb1gsWyEy0xAS4TnVHZ9Gabk%2FVrVfaP4laot9AISfEqhOm1Bkvocm%2B98ks6aBPaIK4HFEsCuREMdNIFyEA9b2iaHXMS%2BlBVyOBcw5wJn%2FSHBbW6w0XMUIgGh5ah4hqsjF71lqF3QR%2FbP%2FvmAayafIKbtxP1TF1GmHDu0yUwpCiIupPzxaiZZH2moJKdZrZoVlYTX8JiGBIAJP3cFfaT26NZvNgYRI4Oj%2FM2NiL4hvLwtBTbHPMM%2F2giD3g3g0AmSY4tJUk1A1JHf%2BlkcRo1I8Wif%2BrTgIoFjdEyUhir2thIzU4Y%2FgCZ%2Bl%2Fq8gGwPMz9yzT24cGE4k3lQvSY%2BVxkPcfqoIDrBF0aDpOxiGQOIA9Qzcwl4E6hZyvRxeairii%2FPEFHt2GhKQiIAnfTvmezs%2FoxAKJpp2nDPH4SAXg1zd%2BYF7e0JT8CZY31eLRJWKEa%2BfMAlBQRWoxJetiiUhn8b5BJ1B0tnrh38JgtcIZmfbDNuGZKlkMRGcX5QJew5Wcy96RFttHcVAzy5uqJX%2F5panaoBH05grhIOHTMI7awscGOqUB45I6ZTsH%2Fi4r359IgpTu8INNTbfCk9mtoIzYkHFH%2BuGmOVtgyLz5StgSyJtYgDyNUOdu659qV3l3yENjc%2FcbB3xCh7pioIkHOsU9F6VpbVtrQQYe6394z3E7cg91VMG5%2FOuUDwCURoMGPiETrrqjfc55MuwVR4KxoMBeDvo2xqOGlcg2i223Z54neC18JPJCWy4vz2AaouCpz%2B5AGV5TVhijPyoj&X-Amz-Signature=0705e1f0fb5d54da53584e5a3245b734df0f82cd2477ef347a213689b0e30800&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

