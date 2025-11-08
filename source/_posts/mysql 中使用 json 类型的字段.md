---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSZS5LEA%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDVXpDKmoNrWTuNx0JZODWB2nVA3tufYGU5REKnsHvlNgIgW4w0oalr%2BYHkTMLyBVhjnCsR0AlQy8XN%2F4TC8TtMcr0qiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPwGVd3SYipXmiWgGSrcA3%2FgntJKPw%2Bdf4hvNAqC3C6brL1POnkRmATMfq6i7z6wdrBoPaYo7oe78%2BzXXD1%2FNJlhc7%2FFpFHpoSA122nR4Iy3gf5BsttPPmwbmoPxgLszGelKe2ZSme3bZjQQD9TgWTVSa%2FSca1No%2FcGKT09LEi0wmlfw06AgL4KsByaax7BsCKQI9lf2J8ghOBuNOekR%2FwohnD1bAsOv3YemdVffnrAwXgZJ5A9m4XyaQ5yty4h%2BIDIDD%2FdBMN6fC8VrJy1lvNI4Xw2lNQORA15iHKSAtrS4APZ%2BD2tuqO0tacNgDVfVBRWEJsqXuUlJF6f2gHHwf1tkdvppwwOS1HaDLtlfhl0%2FM5cjIGYAd1lYAB%2BOZkvJZFTWbS6Op9MyowgLr7%2F9dGg1sGfpRuJeEN3D8b6XMEqS71aiDI%2B1Xo2t%2FUWdzaMyqM%2Bpk1CV1yCHaLCIRpjoiEQiy8JOPKFhsq8YV46hDS%2F7ao2My22s%2BbnX2gwlqpkVnfZyr7Nznp%2Fl2RW1q0RmTuI0elcTNLiIm0Vi9ousFxRV%2F9wH2YsIyyLdYOEs1Zfp8jiameuTWIykGhwe1zTbu5ec8r7UdgO95dZYyQ%2FhfU%2BoGM%2BZNBTSqDPiafLeiSMc35UhvwaT%2FTqPqk8vMJqNvMgGOqUB95YgvRLvqf5Qy8f%2By7JBrJQn8BsGDewBx1EeuCTUSSQGW95SSx2GiKCyhpif731zDulG9J0SRjUeUlibMzMDRchquBR53Kzh7KKWHtmPCpbHjqXaVlWPuLww079NgLueoR5ZPVITsEnMoRLWyGtkZ13ULUEwaNubWkXhr28ec%2F2cGc7bTBwMmhAuydly8UtvP7fsIsSlnpZLKWF9jsUjRdIhbePb&X-Amz-Signature=0c2fd18e4cb12babc99a481537fd78ec44b6eaaad3144862cb1ba245c10d1bef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

