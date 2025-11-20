---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYTDKBKZ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJGMEQCIBmi7QA5XgSUYNSVhPCPWChmLZp%2FvEzbsArvZvoQRbn4AiAojstQ9sx4kCXR%2BuAKQtgs%2FeRzrncTdGyabv08wsPkcSqIBAj0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEWj6B0ZdbABkeA%2FRKtwDLTK%2FfBLIu4Fbm3zcQsOwSh57nDzzD61zHajMI%2FjxUre00Jk2TGWcL5sOOqbTU4Vxm66vV%2Fu7JFqN%2FKUsJyagvct1PzWggfjEP2xtNaAO3ovO22mGH7M%2FAf%2B8Y5gbQom7OqAYGp5Xw9kwFhnOWwYG1yUMztoWPjYWG92yI5ub%2FS8anS3xA6MrVS9iJ2EXb2KEs2l%2Bs4xBdyct3srzBUDWKFY8erczFCLz6cJ2z62yB%2FSL4ZpKWXgAnmTZ9%2FdXZS7vHMQ35oogitbovyQz35lct28Y5QRViMxOVH8lVpzFVdNLTNg8rGv2%2FBbLUwcN6pEMiJGi%2FzWmB5mpAsEMVYD%2BNzD%2BdmFl2etFso5ecgXZsAhm034z3QgQ53jUP4xS1z25kDCa4C8Bpd%2BIrTSMzwT4%2BEN3QFxuvv4Y0Hfb02BcJ9LQIj0n7gJ4zZQmyxOMAJU3eNmqxeGBnmWVi4wpdp%2FJVgt36Z6FI6kHnJv%2BAflenIk7Qb7rYbAMShoiUHy4AZmuYrT1R%2BcwWXO%2FaWxT9Qudl48tV2gptwgzbhYDQSrgJeuAIlKQI71LOzaxmlocHqEiSCfqbEmPjneX8YW%2BT5SJCjXvKtNcEqYo%2BvOzE4z%2B%2FS%2Bm1SkHG%2ByKbf90%2BIEw9O37yAY6pgFeudeAebIN1I15ZnQgTK1usKguaT94zpPrTwyWrQVG9A5vuqwxN%2FcGe14eG5zbWrNJ2YbmxAwypuVun50ffUdqIFnrF7x%2BJCz39nk1fUyiCeiHqMKM0aqKp7kD%2BfJcHNUYv%2Be8MDlKKOB6NqaAE%2FEV6fTn%2FlgwTGFh6e%2BOTU6ihZihOc5cd7ZRLoJGYUDMWxhf%2BAYf8vSmz%2BJVo1WAjGFqeuGIbe2F&X-Amz-Signature=f02d0357abcace160e29fd2ac6b60a1546af1b5d9cb75ab0733047a78b22ed4d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

