---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVSZWIW7%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T220043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID78FlPBL%2BCNZT2Cq2b24csmt%2FL1CHhm4TA%2BWFVhsAyfAiEA2caWhvlKyg4HcIYMR4mqFw%2BrCNj1flSfJFwzxigdZCsqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCcI7WENX4ihWw%2F8RircA5GfIjszmnYZmLR44nP1SRq5jcmQlz62j4uT60%2BYbXPTFKj6NBROKgxV22JS2snpNR7Fa9yiuB5VVsCibir83251K6GUVh%2Br%2ByMtaXLYKwSF1XE6%2BENKDSYB%2BSglPOc4uenpNogUjFpXDNYM3mTnh5eDvmYeeV6CKoIOY6go8XwohqHmnmYO3cgU4XnC6OrYyF%2B2yIsDs%2Bc3HXB%2BZVfsT0KsrtbRA%2B%2F59cDq%2FXBona%2FHvt8qVzaNLiGvQVawpR9bkNPrtr744CONjG%2BGWobiYtyAm8Ax0e%2Flyh36IwpTuKxCtaogcohmiF%2BO1x%2Bqx%2FAeT2Q8GDnHTcpexpJLceeAjgoxHCfTCB7V992Etnh1yyjMdyiXtzGX7S%2FGlsrxQY5Uzt4FzeLSwuYtxjtwqOgAVFnrnwTRbEo09EADjr2nSC8w5jLMkVgvwna31HAiifZZO04PMvBUYW%2BRsdbUYQvq84LN2XLKIqmlu8IQM42jgXO8QZoQJ9OzFym0d7vOYjyKgeMASf3xhp5SplcIf5ZhdFLzb6LtPZVRPDPjvnmrqx8DMHuipOase9GQ2JBQPJObqER5JieomXTU8pSPgQB%2FVjd5ISw0Dnzyi7jG%2Bdpg7X9TThWJ9zlDSMsTDXU6MLW9oskGOqUBna7RwoaBjqsWbYdWyPWX5MF4PmnFNIP65RnQoFkFujzDTH2X2aqyozVqz%2Bknzh7CgzxXx4uqZKlrlMWNYuJsP86jOUjBiD4Mb4ogUf06z5E4ZPVMaQuUVBJeFZ5qt58VMcL4hjlQYjGgUi1BNHb5t%2BPYsK1uU8UGMY0MbD5mkYPkGEyk1x%2BDOLZJfx0jmk88xc716pgTMoku66Pc6c0y4R65ADnx&X-Amz-Signature=88f659b1a72cb6b0c65b86ac8b4c147b93813bccdbb3c6a99aa705904dfb5e50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

