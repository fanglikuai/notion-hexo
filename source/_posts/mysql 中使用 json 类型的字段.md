---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVW22QKQ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCcLbe0BocKFWiwi3ZM7jOtLf2yLp67Y6AQhBKNnV%2BFEAIhAKZSe50%2BEqW5Z5Kce9hGESJtWLeM2gmYT9lQ6p425yySKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0W05hD71lzRG2mWUq3APYcO%2BK1IBNfCAwD9p%2BWy%2FH6mkxbwSn28ZcqJ0MC%2BwUBl76d574wkDBQ9LCkcR4ZLfdLOixFHFxkBmYqsaT5T2aQ0cWw7zNxnIgLvnvukpHefOLSDfREcB0k6Fd7DKV9BzxsxD%2FPdTwS6f94c66Jb3dwZ8bsTIKAjvMS7n2n930X%2F3PDYnePWuSDRUaz1h04kNoZEhAP5Zk8dn2a%2BT%2B0hWJ2WSIlkENBFfzkWxdvOhTAtXQChvzwSRxodszud%2F3YhHx3b5HupWKHAywTNGp2mOHb5DCxtZ5VJ1aoFAvC2NylYJy3VUsYoqVnQFkdDamJHgw7evk%2BG4Wc1lD0cxB6BqFvNOdAEZczXsPV9j0nDGNKe4G4k7gyDnxRt87IHYWCZ2QRkDf2MEyAhuY3Xc56atGSw3jEp9oE7kGHllhCGOubooEmcDqFAdD76fiyYV35jWba3fiPwCxjzslE987dnc9AB9sGaSDpUlGq73LBWxj5Ars1L%2BmErSci6EpHRQC5hwguSczdtg%2FH%2BSa89HuEhbbM2JAXMh3P0NNFjy4JEuTGbA%2FWgUAYmKDs76CjW%2BcQmJdFxvenBtZqi%2B3ToJ1xjaIGzUoSdglTWwOltRVi3F%2BIjgOdC0RiRhpESkwMzDgoaDJBjqkAQuMcQXQLtpylAP9uW2%2BDcRFJ9Sny0ftaVsFLD6zudarYWamMOtMPiKvH2pZfYKAhI8awIcwX%2F8gDw3qrYcmFBjDO92GzTcL1aWkJLCbekUAKJnAXY3oW58LPRxtWscB%2BgzvuvsYjlzrxB5%2BHiu%2B7ogQVNbVRbSEIpFlmn7TJMfcjqVchlbXtwJbZltgtf8wd088iE%2Fe%2B5ijvMlT%2BVcA%2FsY0I7Dl&X-Amz-Signature=7f0a9dec386bd83ad49ed64e1669873a3850e6968cc1f68404d6f5b8adf185f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

