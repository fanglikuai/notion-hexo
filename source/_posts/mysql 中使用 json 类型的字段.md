---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLBPEIEE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQCaYY9dy4ti9WPXGqV6n1sNM2QOfrTJgip6glo2o0Ag5AIgGkPRbLxyo4Kr7wXIOP2mFttY9ANlbknzIFCyH%2BVTZe0q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDKNwsPhrNPHiL7GcVSrcA5UTJVw7lkJTdXfoChhApMJA4lP73dDPK5td1qn%2FokFhQyYca3Sb5Z755BxdM5ONXRzaIp%2B7qC4%2BZqvBwoP8qlDk52HfB%2FZ%2B9AFn77Y8R%2FI4D3bVUmJj%2BrgRfjx7oNOVBcJoOnusOUhrG3I6nxefZT0AJ70I1EzgLKHAPyziRSlSLdIEyrfGD3j%2Fc6fA8PnyPxqJQCode%2FgrVpfYy9Yde%2F5vQ7TUanzOTXC%2BzHVIX3YnGR9nvCBSlekHv0ISFuPzkseaDc4Oz8P9qPEYX2SmHraVC5KfoJAzunPucgkDgylA%2Fe%2FWno2WhrAes0pBq5Tt%2F20pZQbRY6JFYguXl6J1brgN%2BYy%2FprUrERlutqPCbsOIr2XZKtWa8EoiZ18NL9ftmy2ieiAyz5hHl1Bmpwri7J6u2%2Bsca3b5tfvO8avUs%2BwKFT1j%2BZIhjhW5hqXbArBD1kmEL%2FGv0OnzzKWlR9NxBToupZv3Ks8VGKWbhBSen40OmitkzBhhADrOQJiHxQGXwjUBEmON8oXXWeYnZH5PLD2Q8sPcyceaZWHp59DQWB5Qy%2FnAtXSTolTiRqhyKB2SJjq93gYch98UX4Q5kPrjm1K2QxS7X6od%2BWvJKzMROsZeqi%2BkA3I5yqvzOCdTMK3Z3ccGOqUBQm7OxcAZh1J8opv8BE6Zm0ged%2BJic9md8hphV0%2BZPKNdbaIGKbOZwodArify45guS5o7pCO0GPmneLwciwnagyRxeLO7M9z6VAnLkpjcbJmkqEiRazUk0F7hUwXxTgikoZjLLw3G0tJK0vRGaacYjkcSW1Z8L%2BfiAx%2F5f6%2B6fJK0ekiA3LZSgY%2Fgn60KlwipGAo92qKG%2B7iH%2FKt53DWR8QZTjaQ1&X-Amz-Signature=ef37ca639398fc337de234c99349cb57afa6bc5332fe53e9370ef63abfd402d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

