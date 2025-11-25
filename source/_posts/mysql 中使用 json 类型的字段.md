---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UOKHPHCB%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAVOJYToJ80AwPuNzgqMp0cAicRvSSUC%2Fb1WXAr3RdU%2FAiAr7GjXbe0RR%2B2qZrbcyc3CuELPYNUF9PEi2tlCAjaBrCr%2FAwhwEAAaDDYzNzQyMzE4MzgwNSIMUG%2FPep%2Bf3aJFw2NAKtwD9rRCcOCmRbxbPXGIhu8Lry8f1QG%2B39LU68YdFCL5NVcXoxCG7HTOCKqj%2Fb6PqQ4MLdmXQuKSoJ8SjECs7mU%2FOHadZA0BeRJkx4stOeW87rO1HsQMGSW1tnXTmYY2lIzBRGBG%2B1zX8jvD1UDVOFFvM0K3ig30jFyjvoBkC8PWZnSACM8mCLjvHsZd7ZqUHjR0kBiQfpELiMfxUNFcHcKa1wochIlZehdzNPPR7%2FQWP1T7Xa4ZVD1wwVv2ue%2Bt0ktxcAeoTJ%2BhrwnQ6qTE39ZV20kzDLB7f16JhYF2LDsjpLKbffut4PuH%2Fk2bLl9pf5KMcgXnVfR%2FPIIli2jCqABY9qYcBTy%2Fky54M7AC6wDxI%2BUYT0KpIA42kQnOyWPN6XosNfPeFrPXasaRTGHQzwywTRF8JoFHzecZ4oUxctgMtcAfhcUR9LuEI2bUpIwhNqGdR7uScriVqtXzKgATNMUXZpoYazAZIdq%2B72Ch8AaQMPKJs3yqzcSZ5HbBnwobk98MSKX1hOBXRyAQuxArQTeJX8nfMu%2FnFOYRlAwbMG4IIJNomAJP0p5xwRWDGfywZs7KZnxK%2BfVDiCckyGFkaPFwSOzfvnb%2BhLPofNIRowHpBxzGqwJnzmqggMwPvLEwqYGXyQY6pgEiBigg2oMXik9KkEKaG3WLrzmyJz1q3JscqEO5WoVRnTu9PBeuWKv37Z%2FKJzW7iELcyetOtdhCOvV56MhFDA5gueksn9MikV%2BfjcOLrv7VM3r7BD%2BWCXWZPPytRS2D96k3V6QCuTvCR%2F6NwuOoMTnFRt9jv0jhP5Rm2peuhsbEs%2Ba2ASIxLqA3Dm61QawfhRaClYDq9Qt3ERbms%2BEU7VC12KKUz%2Fy8&X-Amz-Signature=eeca30e3dd998461859c5585eea5790768734c75bde54d222af2858a3b42cf90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

