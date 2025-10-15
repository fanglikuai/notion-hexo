---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM3N5KH3%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T140106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGzfGGdDa5hCLC4RdRi5MXwDlnV32QNEDPVddrYDe4RAIgTg3%2FiQ1D%2BHvZa6kw4MTvvwq%2FTXqosoy8fW7De1kRwlUq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDP036WER%2FXVV9qEEiyrcAw%2BYkzaaMkPTXfuF%2FftogJlu4uetnk%2FVYpWmILr5r79qJg0J5dUlL4yIJdSeb%2FotZN6RcIof%2BFvJp%2B6elUKkaZ3FA%2BTDG%2BB6DnFWpV8bvuAmQQreYQTjxie1ZUvq7%2Bgk8%2BqTOgHI%2Frqa0nD%2B6omBmAGGIKobiow4jlih7sl9ZjjozTEjO1FWO5uVAcwkVr0p4MvTfa2IpJEZ6gq0SeHDNwGbZW2G8IsWIV1l%2FoqYeJTz79P5QKguRIHHAkeyKXKG4t3E8%2FNaJF5jvSFfMOA95cCd2CrRObuFXG76bE0vHKcWuNtMJFYPyrXZ1n3o27bznNPc5tnA0u71dmA6FfDSDuxZ8OhQ%2FI%2FzYDv%2B%2FWFMN1At5JObwlNoa0GuFFLEmaXMgAqFk06FQSORbVqj%2BbD5LDW2J3EU1SEz5E6inKDPMKnfZH0IFTqpzPKH2lBAtIwRLIB8e5WKOxFyFDK7i22nmUCMev75NEegs6wWyW%2FaEeslDpAjqWadvapQPmULOgRKyR10o%2B6BKqYHz79AoHpT%2B7A8d5b4fPkqVk%2FpehQUB%2FPVpYu6JGKOhJ4RyDz3TR6woT2E5TNz5douxcZ6%2BxIsmlfsShfvRn3%2F8kb2Nqv8uGaMr%2BlXUrLbtbo%2F5gQeMLe7vscGOqUBrHvo1%2Bh64%2FxRVjtOa%2BrtYdiRIdB2SQ8c2sP6DSmyuCprr7sviISSGMRBq7dQ48AXGWBbrg6Io1pPzvEbgqiguuIZzd8enYHoO9oC4zxFW245R%2Bvxf8qNHBBeOB81xlpIuFgfp7XM%2FlEWRVFxPIgxBtnfj0PKKPwMipBU%2F4x2tvjdw05gDPPD5dqWAeu8%2Fi4S4wq2U%2B2wlYNug5rFzvItHN2JjDOQ&X-Amz-Signature=621c5bc828134759dbadb22043b514dca3302219b5c26bf571f94488ce106eb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

