---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S354QMPC%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T060059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDpPpGKEb4hcT1LMn%2Bz01rXuFcK7b1ib8aDVSJP1VrlFQIhAMpakNBm1VGiqBgH%2Fu2Z8bMwwf8MUjSxgdo%2FtJ2rseluKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyfl89CLXD8rvSAHQgq3AMBG76FF6Qu%2BJ09E9xK%2Bzv1dh2XDjTwnM55PBe3yLFiTiodQe2ekdrHL2cHSR196oHb4R3bqNeUN5zgdUe1LmD7%2BKw8j5wf31JU4X5CzzeDIeLUpsig9szH3EVgOWzZGhNkG2yd6%2FwQWcbjNMYGGTIRs9WBawq4fzqg96H8KxWBgY7LKK%2Fq4rY49o1kERMwNjaNIZCqaaejei%2FLaPf49OrDc8kyivGaiGfBYp%2B1KCph95o38eN3QtSK9FN9KaHblPOazPlOehMYgV96%2BNjHzxRp6MweQHeAGEle8KfJeqdCQUabLNLwuTpewagRwZaIBMy%2BjK%2BA9vp%2BBre7nkSRrORJLlZn2y6%2B1a7Qt18F2tWppqp5zboF5Vkooz0v2EgxY%2FabmdJS9RdCqpx8oubup1HhXooQvQflVAW1tP8vSVWPangLAPiN4rhasqyL4XmQvokPpEpTZTtCbUMdT6vC2P9cjM2xZ4AivvrSmY1nJRBooOOunwrWi3pxV0Grk4hIhO17GLRXtGtx3mE7%2Fk%2FctgeNirK0sODtDQxaDkz%2Fee9S3dpntPH37ViRw3IQo15b1fME%2BCzdvylMmyLiBWuA22XVgueHlD7PO2kxaVjfScbwHDCx1THGsO7tvfDjeDDX7%2FXHBjqkAYKbzm5c0g3BHkFz%2BzqagGFvn5VuJUcgqLMgeGSJbvkv2rnQNgKZXzRORlox%2F7GKxw2dSdjBTtP1WrDerMilUk2Zw3akYHj6p7AmCxdaLFWgUml2b6YeYX2w9PAyqWsQhZx%2BZJarqpwp5lnYulB6zPCsy%2Fk6lDHx9AFimR7S8MPY4o3tVoVz2EtOdiyemNuwg0gsLWLLckdUFT7gXX2yFNQOwPDA&X-Amz-Signature=13b1ebd0018e3ab7d1b4a0aa8a3ac9044ea6f114b7c3c6566f7665c6b3c54ae7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

