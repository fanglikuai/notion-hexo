---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCLNUJYM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQD0VAze7xofzPAbbvyK%2FS7L%2BJCCaddzzgccEMOI9Gz6WgIgQqtzKQhjPL9TpbPGJ3OqGOVkDzE3xhfmMuWgleVPRLYq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDGZ55Y%2F7656ZTASo2yrcA5L15IA7cdv%2F%2FDXXGHxsioyRSo1Oj227178qJmN4SeqqOqvcZZ%2Fv8rGM20qIC6LPstCELdJYNSDpmGV%2B7Gl5kqcBR8lI%2FJR1uABo4lCR1UzdNbpPPICkAEx1Ip5lkBN5BwXmL1q9mDxnbmAVUa8jRtAaMV9IK%2BYMzg8deED5%2BcABxL7oRz%2BqQtknJTEOCUr3xstX%2F%2F%2FSWpS0xypNFPdSiDrrH9%2BsSUJyKWMnscUV%2BpdknNkfFnG%2FXCHafo6VlJBcBN0iBIcKzpF9IuNAynrJyJenvIGK4w54VsapUR7JRJwRGEh%2BLwUD5oP3nDvHUolKhqRvG4BUTkjSbbryqqYt3agatCMSnnNaDeV3S1Qws8xRDSZARyfdmQS1DEyGJ2CECG5RLyYdDLhzixKmDiwFhQfrwuaIW5AhOMXz8nD0IAdduRK696uCRxjRi%2FiVoc8gNFJMZtBgkJWiVJR1MT%2FtS6SiulBrfoFCXQToHBNiCBjQAaQxpfsUH89lnJXbdo9j4mXuqMokW5DoFfyYSV%2FbS%2FDNteRY6DaU4Pg31E5BZJQSAOZwGRd%2BGf45G7BCHuSHEaP2AlyWbnlDMr%2Bp8ECjj%2FXF96jNiOjoUk2CqY5vGpftDvOWWYpK01FvrK3cMO6xgckGOqUBypUzSRS%2Fxh9YS9dBVTAOUPAlkadKL%2FhinJFZ4A6ONYr0taTWIAUDdsjnFkSaKWdTxMMBsQaCGJZHQbpf1S6WFCI82jHgSuaGHKG923LXU26Hi0vu30xkBwY8q%2FCDgcg%2F1ConrZjT2axTOWDUyZZLpwIPntyPiowiYTFuQKH%2FtKVook1lk7t%2BdDy3o4xQL%2FdAmKGzd9MzBf79nlWDafeZSxM4hEUW&X-Amz-Signature=4c88697dcadb36398a0221a93aa308b0e3bcc6c3e9db79fd35f86d24e9ac2f60&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

