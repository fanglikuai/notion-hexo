---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665G7HWCV6%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHWs8f1HYi1kewMuTAcWJuyfGLJi7woLge00PqtUaRIaAiEAxVvvrRqStxc18UJmdGwGF3wyPS5vgFio2FH9AF8zNE4qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPSrDLotStp7SeV0eSrcA%2FuFwLzOCfU76jyEGIXoWQF5cYq%2FfhqxVfnbmFNsLnztf69Bu14n%2FKSyphQ0DzSax%2Bm4JcbpteVMZDdyDwlK3Adp%2FYZbFqa2VfNK3U49xf4ZP4h%2FX9Rj6qFE8dk4ri046Lt1ZZwdwUPxmWTvqgHyfuRTIFXotdxRdydzsJ4Q%2B3hCztlTU%2FaMinuB8c6qDgYwL879A9bOk5RA%2FEf69jZYSx%2Bygf7q0wt8VuZ19fyTI9MbaD9v%2Bg%2Fg%2B4B%2BqwOlOad7Q0Q65dMlziwAvVhTKhVdcit9IRDkwvrLvXyentwBakOGFG7McFKdt%2BLg2JME5cMw4hY7WoviuQw9oxOcheRZ7yDm6eKRHgiqpHmd9EC43UHL%2FWe%2FHIkc1GJGk7UHwXXwHNjSYArwB1WRzjhCKDXs0%2FJmEQ9BprbXNQcFcdpInPqX7qK6XBkGgNq9WdM8U0MlXwuJCVHTU3nh6VKhzEZwQ9bq9MMZlsMujnBAKps6ifNdZk6vJAtWvG%2Fi%2BpwIxZB5tIDPXBqOZZwjSelV1OkOggn3%2FeXx27B0t%2FIMDl%2FlW%2FexQzJIPTC4jxOqzTYGT7PX80bW15%2BNCJ1CBOCM1ABn%2BI82PlRyduvIQmsw%2BfW2U7owPCGLvnPmfYcoamsQMIzqrsgGOqUBzBe1kedSCI60ss1GvyCrYWDCHfWK0ODJEJROGQqLXwO8Bf0%2F%2BNOYd5MZZe3tZvT8YHgVO7kIWsVjNaf2nLlPOWjEgDiyEGMqKDUlUZVvwH7zRfO2mLl9fSOAEWg%2F71MbKMW69z3vCdvAMZils0UHDdQGaNnoOY6h1gnvqyoabhlOI76X3Xvkpn3Sh2pzRP94LYD%2FaIknzzZLgDQrKYRLnEMPf16P&X-Amz-Signature=4ebd537ca339066989bd3cfa612281f333a43efc165a0eba61bbfe2c43896d67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

