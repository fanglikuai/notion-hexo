---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BFEQ2PQ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIB37WkeYaHPBPUhWQnBxTDMdwA%2BSoxO1w6dxaU4LQ3vAAiEA9%2BrPWNFNbVTLNjZtbQUv6xtxsmVQwSVMrcVgYIe%2FLskq%2FwMIKBAAGgw2Mzc0MjMxODM4MDUiDEUrnEFEcWozRHEqgSrcAwdzKYAKgbR5wl2smgzf5FguRrj%2BlATXO3f8fak4aztmk0eVONgZiJgC2Ux7MzfFd%2FpMX%2BYEFp%2BtH9OJyU8IiTX%2BTmJIkkhh4hARwU%2F36tEs2drPNaqyAPqwGvAf3f2W%2B9yg0LKLuQYlDTJEPigW6hagRV%2B0JzMFLPk24T5h5cNx%2FAwg2yqKTcCNRw6XY7KjpwCIv7xAxLgpSEDhaYqUPDeVT0%2F8Uotn%2BeOupgT%2BMj%2FbPlDtB%2BUqS6iKigHg07vUD%2BHlAYWrCAvjMJpdCPl%2BY9UFE8UBQ4HQOSCUsfg3bHdO6P4St3dhwMdCMGvh1Fnz3i0jcAZtmTlpOJyAbAVccNP5bOJGAIMJsqzn74zTktPWMUwA2gdc3E0mI7nK5QCM%2BNm2YfT1etYTawYiUa52SsQfXvr5J%2BDIEVcTeDWu5SHoB78xSbm4ZI922s0oiFOoYLkq%2B5Omh%2FlWtQLPXO5%2B4eJbmkz0595XHAu5BPAkwrx%2BDcZ%2BihrFwsw%2BHpiiGAZuv6xnyJmDdQ49RK4tTlMllMYKU7S8mbuu8BPTWLMqzwGCiOMHYHCiYFbwWgQXafmWXA9OQI%2Frlz53TVq7DCjA%2B7Wy%2FQ57xLvDtEoVjWwo1PRCtiDCJGjws6JS6DurMJ3UlsgGOqUBTBszKC64kBL%2BgLWyR%2BoUtu%2Fm%2BxEZkUAeuXPNEfLsYZ8JysfyNOgXGQyPvwm0AcOMGSXydFl0MxuCNsCUCjQRgJ2FEx5REFq9oe%2B%2FPBWMGhLFuBJ3nwpDPxGrY65tXeqpH6fsI3jBBjK%2BfjtAlNUhbgynpxtcvFLi7uTXd1Bv5MvmJ5T6VqKd%2F2uhBZVAc0FX%2FK5DYGN0Bef6nxxLDGCLMQDyT8%2F%2B&X-Amz-Signature=4264e7122248e0dc58e6f187786f189ee2089e9f3807091a61338b2c15e5bb0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

