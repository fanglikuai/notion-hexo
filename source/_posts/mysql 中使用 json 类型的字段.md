---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VVG7IZ4%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIHrV6zfO1fruR8Dc0yskLzEOCARPDbDeoIZ83GFK2DcMAiEAq88S4gsZdY3Bxn1uM5rpXedXSWNXlO27fT6B%2FWKrPmoqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHibS7pCAv%2F3pHjJYCrcA0fceBNHDhu6hNO4tDIBykgzRONExPcxlJ38fHI2TLSRvKmUiId8VyyciNVPPI9GDm%2Fw4B6F7vl%2F0Enh2P2qlJnKURzCyAjF51ptD0Dt%2F4NO%2BT4ymQC2PUyD6ii0ZjQeD4RVn3trtZuyPs143r5jYpnmIZSFBUZIH94YS9ZJDCjvGLpwTEzGbCl5OMnwsebDeu7OheDD9l%2FILajZPlfDORSNPSDRaxkSpxEabOz8lnRRT0o3QegeEnI3z2gmY9ccJ6Fm6gyJ9Xc%2F4neIEt5aWh2VSVIK1XBKZ46I27lKjkay7X6fuiml5H5r%2B5R6fZ0ACBrFdn%2FhrRo5YoKEuubC%2Bbjqrzgk4Xam5jB38A0%2FqWZxX%2Ff6nmjq1mbqPW1XQQ38SaaqapOL9nEKhTGV6kQrHHwRzORaRe6QtONfPW7LjUSsXWE5mTbKHcdeMaCTfEYbNf8%2F4AwAAO1UusvYw0zaVNl17B6FI%2B4xDsIfQno9R2S2xSGzCFPTwY9GrnveBWegCaiOxb56V1q80wa8U%2BL86nLKeX5j751UZAuJ6cR%2B883ofPu8LoCec5Qj5qTT4HJNZGIgqrKbNlnKuZ%2BDWJiASCuP%2FiB45qOXIbWQrkt23x3y7VdpGVL1i8pemzISMNzetMYGOqUB%2FrAi%2B043It%2Fp14e2CEj2YjU%2BfaTeaXEl%2BTid6RKJsVIktYhIEHulT%2F3bht76VSiEmWRGlZHw4ePOsElDG6Ezj4KbqtnCRMtaC0E09%2Bc47MoGUbBsHXbiDckdn3Mh86Gwg4wgOvrysxD7dD4WvCyBk5C6dyZserurzxFJxZpHPRCZqybU4cq6hicTjwzn9ygwYuF2oGjp9pLK3Z2Jh7CTL2qGZyEh&X-Amz-Signature=c8f57df63a5add59be26d431dfcb36ff7c2a0af0aa28ccb683844e6f286766b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

