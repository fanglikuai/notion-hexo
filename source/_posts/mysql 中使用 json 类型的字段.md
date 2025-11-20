---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7HVDN42%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIE4c8r4xyTWHUu6YTP5ZYHkefrHDHa7ZE39B5khIOD7pAiEAn6k5bkn8duf3Hisj5i6DAsnl2H%2FReBHKLuEt8NN%2BIG4qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVG6CgB90ITJ1z0WSrcA0OdpDJOFXI9V5QvQ9FHa3SHdHC%2BPFoUarIxX%2BLP0073BoXmY2ZPEkn2nwqCvXC9YlCMfqauy3M33yZmsI%2BSiOTCkPIUdgEVueKnHjGB3tcNbdpBJw7KKw4VUjbEjGKADwakOsxNi8DIWGbpMW7qa02OvmsFIz8%2FVrR0lQLrABVM7qUWmUFV8Uh0b9pfvSxeS1rG%2BDYEBu7uf6%2BUxuaQIH830dtlJttAREdAYrMNYitUeJIj1wtIUaRq%2FXe5PeB4H8z1pa0qGWgTMhvDVPfJu3ZK3xNkfuhx7Fem9D5HXavsKR4WtyQRnPoB6NDURen2Pe076spVSoQ7FZ59dNjZXpGSbTXpyMs%2FC%2FhAayzJvZsVfVdn64AI01NVbveHTUDW7opT2Ef%2B7yKN4qQ54rZTQSjUecW%2F2rmYkIq%2F71s68pxxHUz94i2ASIWkyt5iqmPv7lkgA%2B%2F9DMTrCVNwfZT3Ixkd7H%2Be10FZhe2IsAE%2BFyCLW2DvUP8Hr%2FvoU%2FgiUX7QbGVL%2BEaCXh4qWjEas67FGI0W0W8O%2BofsP455F1t57rMc1fcegdb45PFilhMzWfN9lVi2mIGttTMchWOFtxkPmzYud6IpaEb3tGEu88wYZSq7K3yxHnCUZMVcst1kMKuU%2B8gGOqUB2T1YsDIUh7wrU1zAW4iIIlG50xi7bDVwY%2BUZP59S7f7U51ltVQg5FKI%2FJy3LAD%2Bs%2FVKE3LosEmjEX4RcNpYicMyGkeTLN%2FdmvGhl6MW6r2at7Ka8x7HzfteixN%2Bi9GDwZZM2qJRwgcFfyDLi85BqmOwO%2BLBi13X4GcHQmQ3m3poY1KVZJe1gkBFiclfzrxqfQpCmR2w3cxGAOHkkaUCszGnITcp1&X-Amz-Signature=b8979f7a831d80a928d6a20d2d88bce1385e020ac46a6c09d378e8c05102a82f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

