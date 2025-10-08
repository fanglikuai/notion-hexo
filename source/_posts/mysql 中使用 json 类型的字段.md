---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQC4VN4N%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCpnAAVT6ygu0rPqn%2FCS5b0IvztTd%2BGuEoI63b%2Bz5Xc9gIhALLnhUVhOhbZOkQQscOlKEoAi1mhqowuhCbuxARF%2F1ugKogECLT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgySc7aG8ujihauKkbgq3AOyk3u9XRvNFx36uwHrX00szPlSKez%2F3Qd6WM066PdXnNFehb7sgUvT%2B1%2FR4VMfutR%2BxYjaW4z7ndKacPk58OnBQ4LOEQNc622BW4eESFqcObp3dNnWkGnnaQzI2EUYptMOJljY1JHALyKmW0QsxPz2BVYFPS4tA2V4quaQ8At1e4DBDDRt8jkiZR1uLU4JZAE7Ezq4sMR%2BbkmliMFofV89AylCefaOnI7uW5al7NqPXv7FTs2cFgikHxWaZa39MaFLJfAzt4cABg4WT6lpbyC%2BySs9IK%2BU1CEXIWuuZ%2B8GhyFv0m8afhe5bkvW5FC3NtElaTZsmwqiV9QSk1ylTLpNrvqAimYMyWW8C3q5kOSVsWYz7vBEoiw7TFpWfvTapA1SKctgZk1XQU4mXRXauf2W4G22iWVptgJaN%2B9j%2BrHUh6orG8J8XVIxJp3iygTiUaLgstopCRickWRN1ASCq4CviZKp9Rcyed7fjlVmDj8%2B6X1aCcX6dlrItgYiQCibXMLhzsmAeHF43gENMPOA5LHlkLb0GGBhsThiIGrkwa96NEotss0Xv3lH5TR3MgaURQTNKJ2TGepsf56twDXaf4Ied37%2FW8a7eu9JQymAx6jmWUeeS6MgwYo4hVaCYzCcr5fHBjqkAYtkRXfREsA32%2B5S8%2F4WxVSwIMYmcftHujlfbltk5Owa80TC4CaIcY5Qpn6i96BuHMrQ%2FaD3R3dISyng5mrs1n28QPb5ywPZWfTHO7yOztiSJHiUFnIo1Awyb%2BY%2BstB39LfSgtKQ1hyvo03%2BIRio25TI4mksZPJfL9krk8PXaweGrPlUH5Dji9tkG5LvxL7kKuRSyHWrA0%2FWKWRXxlPGzHHuTYfX&X-Amz-Signature=de2ae101ca1b65e92e3569cc087f00b25ed7d57ae2f2dcf0aad0bae58afa2936&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

