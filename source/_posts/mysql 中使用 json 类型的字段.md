---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667VRJL2FY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDuw%2Fb4BrWq85t1jP2CX2L%2FKhdOcgM7Bf7BTJeLhLoQBQIhAO4ZXwB9YQuA%2FL9rxfCMgx88MhYPPGDlaNmd7WopNO%2BAKogECOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxSPVGicUItyeWuPT8q3AN0Cowtw8HGxt7yRa%2BAX%2FFWzmSM%2FVWt1%2F9KkWjgq7r3LC5g2CyOxh2qvpevPyDtO8isbGtkm8mrGQEAFQwbooUGSe4nplHTZwD8IE9TokngIV4TrRVbe0Uy6R2i2t3RWez0hUCg3h0bAEUUv6UxqeT9f2KBadOL9RGwyJlEp6dt4IQOLtNdqRTzX8AY27fp%2BOp0vyYzCfGkuHULahXW1uFBd%2FR8KiT0o5o25Mo1tIMQ7QsmXyve5gdPx6GiYxIou1DwzxEUqkSPD7Iayz2OU%2Bt5ub1lX5fFcqMz8GMf23emtHVeZxgOIol3vV1eBfm79pbsd%2FA4%2FzAcKgLo88TCaLHRt10NAahIM1ZVWFXlt8DAw7j2vc8cXrko9BX0MPqavcw3oGixB3uir8Q3oXtdCpXG1rDk1R43DLY6Qlo%2BLotHQNF4OGBKKcEnSLR7C0zldQF0MCW%2BdZEmgs3%2B0I8iVSlhHpUlgleWHHO1r8ul0ZGZO6GstdpZLzGLw%2Ba1UQjg%2F7OxvkMkO4ubmXIMcv0%2BAEnyTxtqGRf%2Bw8AktVwtnbT0xo5oa99%2BsSdwsrT6j56eM1iFcLbRbdyahF0ZIepF9GHFNkmNehspBKhpOD6OHkcxbAfHD238fH9eP6snvjDJ7r%2FIBjqkARkydrzxazCcyVIL3duf5SjP5uxYT8kmP6%2F%2FEUTGXCK2%2BS0ZgFINLqx2%2FcKVMmYC%2FDMObupEOFqky4o6FitjKxInT254B7xOenpjEmDHsE7Db99DQJBhJVw16kTL7K%2FNm88AyWID0zC6yIcVAtw4E22vSjwFuXltFiwoSpHkuX6vkLGDbRZAptPqIPDKw40eBGVTXuI0S5KrIU4ic6l0dtHSZcpc&X-Amz-Signature=acbb628765fa11b11bf0f6a791a3204f59f37222e6c6e2ccc762e67719243d86&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

