---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TUQV47VV%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIGEEkFtZv1fmkyev3LWU6V4r%2FdmAabXDRUP8nW3U4nyHAiBDgGO19zwaLOSBCeNSUDHyv%2BuiIqIo5SBqv6Daf1KNEyr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMv2o2HOlh33G0qAXFKtwDK6tiClopbl4cr%2BbnydhcUdQ2C%2F1vr8QFowhCx3xp2QjdVV%2B7RZpH69hBB99l61Mzu42oNWa63W3l%2BgGJ7bByU2SsaN2nehmWpm8bgxACDqPQnAj5fNEtM4alJNjzK9NZNNXNVq%2Fi2Krfah9Q5vsDfX7vOW%2FUFCrPmhMX%2B6nMLQGDUi5fwhhNLQHMavtzBvKPSjf3lViwM9NuUFasckU9dJMlkbX2kDF0LpiVLn%2FHiQ6f6lPVYyGsq4N%2FWYfzZ6T5KpKyUgABnSXXhoWC17HZSkIlIJEPseLLPro6eZtnjrpZZuDuNsQpFdY1r4plxaYs7Iyw%2BewAQSeup3GppKI6%2FInFJJiUgD67%2FHp%2FnJD4MSLB733YHgVVhcfuWE35utIvboJyzMhKC9HM2tMDm2u4aaC9zUlidsjVrhZR4InaMwCM0niNj51T7%2BRmJ1mKhvzlk6fAWu%2FcJVu%2BeWcQjULffSaQkTkpjRJW7%2F60W11SUibfdL%2Fe5j%2BuYVMq7BkQMhsikr3OFuKmZckLm8gh3aeD96H9EcndiD1WtvMwq6NyCqJ1bXiuY5BMHiBVBg%2BNom4QCwuLt6n4rSE0JXdfT3hhp5B2PoIYx1dkFZjvacdZj5%2Fh7zT8JCW4phW0OEgw%2BtSbyAY6pgE2RSahduQo%2BvSKCZSeewPiikaq7oEX0hoCixDjGrD4JBKPwXiGoyhI1RGAIAGAp6jTiuHm2jWV2Rpa93i2%2BDYpq38K%2FQO3kwgplqqM3zALl%2B7GKXPIixUZMdyxDJ9EjGInWW3zYtAS42f9JxnwelY3BtukLzmTRocrhlnwJ3PEUkt9EBgMBTH36bMdLzWLccL0w7m4I6NoYSiBOEAfvfc%2Fob%2FjhT2s&X-Amz-Signature=5dd9cb032803ed36bcfac97385343b5a438b0e61829561579f07427fc174720b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

