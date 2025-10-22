---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664N5SYXG5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T030054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJGMEQCICZkHplRQ%2FnWZNYJK8pcldwz08fmAuW2goPTP%2Ffy8AgSAiBTKfS1An5IUuZB28sLW5BCeWXgxQcvLpaiLFVFRlwCiyr%2FAwgjEAAaDDYzNzQyMzE4MzgwNSIMtG1jxBe4bsgdXp9pKtwD9Gee1%2BbrHGquxEy9mTpQgIBytyz14nOc6Rcnzntj8T7qKOPL3Iqlo2kDmB4rLvUG%2BaFuYm%2BzANvhpC1Pz74oZzyOM0y%2Bj%2BqaM%2F98H4vBr0SaMmoZauJukbQEUiStY25hqHP4syGfgBm2woduCCMIY5v6%2BGK2ubqsodrSymC2RZ4bGk0GNXdtlMwDDUAYiUzTfRCoC%2F1r%2FHnugg9c6uuPVCVqKhj6p6ycJ6iHHss%2FvO1ARo8OYFw77Oy3HcZ%2FVEV5JocYxl1QP3PegJMhqtnbnymoXsc442pDtQNC6Q4i4Oh54%2Fnv0wOKCwiKaAYMmXtoXAnc8MijMT6CrZss6Pqt4j47ydkxfNFkJOuXX7nuRzyc%2FQlJYDP1oE2eVppXMI0gn3SX6M0lHPEawRrZCq2GH9QDKmFPwJDVspxXtUZOGLKzFsO2XUwWYIhx3kf%2Fs5%2BnouNIp51VE7vwdO%2FVvDXMFfYbNbdS6KMyVm9do%2FN%2Bhj27OA756hBi86AndzKepb2pLAjRmw7YinX7V5pgoYRR8jqmOdypoeufAGsJjvG4w%2BiWXKaqU2f3wOtgqF%2F8O5pSOKxQ2%2BC%2BGozD87qSpXhx2eXST%2Ba%2B4EYzF%2BD3tbEfLlUqkckNOlQvtuD767YwzujgxwY6pgFGLOpaUVXnOSjHmgXD2on5vKx9RP4JSVxa406Fux2aE3RlOl%2FGptm7xecg1QA%2FlYYEbw2QE4zV4KCqGngpuic3VDtvEK8KrO%2BmIqNlJt6zVBPEnNopjFv%2Bu2SuPIKZVa%2BI%2BonGdLfcN9Mrorhil%2B4D2IQ1HRZhmje%2FXJ3TDxiP%2B%2Bu1GB6nht3B%2F9njF97E%2Bugv6lToWvW1kT6Dub2ZSkSIkTPuUuTv&X-Amz-Signature=7561236d4bbd7f4d1534f4cc603da7737b873232f74c774f4107793f18cf7ed9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

