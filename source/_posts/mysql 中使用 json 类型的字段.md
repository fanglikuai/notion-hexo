---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S42TD4FE%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T120044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB1AaoY8Wi6Xr5naSAWXVNHtbw0LPwUUOtB%2FcclkkEbtAiBfdfcsv%2FlHAMnIUc%2F%2FmZ9nJ9CRcdtKlOsE0aK3ygWfHCr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMCgzO3dlNMOF68SMyKtwDKtzHn80G1hHieOgh7JJlaNLMEUV%2FK2v3DSZS%2BC0vt%2F0aLCf2GG0UzGrXfeYKk7dNRzIFebBBYFtEeDALzeyhuN6RmcZaOxfQFd1BYlFHSYi57qrDKwH5yjuBsZVwIBWl2FJ6ipx3eflDgEyF46wBSYeWSBcGK1lG1%2F%2BoA6IWkLC8%2B0jdIif%2Br2svAbCYzIkvR3ED%2FoFNpfKFp9ta1mpnHUcWGIvjaGjvNj0bRk59A84%2BprkqLobgA1nBmlfFAgtmTVPpb1nfNQRf0ETbkVTuiB5WriuTGf3UcOep2Q5TCHJfERff%2FtA3rbhuRDaa9r%2BpTrzsjYXZjr%2FXcDw4b2I1zyKxZ5xKRGD4oQR8S3Zn2WFh7tFMec8KMBHa10Z5uvQkWh1XMikLIk2DSq8JExfCE7cRj7wLVE6wBAFE9a2UoY4nzDGJaIVpb76sqjwmTiUJi%2Bs5HlcxYWpWFx08X33HCV%2B3nLX%2BpPHh0b8itBhUlo%2BC16mFOrCW0AQrhyr1nVyUC796auyotQTS9UGDqHxdPMShoXq7lHwVJ9AGxronHbeGpUA1bHdxBv4z4%2F7CoQJi9E8lG%2Feh7%2FUZmrsrxe0z1ygaOLAGjYZ%2Bv4llM7vc6E2pZP5%2BR%2BhA2XxLIGQwlp7cyAY6pgGJBrHTaVH0RQFJTLRW3cVbUlY46g42ICXbjsSUE4uhI7iU9yuZ5kEie1spg3uXXHQoIvPp0F5g5%2FPrpKADeua082mWCCoNONKYQTmjNyexJ5TkOLe%2B4S1Qx8aAj3P22wXPrUv7539BXYKyBF1pRn4%2Fs%2Bnbgqrw%2BAg6iNe9llvtElF3DvNXCCcMO1nr1rYdOaoKhOrauuXsot7%2BhA8grsU%2FXwyVXbNX&X-Amz-Signature=ff6a7c7863e0a7b59bc6ae81066f0df60b539047c51358cbd18de897c8a299a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

