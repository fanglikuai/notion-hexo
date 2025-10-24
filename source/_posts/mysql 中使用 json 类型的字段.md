---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646BDXTEV%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrFoB%2FmefvkiFK%2BKwlAM%2FHIfmhiitLSFj%2F7eHX97rrHQIhAK2%2BW%2FZuzeQfdREhtMmOEZrUWVJphX4ooS6Mqs3Z%2FDO4Kv8DCFQQABoMNjM3NDIzMTgzODA1Igx%2F2ju2lb8H7Lg0IZ0q3ANB9GuZRfe%2FskRIQuuWdeiVqxWFtrrLXMDfTMnD6odPIh5QscMB%2FruNEcqYuLARmVOIOckCPIY2Ftwr6aG2qCODNNo7%2FDjF%2B2kU8IlcdvlYSh2MkjQhw7Oxv9TUa2%2BWu8RRBw9MAlmMwVcN18Q5XIuU8Y%2FUCCzDeF4eOabkPykxg3wYyu2LYnSuc7QvZWsgIiT%2BSK%2BzlE2LcdYI28RSFQR8lnZFxhqxpq6hc2TScugiy8mE2J6vHLvAhANbDgcRCegb%2FkudT%2FZsTJSGipncxi3AreeoLSE%2Bqu72zdgxBjkl7%2FOp88iLAEzMW256DPICA27rh36pqCC7HOwCB5JxRzJCHyzFLgvnMBAYbxliZJac2ivtxCIlK0A3m92PMMYwc7c7QqjurB0HoPMvV9sz2k362voQfa1mQm76V2BuZLudpw66G%2FbIzB3l%2BNfEPCS%2FUQ%2Fb3IP9BtV5VlsAaDIVcjZCP%2BzvaLDj%2BA095jljva05O8Je8dnGoZWk2yiuep9jP3GRVFoccZOzbUoHrxF4Sl%2F4JgmALBP8FX%2B4lFZQlyRrZgtyPHkqgMoF905GxldmexZMzAK5PQprGvxBnd4YcEFEzIgrgjK2CBUO8oFbIncl0TFxuC7g7CGjrQMGBzDmy%2BvHBjqkATTxL4y6hD%2FK3FCz8BBqKiga1XfLeYZjGC39dJ5khdS%2FqJFsB1%2BJXRX%2BNbFbjABOHH1diQxRDvOOLc01F8cguy5GD8mdxUczz7DYWEVntYMtCdCBXeLZsgMCUTcruxM6ojI9sMyPNB%2BDujMKISuEu59HibI1Am02ueF51uApnj5%2FY%2FZxMGoGmI5j%2FuXiR8wF0%2B195AQ6AulmrSkl4%2FC28Ykus37a&X-Amz-Signature=814ba58a5cd0097921e06a07ea0714c09f642b1784b27b64b62fe056803af46f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

