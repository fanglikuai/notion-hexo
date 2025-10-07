---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667L4ERB5S%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T140051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIA9MgX5TbP9g3F5G%2BRD5MXob5lnfu%2BtC1YjkeS30uzNgAiEA7%2B67A3w0IFSAz0TuGAs3xxoN%2F0y9%2BenNddFsZOJeJDsqiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFJQT1LEVsms9soSrcA07126UhFBNeMuXa6U05llK40ys%2FrqHPVjIis4OnFfJmMSyPhct3ld0%2B%2BZDDNXeZScad4sLXSYRFTIjvax1YZE7GvXeaSALsYQA%2FYmfFhNiW1su9Mqpt1%2BDyq89AqzTtLiESeB5ZXM9dzT27lgK4f6E9Cp7Qec6xDDzCfpT5HFrn8FccHfPIiQTXFYmjJyAWevnbNiv8MS8UhNZtC8aSOHGjt309czvHI%2Bn7YTk7AfjHkk12IHRENw34WAqz0OWG%2BxrnBfgEa1U99m7kPN7vqSnBO1CW%2BR67LR19ykeaIWlMbpFLMBcjUx1eP9ybZaqawLYIMBkLs0xLlvi0QHEOE7m2M1KPRvQhx71LtyUir7DBFduGeaEyHr588A8eTrP8ZVt2FfESmWecpAtoQjGQwf0g0Rlg4OgYeQfF6qpwLBpnpiDXKLFivRzgSRdE0h7vymEzL7uh%2FppTt2EjadObh43Ojaii3zfOVw%2F2fojdWrG7SCMSh5u6RPzDJ2Mjz71aNv44gmpQunl%2F7PdF6evDr6FLACyFFVi2N2avOZVEK1C1Kg6lqZcmn9UzN0kz1qgx13EeOKOXvEA9jocmC%2BTDDhO6f7uacxgfr1lcs3zEx54ku7Hc85ayEdYQfM0BMKOclMcGOqUBnHFSJpri07EQvCF5TsEp7pbvqu9R%2BWO7gUuWa2iv0jy9y9vuftZZPyaPV3ew1MLaX6X4K3Rvznix6623W8QTHhl0fdySoYcrLJaTILmLJiDOor0YrNqoc7eAJN5wvVNxewocT%2FjnT%2Bkful0yc%2F0Hwm0F7kWQGMIG90x3%2F3B0MzS5TudTYMMIB5Nrs4vd82%2BANKWfbubrXODlcItMIicLAsmZ8YHr&X-Amz-Signature=5afc5ec9a99b84e89fd9d64195a991bf83807e6131294a9e8e04337b919440c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

