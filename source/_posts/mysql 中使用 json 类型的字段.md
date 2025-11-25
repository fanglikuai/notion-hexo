---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662B76NLCW%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIExxAxg9RaqEmoWt1OBpXywNUcbid9a%2FeHfbj0BU%2FvaLAiBQRCTNvSpmGUBzdsoYsBF8QuAKXnwA8JNYWplKlkjmHSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMX0dWJFVdCUQ%2FZA35KtwD0brZczQ3cn90yFlETucoPyguHJW%2BqeuDKABIJ4WXiqtrCltXbHvi5ireHv3LvivCLj%2FmKTtwulyujs514buz77snwU9WX8fd%2F9CqttwNtwJ2CuqlNO2ml4VFULl4YIvNHYkiLbobpTPMCRr3qEN3uy3W%2BvKjkcxVyWNGIQ%2B%2B%2FsEAn1ISatNNXjW9YO1pOJxb2H0xmxCni5dURtLv4ga7fnxUcXkEa6FOuteiQ8uTC8cEzbJObOu7CxaMjW%2BcRmqKacUBTOvFJpUWKxFSOn7%2FDMI27%2FykT%2Fz2RZd%2B3a57585hihvBvxqc1r0RyuqLEUcYJVMtg9e1YKV4yeumLLKnxLtL7XJnw2nnxWGb%2BCdj8Js2AkidamPmDh3%2FrmOIPVjduffGgPCDGK556V1gCNcKtbHdJZF7U3MtWgBRVZuXEzTLr4vkFpkuLMkobF%2BBA4y5P9fiQ7s1Y43vPuAPqPEJcDuINKL8FQmta%2FjaPUK83Fhh2rHqir5eBqlVqOyFvMk1AB4SIXlzs0J5%2B70Gs4FpgUkE7%2F7Lq3BTn1gnJ%2F5o8mL6m7zIpOVlKFoPVh%2FgtLYiKPzN26xmFQt1tjha%2FrxdzMmWE8Q8%2Fqswf9LBqOr7bl9w9BAW8%2FnVqQj7tfsw9daTyQY6pgFLDFreuYSSBupbEXjA%2B%2Fovlt%2FLvS%2FxvUiWCN01D5UGQK4bgNucpQLh8nQju802cmkQOfqDGKBEjCrmcAc%2FVlno6vYxlgcVWxS58KyaCpRgdZ29%2FBZYjF1u8EeiF8dOMdA3ZTs96FY08D4mqcfTZvQeYlZ3veVL6gA2Dyfen3CrSIrt%2FMN50CpR2w7xKbg2vbjEC8fHF1ggUF5z0%2Ft88b8uPM1ctqc%2F&X-Amz-Signature=ea24627b50dcb78db5e81e8d4f95fdd62c35709a7a030400f3884bdd0cc5d489&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

