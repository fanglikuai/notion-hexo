---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WZS45AD%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJGMEQCIFPP%2F9GNrzHd0VwtLFC0%2FnRuukCzzYXtkkq4R5ulPY6qAiALRGLq8dRT1jnL99wBrD342nlCQnAz1UD5aOTfnW%2B%2BcCqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMd%2F%2FcPwTLefgEZVtdKtwDevb%2Fw7WJYDzqqX3l3hSl5tXJpQHsRMJatu5BnDN9WPubx7UBcV4IH42S%2BsYaEfQCYeieujNjpqoxBksjLhd%2BM6UHCv7SN9pxnvN4D%2Fca%2F2TSgR0a6oPD8MeOzepFclsy6Gii%2FyRhQ1D8VU0BImdX8fwXtvwihrGSRJDy3NfTjezGHYuE%2Fj3%2FMHAWut%2F755DXhzq5ybfrtG3QcAtA11a1JQAwyO70n2X4L5pNHbv018nXDU9XP9Cf4bCFIEXPylf7VOi2chgA%2F8qRob%2FKzuGc3tJMarMeIviaoVUXUqf0H7whH3etOCgjjKM1RCkVgv3suJoY1Pvuq7Zu9trSfiWKXGyEzN0O%2FOgVicM%2FPnvhJimBCqMLEIqzm4BR%2B0Xxxouy6X5Rg%2BmllUhHFOlUltkVopGqBBQ7NW0T%2Ft%2BG0j5E1q5mr44oa9NNhIPx8KBBE3dNd6Wgu%2BN5O1qb62aioZ0SWDnYbPFaG%2FTBUV%2FJds%2Fw53TktzznwuG0V3dUfdLTiOUa1ZkI1hpYUbmbq4EMPD9Lbz%2BuNJ29JPRgH%2FdxjtnnBRCk5Pf%2BgkGZBGaRexixpxg7kP5loMEEYv6s5gLpyqZU3J%2FiOTyoCJVyvoaKRYS1b4mlQ4RqCafMjJL4kEMw2o%2FdxgY6pgHrIycb7nokf9eoAG9vqWPV5uCsgtf76bVVftwjxa8IW8IOrZzCDCL1zlObXJOFTSZVyasErmipk9WunAw3Vkeu1X4axUgylT7Bu7OKijZhd6TnTDbLiaddoXNfE3NuAho31eQUfq2JWq3ctF3m4cgcN208OR01SV5zmFQGsSx8PYBcSfk3p6o4%2BLerGBNu3Tz4pGFqhoRt6%2Bsf2x%2FLsV1SDuzcWDt%2B&X-Amz-Signature=13b6f4cb85ef4f7f3b6378d34b751acb61530b20ab565ff4eca6b20f4f27a6b9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

