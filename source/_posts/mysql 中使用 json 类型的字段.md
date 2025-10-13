---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSAJYO3Y%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDndZ4gttarasSnwswoQUVuh48eyykLynN45GYRTgSEsAIhAKWw1sXPFh2ErtHKQvrsDCoU2O1%2FGVRaPcSQDF4R5eN1Kv8DCEsQABoMNjM3NDIzMTgzODA1IgzAN%2FToGuWQSK2%2FZ20q3AN5YHCO%2BQY%2BouMdWmRDnoTBZa6RSVPAZJd5BTdD%2FqB%2B5DT%2FAlr0HcYGD%2Bmn2GevHKSZ09jVHf6Dz1RArXBZHugOZxmEBYv5PIjOxvK1zjKJfYBkqifmJH2k7sDj0IGLD0q9YBSK5tMA9yH67Wbfi4riF6PLn5HFXQqcKN4ARqga2FdFdNv3daSfZaY%2B019V%2FmMpr%2FzoxZofP62H5pP%2BQOi%2Ft4oiR%2B7u2GTyuoKdgXj8UL9%2FfxyupDJbOXxcKa4bREjeN%2BK7SkUn2AL%2FKAYjC9kVk5xAr3R97YmxtU04w1TTeZ9bEoTMP01C7ke%2Fri7r4LHek%2FtXzt01WOXIKRJvFpsd8mL%2Bqr5Nnfdh0rN1RxKUAcNbs2mmbLu4b%2FVFxNDAlyWJEPevdnJXKZCJYc5GVvKB8n4E831k0oaKCyAmJCEB2Lg9f0BZ3VwBakBWEK4t6npWudKF19j%2FVyAjKUp5wBm6UgD2L1mX7GknQuX0kdlhvf5d8OGx6F9kIPXwqHQzS5Mc4%2BWSk9S1RK%2FrpW95nPguxUYj4JBoG5KNblE%2FhYldK78f2nsFqDuUYRtQQbxBCrs7keLuECacqpYnuUDtZ09D7b%2F6PtWyIHPicpA%2BTX26d9Apk7pbo3aroCgCzjD58bTHBjqkATswyvbf6GZS%2FlzPhLRyRwxs7gHvA6fF%2FwSLHGgIGZOG0BXSugOXUxYAOJvGKQ5UN918BgRavNgyKCPatE8HDu5O91JZO3q4yOA%2FVG5bldyQ6zGtCGuSX%2F63Vrew6R2ErxuNUvovgRwc%2BN0edtp8tOp4VObl%2Bo%2BTb1hf9zd6wMzVhihVnX4MiMTk8cBLNIp3AJTtjJQO%2BhwXEkWs7mbt%2FPN33YZG&X-Amz-Signature=e1d0f02ff01ed1700a6301525a4dcb3eaf221723c7da0a3278bc132d3225229a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

