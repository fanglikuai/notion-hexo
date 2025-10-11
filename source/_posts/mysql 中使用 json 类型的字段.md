---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6BM5BMF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJGMEQCIEyJT36lv01J8j%2BSv%2BCJOvlXDjlLVpfjHN12GSMklF1KAiAGyPiDCFyOFuJ6uOrW8pZ8%2BSqTcl5Rsd9G9SXbXOEHOSr%2FAwgcEAAaDDYzNzQyMzE4MzgwNSIMFBcCh4FD%2BJJe4NbRKtwD8DPyc5nkqdhh3R4AdcY18iCkCA5cKT%2FsycscaECS7rGGr5f3r1W6RK52XneuGD1v%2BQAgQGPEpkuCbYZNCioL9f2%2FthaOTIt%2Fb24%2FrXv7hTX0tVysqC2t%2FjJRHvfg3o9FRhq%2FQ%2FYidoG1iceP2zYCsZ%2BxebWE1Sx6W%2Fz7Tb%2Bnep6yPJDIvPzUpTroTFD9VMDDFoDshCNThSqMojnq8gZLYZPSh4sgbWhPBi%2B9BIrGrtwq5kxFJqyu7k92R1HaN%2FD6me%2FibLPnHs1fW7BZ1AuVZo58bAqFJom27s0negst8F%2F3npKabgxQfRTvSS%2F34TpnmjxrfYSU9XMy4LDubV3AV9l51zTEg7Q%2FuN5BR0r0oLc82FXmJDICjNHA%2BR74TR7HALfvZL6EHhD0g6%2Bt12zQjFMi%2FJbPs0stc1hw%2B0QmZnc439izw2rcYjmibB29j5okE2WhDExiqaq749Y57YzUX2%2FTDdl2leMMDrCXQi6%2BOV%2BiDABVuHdl3p3n10a29oKf%2Br6qJ2gzJKmYNcMX5uaHr94kC%2FAPOMAYLBONPxS5WXR2cXcLKmlIwzEZw5czlShdkI2FVzFL5Pb61MjtfWhR%2BMjNjIJIZm2OrFEdqKudvVNsCwLkWH6Hg4Qd3l4wh8WqxwY6pgE7cNCp5WCKvxxXQj%2BtM9ek%2B3rlnKQ3njRcB2tlD3sjIFONlVCHrxhvHITFG9bzRSrkzC7xNwz4rRQU7y7a%2F341bnmmeAAZGNaou8h4cQTcR4ukh25on23UrnAuvGgz2hKLWcbxYp2KSmWdv0%2By0LR2ohoqCGXDH7N6pFiUam35nRl4YF4ppOB3jyDXEP4KJL7fB2nuf7tQbZA0pv%2BOSGQrqTuo5qqd&X-Amz-Signature=995a8d22307adef2861b3ed7dda021fa3a85d453ccb887258e47716aad7a395f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

