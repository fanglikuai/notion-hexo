---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJT7OWTO%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCZQEEWsUlnDIRSni%2FRGGcioqyEh%2F4snDnoM5I3H50wQQIgLJ7WWoD9s8mNdRFxnGEo%2F6iV%2BC6yxmWiRFdMv6X5H98qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2FMAqET%2Bj3gRJ9RVCrcAzxcHqMrXjvHXFi1tNbh1UMRBa8346zaEH%2ByGPe0isobJHELvcLobD3Psoo1MjIEXKgSwdYfcr9FtsJGTcZPzYMB87ZO288HJ62ER8O5LLisdjVDjD2mEO2nq8SdSLenHEy1PS1Fyna5DnBYDFfOm%2FeKClXsUobtkKtJO%2FPNccr27JuQa43fgIZ19Wl8xxlB54dm3bAl0GNf2RJYuVH3djclCTqRIRLf9zvjPAIir%2FWUMucDKGRWz6zZ%2FkAXNRcufRtPYNoygXUTHRgumHDvvF3JTd6hYLNeHDCjayRa5H%2FkCKneu0Vl%2FzulrHUKDCCUpMeqI0o0hVbq%2B1QsGrFa%2FyIkZkWnFn0Xh%2BJ5EikfpKoLxVEhm90Fo3jDliya3BxrDKDirxLqW85DrXoCKlMuqtyv7xmzmJAhzNz9Az1zwXkjUtYgl842jwtTXzB28EQtglSM1derW7Z74wEWQgeR8WNweU87hDhmZL8t%2FhWS5n9wJRsPi5SklOkAvHViEuiRmgnp3tFkYmI7%2BnCoM%2F0jL%2Fnz9py7%2FkhnKlsXdVbq19DZjurA36I8WHk%2FIZ0Gez4Mz%2BVi234d29kqzqxdVIlQsxUTjNDl4h0gqTauZR%2FgaEJAveAFUenHgzCLM%2B%2BvMMm5s8YGOqUBkBkIgNecbWJmWTTStw7vTTdTkP%2FU5fECv4u7Aj%2FKX1yyvhcNreb1Rqo%2Fv1WwqnfIsuBp13gk2rlt74K0M8Js97Dnf0gHHLESF%2F4rZdebLCbp6h8FCK9W9BFfLS0EEjPgm%2FM9TTdnqHEmvToDEU7sMHVJUHcycF8h68PEZYGSbtBqyN4I6BG5MuZG6R4yLeDwaxV5J9shxb6kGlKZZM3i%2BPs1uL%2Bj&X-Amz-Signature=5bd6240b9a19f789f5a43914173a0e230fbaab649f227cdf03e328a6cc0f5c26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

