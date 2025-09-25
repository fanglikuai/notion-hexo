---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YHHYKYR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEWQjeQ0nRndFkXofnupw0PVDKBY4n1qH1Kut2ga9NwAiEAqoVtMVc3BwSVAdRlP2rlyBmo0bWAaA%2FETIMKBIqKk9Aq%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDIhuHWyTxznX5LQwiircA0PXztK8xQdtxzVsn4KIGAqDTdDXnpC86Pn5rRp4P26%2BYIgMIGi4wbzvWKrWrPGXgrbjyib0Wsw6VTCOaLcyHVoSGXEXyzSBMSywqT1Dvkq%2B9la924zAfWSSx8t0hQzrnW6pAidGPHfvIih6b3w3BDhogYBAiLxzxMy4pcdgMftJ%2F%2Bf08EGp4oXCSJgpeUBxlafmXT5%2FWipVT69YPCRILoF8WiH5kwrsW3ZLg4fJyr%2FnMhf806YmOi%2FTIRKbgk5Zva4VXiA7rc5VvwYD69eveDslGpOr4KODrPx%2Bx2VeHfc7YLE7p%2BCn3Cm7QLkccqzybbbRfpgUeORVHtCammeQl036KnZIIJsb9kVDebx1Ybzw5O9SxcjJxA1rUEaogBuW8J2U1LyvR327blQQO7lk%2F%2Fwu7ybVUFsWXdU13x4wCjzRPPeUNDq3htvk%2FgaL3vrG%2Fat9bsHy%2FA7YdzuimaMkFk15ZdQIFXydi14wxlnW2NOcsEsU0OQrJpcg%2Bnwl9h7iPOvpDgBRkIsZMsckbjNg1vSN1th63cc9A38FizbokvE1eOnnTlC64bemMCjIiJ0nmKEOixbzm%2B%2BhN2qvoXW1hE8Vo1WTaQyI4UxHHdQ6EHUPTHyMKqtBWsjYUsX%2FMIqp1sYGOqUBj1RR%2Fj%2FM4r7QofhsgTG64lCOh7f6xa9%2Fvwsg8QdNUVjLOTSOxheFWvr9ayqBBvUvhoUjqsw0VLDgH%2F9UO%2F%2BEXwlVK%2FlfqnImnZ9RzZWu6zRx%2Bs93i5e9TeYddvcbNLo0Yg0l3x00RAd1DDef6EyGKNEXpw7ZjbV928C2kza0TS3BKWc8XMAyL8G1V%2F0VaX4FjQO2lkA2Hq%2FZSDyWmAlmCA2kZLNQ&X-Amz-Signature=e1a4446a51d74ed89f946be3d749f90242fe452f4a7b2c780f8f973dd383ed0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

