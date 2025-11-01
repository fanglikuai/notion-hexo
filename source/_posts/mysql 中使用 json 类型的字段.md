---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3IMNLXF%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIQCAjzIBGxq2dN2An34QiCNvbnOcTHBgnytYZcAkQy%2FfMQIgbYyAaGp6XEkFZO2aADNtsPeFcierZ8mYj2ZYuq6Qp%2BQq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDN92YpHRllxXQLzsRSrcA7xICYCa1HxYIg5LPn2lyJwojsrkiJIxeDgjbpCDgfxp3isCVpMvNe5VNXOIALh3lYCZlTqN%2BmJSX%2FC2IN%2FSVRTj%2B1idZ%2F0IEH3UHmSooLT1MDoMzTrKh3SHAyYmEyRkPT9r3q3%2F6fL9KOIvRmbRZ7YnkrkXOVs58KxAZkkss8TVvZRZ0PNIfEKnF%2FAP8HdX%2Bj7URcBOvGh42trZ1dZKwq0rgGYywgG5CHrwXOSNECYaKol1oflj8CQEMeEsjXSDyPVe7aer9zTfqHtUPEwPSiRcSOB62m30HAaXRBp5TeYL%2Fefy4tjfrN0RRVApdzP53C8kQ7s80d6J4lFsdLZVbooh%2BxRNbuyfzuXYaoL%2Fek15WXj9ynvbSjVPn2T67btEC%2FmSbYpFzIeOeQEz3e6kHapHL%2Bf8rF8GfFZ0tEqor0QuZ0xXipVadrkNVDQF43%2FVv%2FZJtfDHJ%2Fia7%2FwbFmk6lb0N7Y9ssBN3rTzofEmWGq34PFFG9z%2Blwmo1TF20rGjgnzO0QKzV%2Bkw1WzUh%2Fu2xBzmfeRlLUbBUrtXB7RPF9tQNwR4nCuZL3S00hRcRBTkc55CmSjCTrsFuqgCoG0IXYoFEHlyKB6TewEQFnoDDO1WV0r5xLlb4kIfp%2FiC1MNH2l8gGOqUB8TPDMgUks2RfsC%2Fm5uwAayyFouHN5T1%2FGYc4FRciiNtkwjNA%2Fu7o3qG4GEkhJprrAapyzb%2FCdGjIhZRkLeWC6wwc0XdoXi6x0ZWuo1cPZec70gkg2ZleFGvqhCZIqKrf7dsuTlOLTSP21ouIZGiNm9sbk2Lmy1kDZMpB4jXl5N5iTC9DqNsMPie%2BkV%2BZXKnpCA0k5JmWxZanotK1eT9K1HEvU4Ek&X-Amz-Signature=93c4be505cbd4815216c6823c882defccf841091f50f5dec38654b41573c97df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

