---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXIUWUXB%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQDKvJnH%2Bxtsv%2FVyiw4c%2FXzoLZ1PgpBRiqAvBV29oSGb7QIhAOvk9MSOvB2qTm5imSQBuQUpnGDVfJRjfYXdVeTcaWEAKv8DCCMQABoMNjM3NDIzMTgzODA1IgwYAwzNLIR53elahNQq3ANyXAgS%2Bkx%2FjfZSqoelsihgQNb05b%2BRYbwnlPzK5BUeA0foAmEOULUeLoUSHuRfcwu%2BMJl0GjyAOJm20QJ%2BdA1ytCWcCyMod1IBoi0hZDuSD1a0stFpd5HHtvTden%2BYeARyMuxl1ldEaxZGMiYkp5KItAdZ9tC%2B0aoNrWwpnZxJlz5qjVKacfZ9v1GqabItqc%2B7ce3fIqmEe7e68N7Ei7ePeLkty0LMlfiv%2Flvx9aSIbu%2FJmH5t95dcod9rvMl2BpBS5N3Se%2BgHmy4dbR22XaW9VxNorm%2BMfPKKFEBNGO55RCGE5O46dAgX%2Bj%2FhkU6iCx6SlmfCpljVl13RW3Eu%2FyeEExALikSvtMFk0sAbnnpf9rZ6nH%2F1OjZ9JS%2BgltBYjaT9TNlw2VVeKZhrV9emLFzGRwsP9xYjmERYqFDJgyLFgI1PinBjPuj4eAARfMRCFMBScs708XFIavvPr4JDJnAUbo76WH3QappRcUgK%2FNmMp5JRM%2FCqV3G5tJMra6wzI5XV2ipIT7w1QznGfBuSW1bmtrBuGXFzU6xrhuGRYUVE%2F8qy%2BCYxPPWzdRLvFIp8KVUtN0cbvhpTf31r08thmntn9%2FbxewRwFk4p%2BNcF9oJ1dM%2BYC7cjOiPGFwhhUjDixZXIBjqkAVs5o0XqLCzcJrxcPLdp%2BbCC3tl3jcSWbOUMj2xOxlOc7%2F9szEEE6K9POl%2F93MRLB%2FBMIfbLSYs2%2BC3gJSOFS%2Bs2X2bmnIepG2F6mjNJ%2BUrzdt31blHT3iavt%2BSdElIgheAhwz0sp3Yn3tTFTyM5gVmErVDrGGnUUcYzTP4GA%2Bs410ib1Qd0c8J9YTjdLeQC5f4%2BfhcgctY%2BCCdvq3bx9I3cGpAc&X-Amz-Signature=d676b8656680b7d12f6e6ba6fc20c2f3f118cd451e41dd13f7d4350b37bd2e98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


![images944ad29a7fcf96a0c51a577d6bc43317.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/2a08254d-ce2a-49e9-bc6e-96fd291b7ce4/images944ad29a7fcf96a0c51a577d6bc43317.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U7UO2GP5%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T020057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQCtkQrnk5kAsyrvxMUQKnJfHp0DAHKrYPZFDXcwWpPRlwIhAOWXeHsVNqE%2FLI4moBA%2BxV%2BpFBl4DSSOcZ1HxsaOc1NBKv8DCCMQABoMNjM3NDIzMTgzODA1IgwYZyT%2Fgdzn6JtTcu8q3APE%2BMCDlWtsqrWOT%2FMPvxIxngkTfE8%2FRHfJL9NhUHa0Sj0%2B4PeEkkp0BO2xtWDG5QDqf1OAfjpCYUgp9sajuZ8Mbvx7QMX91wgB5coXnp7PuxgSTvUIFi3wpYMEk6xHtzba8lvXWfpQxxHsOxOIOyg9R%2BzPORYkojwFzDYP%2BBwnxwV3CQpiFjitxql3EDVpFQZeh3TEOnDjXdRYp5zVLu6ntw%2FtDP%2BxC1gRdz7VDQNjER2YT1zeTf55QQq7WywVtQjYZ1XKHReLcSevvt10VaAvp9wp4Qb1z%2BgcnrNIGoA2ZeEmE99KjQf8MTr0gBxrepw2p0CzRYZEEi%2Fucqic8NPUHLsZ41AhGObUoK43n26%2FLJKMi9V%2FeS9S7E2rdQhjDw83ZY1NziGqnXlsS%2FlEMCRKuCrQ7MrTiE44IfweDFkOLKoh3tEtuRgaQx%2BRZcrS01qF0EksODMj9RLbrjINIcZe%2BpshPOJV3FCA3%2BQOgVvOjdHdkSt39Q7EmIMuux64X5ZGM%2B2SyxSQo2cwWfls76Ba%2B5bz078gvP3%2FHDKIiXRXeDWenWA2pZopTcjecoXbZxCQhPasZTG545IybD8fBaWnrcFyubHzLPgyKtkc%2Bd%2FhmlnG9CbnKouLdkF81jC1xZXIBjqkAWfi8ffI3ZGrMphgeOjjSqFcNb7RN7voVHEBI%2B7vXo4PpOrva%2FWOcJaaoYJkrVJYnbC5jOCV43WGNIkVMFnx5L%2BvtmsenmOHMTpatF5uTlBNJCNKquAQjq8v70yKCZpgtaEGiqWt8sCBUEk9l0hKOmeKw%2F5VbyULedvCw5dVpmTdw54vDOnmr9cXTS4tIsBfLtSnOMsPu7phLhIcpzmFzTjhfdmh&X-Amz-Signature=8c9bd84e60e8bc23a4f9657dc42c5e522314ad82ccf763977505bcfd2d117895&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject)


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

