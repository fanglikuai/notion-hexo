---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJY7ADQG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAExFR96dtswfURqJJEjGmWs69GK5JZzeRAZPI94z%2FryAiEAgsrklYaEfqTUnc92HdRxfbuap07Uv8wzUgkbP3KmblgqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOvVXUR%2FuIrNg7CAwSrcA1wleMv6YcK8hGHZUqy6a26NsiZGI7Phs3aGFDtPmdeJMN8bFp1i4LQymUF1eK7NzD8jiRffWbR99vwStsQktCPd%2F9fZmQSHMkRRXGEdLKHKsoqGIH4tJe240TT%2FwxlgnP%2BM8Jzb5x8rhTXENZLOnCnsv15a2do73wzMqLls9TnhQ8rMCpmG%2BQAA%2FV1J4e3jun0mEF9ro7LIZelDhkvHDGAeJvx8%2Bmrhf2dD8XkEtR2ciUiRr6n%2BgQpofrG%2FAQNupAzK8EwECdWOfu0%2BZWskSONcmi1%2Ft9eAKquO4rgrLIgjeGzyZEoL4gsJzhw6SDJMi0Lty2v5%2BifyaeaQqbsUw293ezTx%2BHlgV1xSrH5Sqpyq2VbA65DnThw%2FnPpimtC5MIkFUc76NOTg352gcb2NQttHpz5nmSWkex%2B%2BICHOOq0GkgYJGKYvxLDpUowUgKOFta%2FJxqL28nyJZluzPDRaWe3RWc2t%2FvIs79PQbbQmiEEBGImGn5kwV8%2Bc6TyP7TVEufEVImHE1tOMnpZg7nsTKQYXgcoVOsWh%2FULMoS233674YZTA0iBzHM31w83woXwCs1qej5RAp8Bb9xH1310dvbuUrGC0yZotGCRxMWiTT8EPCnDaY3scN2EmE69UMNDX%2BMcGOqUBOdVjnGL%2Frm0JA5o%2BbRKQ6wyy9vC8Usc4Vh%2FZXiQG4C9xPq7nbjbCTBBpmJZsLxmLenH8d6w2sUFLle%2BI%2FuJdmrhTknDYa7exwSfE8rBvaDY6cbggepZe1K0PqpdB3nrJnyt8R%2BfD0cCqLtLA4eveHHn8007p3YW1pSQS3NxoBaQYrd%2B1nHrTDKkACpKF3P7nMO%2FbOLj6eAw3Y5vZztqYEO3Bmwt0&X-Amz-Signature=7491e02d9b6045cc6a40efabee40f691d3a02b9010c3b7a04963e892f92868cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

