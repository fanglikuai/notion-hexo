---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUHKCC7L%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvX%2BXPreYLbTcVMZbZ90XiTF32JMtV%2BAjggmGO6AE83AIgOM9l9jCDXFcPaBxaHkqUDrK%2Bj1LfIe3Q5VjPJQjRAtUqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO4IfN1PzeCMFPQM0CrcAzUOghLBdZiWwBnBpyp8368LAcsEjhSafXtsjYbIqWk0swly79U%2F2b2UPCMOvoQex1doEKfCoHiNt0JDjeVKx%2F%2FkzbMqC0Rw8IqfTgNZ8M9rXT0oCz0B%2BcR8MoOc2%2FWIcA4VhgNQFoghKutDu31qC34HbHdvtK%2Fvrr8PkgRPdNSjQhYgmVu7e3hSOFNA2WDUk9vNZfdxRaWiqgHmEHuuDMBpy9j6%2FaVv1htkxDkAnOKOgORnaVyDa7aRyUvWPamvMmy8kwExwqfCqbrCJRBeLUW1p4dO4JlW4tK%2Bv3s%2FGK0VnnVIKSPI78nW9QzdTp%2F%2FatUUEQhikN6rnGiG258KxM0WlfZHZ2abMjM2GbLvn4CUC4q7FknT3yWIrh8SG4eLdsvGiJ0TTG%2FchBHrUpMX%2BB28O1lLWsJatEmZ7F%2FuwxjbtjEeDnzgzZ2IMDFZItD8N27bi%2FzFxWyHTo9%2F0HMpf1Rme3hr2nNvsplqH6R%2BJ9uPT%2FEfSQ46MFUm5trJNd3TGkcNEf5cJ%2FoRmbJVMB2Q2psbxZjvIx7CGMYj2HdMO6kr50URLUmVjNeaF3v%2Bmvk%2BGpG7D3Rp8V7q%2Bz4ptJDLHqywpb7%2Fgu7UGWq5MWPcaqZ6PaWYWF5fsu%2FpO5lcMKe6sMgGOqUBOJsN19%2FNn1BXG%2BxCoinvWd%2F%2FjKPNzBbBXfNlnnC%2FpJGKc6NNRVW9aECnEXjmCl5U33VXwM56azk%2FXhB1%2BOYWLmNpdwbNVdoyjaZOZ62GYX9Cc8ARQ%2BHMS2tUi4YclB9tAH6JzlRbd3zomXGwB%2BTMWRQJG4RdlFMZluOzIW5RPxrlyL8%2BSBaeBhHZ9KJAqkUvjnMiyj7Tqn%2FyVjV%2FwBsSKdEVnLsz&X-Amz-Signature=4e79b194c0e2df430ddc2d6ad08660a0b821166abd3674c44e300c7394788b99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

