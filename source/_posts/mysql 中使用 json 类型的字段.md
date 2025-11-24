---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q5HKTAX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpPBmyJKvH%2FSjmTiiaXmY6Ql5ShJgKRHNf%2FCMPQtTmmwIgXhUN%2B8cG%2BkKe4WirOAPLBfE0FijupOCqfWMqd1pzr78q%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDPerhspGwOlkgIP80SrcA9THSYyUbr3ygsvnkoffblp3AzXS86vBiBXdDamTU0nIsbAC2EEglJlfjD6kAWjhW49HeZ7Wd2D2iroN1iUbGlks4awfDCoFDX4ckjHmnHSWyBG2TkynsnLMqLGodYoMvHYSWYFgyZ%2FHXEiPCxuqEgA62xBhZQ%2Frq8DWxHKM2Ml%2FhjNRzwmpkDDk9trKUu4NryUz%2F3Tf%2Bv4WvF5YOGVhOc6n6RMgoW%2FAUA5cmuwj22m2XHdyIH5fPPHAuz17dgooT44h1Xc8Vvy%2BkDxxOJqBpC99ZUGk6BjdXpiPE%2FB8PmVrZ6uw2qbIRQbVKtLYuXHLcYSKLgZdq8%2FAMkH5%2Bv2EvcEabv3uFU485IRdCLHTvsoF%2BNGvIgeIJ%2B7IOs%2BqbPD93vfW0LX2IxXz0uKwvzr1nhBTFtpPy5an8J4OYP2irnXIGEqkWTuO3HnIYY5JGgA9nIlcK6NVUQSUHLjmSaKwCLIup3alUsetj77nJT1r3u8nIgFHu48OCUaCsav3DrR5PVVZDgN%2FPmiwkYlCpwTVdNOIt%2FsYwXykegQI%2BJroS7aIQBTXujXsMg8%2B3HCsDGJvSmRDao4%2Bqs6Gd30ARy5EC7DAdpc2O2jmLORdwFo0zqFiTAq1wqfZZCr4EyPLMISZkskGOqUBHZBiBM48qEczWN%2Fol47EmpJiSD0KvzEd53sVySOKJM51CHkegT%2BfV24pgAFrYQhiwwrZ27gumeV9KpNvFn%2Bg2J%2BsA34aBx9q0JFZNsxGifCTZTWs4PK0pUxxcW8rmvlqsgzXEQFPjlfYaZMN%2FetPcSyfCDlBRY03E1JevVCbTPAhaMFIpwzsFwuFHkuRCOn%2B41LBTeGA9xxn%2FHcpQh6qSLJZM90r&X-Amz-Signature=539e99773cafd1e2bc7620e8aa3236155d329433ab0c6b648509fdae5311a0f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

