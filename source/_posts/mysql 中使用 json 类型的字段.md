---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SZLWJDQ%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBZZas9VgP%2Bkdcn2kg7vpUYNGPxZMuU8oOcXP%2FbQESI7AiEA%2BaE3RSN6QcHWmapQh1AKJ3TM69Vth7WhSR4DYnRLD3oq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDLQalkTrocyCpon%2B2CrcA9WeT9ZBt4gJS%2FPVby3hTXdxGUgCOYedhdmprdeIIHCzvBb2UBLWcHYvhEzkg8m%2F%2Bjci569sCFYXm%2BUK9GPU4fL3NaJq%2Fz2fb33Ed0l%2BW%2BHDryAIKv8pEQAdLcMWzaGRnLvra0VsEf%2BF73t%2FA%2FHH3Zr5Ke8e2n6elzgBQZYAj4Yzvum%2B5tcu%2Fu8HaxUd5uH6d6Z1Eztt73J3SwTGyVLbkHA5LYoV3thslOZBqVDBV%2BwaI2xhgGZDahUFFbnXtsbBrqAI88GbusZqsNNwo87Xtz5ZdhmKWfsHLCZYRV3e1Sv2UCkdxxHidunlJzQ5G1TWjMD2QCxSjUE%2FAZTNKZRttmXM3rBciHroYr6wRjxFOTjFDbWl7qEIM1VGDAw80Pht6PLx0wZE2%2FynwhqrIFjr7g7iOptVVMwdnUADoy3sosDNE3kGcGF3t8K2ht7cYISIoReslfeD9xS6%2B17b2uV1oCwRddkn200tgyDrhP303szComad9yZakzPbaSla1rxzMXzj3YwiMhP4iGOZmiKMNjMrxsHPtdGWqeIMwlWdkZ1f9zgbGCyIwrx9wLYM7D%2FlSx6MsEMCZOfe3uucpmWN9UgN1ufAM%2FCk7Qs8CIAC9TXKhdoWKETpONp7Ab7mMMmElskGOqUBJewfJ3GwfvBBeIdyiGnZdlFCges14x7muDcvl3uu3l3a2WLoSB%2Ft%2FjCPAkRVrT8hvIIhZ0x1%2Bi6IA15KL%2BrDkDZfuNoPJBpaA7XcSc3%2B%2FlITmy05ujasa3hAP3%2F8d8CTh8fFDcjQTeXKGl5VGhz5KsQY40WyCKgH1kWiM2SF2V4%2BE2Yr7cGfN6ENgJdzA3YPNqg3M5rXSSWzJDAghHO1ay7sWkGq&X-Amz-Signature=932ea27eb58d9fc356b2754d5f644d92557d6be48a99cb78b047f6a1b19d0c8e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

