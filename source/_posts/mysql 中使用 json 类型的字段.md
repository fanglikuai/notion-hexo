---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662C4EVL6T%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGtRgF%2FtYN2%2BLAN2JwDPT9xal1yjIxLs5Vqw50gDcCAAIgSwqXagQLzvs3xPBL%2BHhWPl0N5ZObrlSFmjrxeoye2EAqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNRTty%2FrJj3s9%2B2CCSrcA8HgIToeojlE8ZBS9kA%2FBggsEQ8Vq3D1NzzCfpLhH0mQoe3bJEeaUpqRbhMQDDQ6vk2COYCt3iLxr5PYYM4FchvAVcaLMEP12ZnoO%2FGPsJukcd6Fa5Ld1ItkF7%2F2YTk5bkNmEXpat0fs6gfKEfwDAwaJ%2BE3tkK8nwSe%2FTsCN%2BFomS36sfHuXaNYRh8blBDH6yPv6Lk%2FHbfixSwHcPxpdJPBUysnggfwj%2BoXyViShi9e1sfiwgiE7Z5ZUyFw8OpDxmAEQvQosxo5nTyRSljAq4SV%2BMSwpnjUt8%2BSREpI%2FN0y3D6c1vIirwLemBfUzWHFyVU9VI%2BgRNpGSaTBmN5hgqJLI6ALUJ2RLJ2A5XHSxBSWNIgB965KdPJc5MIw%2FAwB9RvLu%2BtDqeKKntJZNV8viEroFf6ifUuSqesPcBQS8w56OJOdLoYADhhUuxtLgUQYGciIv1zilWuqO7%2BDtvbnp6VW%2BvsPy04zgFQ2eX2dDeylVVZoSay9GvMk6Oj3Jl95%2BJ9m59RmRO6%2FrhRqUTrE8bNqTchaO0TZdUTYVkPAQ673WqLLzs%2B1jYVsWtt40PnCZWEPU%2FORNr5%2BcD%2FhZkzzIJdzdi%2B%2F4OqX5d6rn2h56dwgcjWOp5wdlH9a%2BG%2BVoMKK7gcgGOqUBH9R3Jv6gzKXChmBM2URU4htEs9NabdNts3OZdWZ2m2qpn6vkyoghXuGRzPYG%2FQB8hFbZZRPWPaqutLf1C9Pga6%2BkgdRZLxnQUchqZaLX49fnt43P3zAT3UsK3Ma7FuIfmhqt07jqfe6WZv4nlzEriE1A5HavBeHxoQ%2FbMOdS%2BKa358LVlwAv7qMKZVDRPOQL%2F2yqdxUEJmQ2dRMx%2Fh8Cqzs7WjH%2B&X-Amz-Signature=0231a2a8de79fe0c69a652d772ab72830c10c3767827db936e2afddb6de18809&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

