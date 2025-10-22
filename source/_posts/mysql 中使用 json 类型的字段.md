---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHB6W5ZW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCIHLtbS0cVWiUzgUjXMvcVEGVOffvy%2FeUK%2BuuwiNbiT5zAiB4eR6P5UWtXbyKcbNfucGZBq%2FabeUm%2FH1%2BDReEQ2njZSr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIM1c8k27%2BCOShOITY1KtwDkf8U8XxVkiarqRWyyhdRsYF8OnXqFFQsZ0raKEmRZGI6z2b6bDVoqOf57ntT2nwHiqj72vq%2BxtQeu5rEJLOsQiTTMe%2Bh7zoZR2fNBYkc4ERijgsiBX4nXHny2cEtJFYlb8jRANYQq8nqvIEO6v8Is4fItbmxtmS51lHSic0yfvA1Rkw1pUo52A2AY0ZIwjOf%2F%2Fx9CANCWgeIIFVOgCsSFxVm1KvujHyKG3boacsrZOLAkjuY8g1SzZAqm0boCjO%2BtyLaTUWW7OkxYdHGUJshjnhqzt1Z0IcUgw8RNyGRG0kQpBRxF7PgEddzn5NWBXAiHS65CFJmpF7frELkYOtQ%2BrfKZiSm6GuAFcPsEe5liUp%2F4MZWGlvzzfaZQ%2FbIy88HP8L9TtLgUMkryHWqhY3HkuzU76SQY087qj4136y31dh7r4Xo6q2bS03YvDocC4neZGOqbUSxd8hkblRcpdYoxDDFX%2FC%2FfJdDJVIFsu5%2F%2Bhjye1yCgDajNHFefAhTSC4U6hmw4se%2F93ajhRaDqZHFo8rUTAR8sNj8ErwG9FbKAkSrZmttaK3AyXAsY7RAcHftD6BnIhrV%2BsELkCuZpSrzrXBEIDq5EtLuYbwukqggyvqVbgBd2nOQixKI23ww2engxwY6pgGzJCESSqXZxMnlzA7LLeJaGzouUPIwdNGBn24FBPgVHE5ylz0WlibmKoh%2BwoleGGuK%2FOkaeNeOqtbkNPNjFgmOyqwrYwBCJK%2BpPdoxf0u7KFptDBF90eQCCIgqbUTkDrGpfErvvI2m%2FPi6QUftcRZn7HafM%2FEAWqC1o%2BdiZVbQsSeHvMXln0QS3jGjZI5SrK0iUAYS%2BXc9Xbg%2BWdgEDcNTuudVUOBY&X-Amz-Signature=b862d070c49baa5563c04f9743e02dde0f6b936f41f7e79cc319b09901918283&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

