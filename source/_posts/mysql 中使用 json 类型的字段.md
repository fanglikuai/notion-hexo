---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVNH72DJ%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T160101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCmLGrjLoCb5PAdIyTvRlUwI0qFn7LgFEDL%2BcAjyVQ4mAIhAPZeXVF9nO52bpdvtkDW7z%2BYKVWBTvzomajdDMM8YlyUKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwecammCscfy6VKCqEq3APBgeyv6V5gBjmq1C8OI81uuxtplUzGUakl7gX4D93OwRrqpr8N0oiRPAhQO%2BkVWGgK8EV%2BLtCWyNdU90Zs1hnEN5ltJpiMAMD7n8h9Xlwbq2lB2JsbW5dqw%2B0DS7NTrgAMoE8N150Cw%2FjepC7wnBNdHv0cr4L24hrRTlC9gVnzQCnwfLNnEt24hFzWk2AM9RYF2NfeBib9FrAqJfowuY6Op%2BiooU5lfY5T0KDnKpnLZ5g5Tf5O%2Bk3zpFgkKGzRn79Q2W1z3h%2F1bzmJVTIkVOsGSTkYEfCPZRk6JsUZ9giTzzGGFndbFy8SSG%2BgzDIwHMSbWwt2p06lE94RpQFph10RVj47n%2F9ZZAOTGr7EgcnQSWWcnauNkp41eLIFAZ%2B8c4%2FsXOA%2BYtJ6h0ujuWzoRPPVu3GFWtcK%2FscqVirT87qbWbpjc6V6T900i9Stb13TtRICx%2B4Srac5JsjNGXly2klFjetwAH3KPtjX6VP7tmILzwRTMZ26VDriRxO2M%2F1%2FbrImXk9SQiqyd2BAkQGEOkFZw%2Fe6hNSaT8H4Phgcgaft5WxRPa5QItamur%2FBmLlFSmES3qcNA%2BzuPT6%2ByuIdHaW9%2Bl3JMAIugkwyvT8zsPE4%2Bqg9oVw4G3HK0rA1GDC63ufIBjqkAfz%2BimWGB4AKyaOvlrWnbDp7Jl%2BQCIKwXEMEHsLIN3wKcas%2BrkxkUMYpSzO9D%2BngR%2FzVpiWe%2Bz8Mk%2FfkODDLZTQ%2F%2FXEQhZHP3VIZoSCBru34sXfE4zFM%2BoOMmS%2Fl7eHM6B%2FVJaKk0u2C%2BEpT25aoGK1H8X%2FaNiOWlQeU9F3G9z6LX5SPFkuACAArqm01UiUHJgwf%2FG6rHIdtiTI7KnrQElT7eFdG&X-Amz-Signature=8cf282f0676e856f9b89acf4d32c29806e724b62976f568fb5323acd343e6797&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

