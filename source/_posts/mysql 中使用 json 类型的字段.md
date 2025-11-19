---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THTAUXPA%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T070053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJGMEQCIGugFLps13TF8uUh6i%2FeTThRmzoWQ2SH1KRQ3llaVNUAAiB4qDVZIOTlVi53POHalll8aAEk95vtT%2B9d70kighs6liqIBAjY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaZbznCs%2F8D%2F8E%2FS5KtwDPO8A6%2F0mUx9dv4VYt7giWmpojGWqucmtWY7MnpPnRvi%2BTh4smQCZnTG5wW0OZnuj5nJRq7f9Syf5csjYFIfxdVVP3Ofp75tL6w1ti%2BP6q98lHxzbr%2F57qb2ZLmRFyodIPa0RvN5U1WKTAcHKiulPSnGSRf85nZpgnRoIBKnDTmjmbEP5ojCbPA8Bs3iWHLCISBrCkMI%2FDwt%2BhK9dTbKCzoRyxcZSdTs3VEa6hEXIYhNmUNgg1wL7eXWIofbFRD0%2FJbDyMfy2e4xX0T1jPurisY%2BjOjDEW4AfQ2wb2u%2BjdKdG5qPiSOs%2FAxdZUwR0x6XgdyrU6qlu6GaOGgrNEgi6ie8tn7E%2F4QxgsqJ8VV3KCjOzzEPdCTnRo7wdLp8OcewFesi6DABQ0UWwdQPpDslLQX369qoLML23mw90neLoAJIoM0igRsoDQ3zdnyHbIBw1KE8z9wqbFNew9QdFgkJXv7pPHG107n3HdVRHwmrck9%2BPu7lK07bz6Spjfo0f%2B2VW5pAXPMAZN%2B08y%2FskCRc2q43FYNdhAwTNuCVjUlxeJag5%2FL6CLRbbq0%2BdjuEUc6jpoPvMny0x6bMgWxWTmOE5rIl%2BmKkTUUXHmiNzYboTurlkmViu4bJj10jeVP0w%2FNL1yAY6pgEsSQLn8NFaudWgI3HLBtO%2B%2Ft5lizckd%2FQElGW3a6lIBxli57xqMpg5v2k4YWdhUQROHiZkV4TKKldBN%2FXz2g%2FEM6jH4pL6tPw6svGEvPPuJcQGhLdyV2Gz9YmXDV%2BYp58CI5BdVKoawAGwJjd6Z3F1QCEpbCximG9XANzugiF7kSLWhUDnHe2JgI%2BQ2AJ6dBgv1uQqULOadWlIQ5uZBx6YC%2Bt6aORo&X-Amz-Signature=1a1c14a32bb8c9036721235716ec28df3967d34626ffcaf4133827ea7d398078&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

