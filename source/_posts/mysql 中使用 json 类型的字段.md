---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVYQZ57V%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIQCEEctbmWQw2DmiRZKGHjR%2BxUAvZZCosR60oHWFrTylSgIgDlp24wWlGDy8X0kaVvZIqPBdfU9GbN1DFKRpiZS3TWYqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNAQHdpU5EpWooQJaircA%2BeKC9lK36CJ2NnDqf%2BquppdmqFnbshpLp%2BZKCDXRYXHGe7tOY4CuJ6iG82GxnqIFdjHGJ%2B6h9sz62IKGKewIrK2Fp37ApM7NRu2iS1XIGC0cN%2BXvOI6vg6HMKLBr3G7sZloUn7yBQ2xtzkdIdsJaemEXA%2BE51u62k13XpHZ7ODFNE%2Bsc0YlkhW4CqkyLqCUpJYOwBOzWEvMmXYqnwfJxD4WyiGV979La6lHfcJY3UAMN0%2FfIfxgWPZgHlJqKvk7U0yf3YWEZkOn9KQM9ZQqZVc%2BzovVdsKus6LZXB3k1bL73n%2BQ2RE%2BPvUFuLgyKA79Yu0vB3CZqie0ZAs48FCSA%2BEPLDLfmzdak7WmY%2FVve8%2FqCOMo9%2BqGGjy1AqSoGU2RCGldweZ6o3O9bcoxkKzdgGMon8gnic9zMBK356atGlLYYmCK%2B9HQdJduJF3%2FA%2Bw32t%2BrfqExU46uQje8uKFGq6LjrAa5YwBheFVP8wJG2t5cY74PgCUIuZjnAgQdCejjN4ndRTIUOfbziYrc1eaUnYysRn54ZuKuYvFaBLXJL%2FOdGtFnItO3Tl2caf146CNq%2BtfKrRb5Mko65DhuBDV5JGLFX41P38KhE081OVEOt%2Bt%2FfN5m5H6TjK45Vmd7MIyyqsYGOqUBw26wc2PYEs%2Fjlnftazoz3jugC1j2JpQQphEIyBLbR6dPh4RZ%2BHWCm7S%2Fd5f%2BlnIaUOhtJvWwQ%2FKN1XF8H54jROU5Ay%2BNEuAkwJWdG5P3Tbf5EUY53v8fLCGBYbZSp56UzWluN3sPROOHYjys8a4c4lT%2F8UCpTDV0JNjvf8m8x1mpBLJIh6%2FPPz3goAnqkc4J%2FYnT%2FHa1Y2qwRuVmSaVBc8QYBHBN&X-Amz-Signature=a1a6f930cfc155af12aadfc8910ab8c12927a8c4316a2f5d06bc8c049a24a177&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

