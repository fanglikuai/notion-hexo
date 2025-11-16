---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHCSDPPW%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICIRp5wG9cYlJir1nG6phZDE7H1FdyxD9n8Xuxfg91trAiEA0Yp6bIUKou4wkV7Y88H3keR9g1N1nwadGb2RCkJSmysqiAQImf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPjw2sQHGlG%2FX%2B1M4CrcAz8HM%2F0HSDRi9ygN7jRaHw%2FCDbiIjZP3ATNvtKupqoaS2rYxAK5%2FS5e%2FGq9T021ks9aYrtd2ZFc3XyvhyY%2FpUkqkztFUD79TYUhJ1TFxHVS9ERAuOKn9DhOr6%2BVO3z0rhPvlZnvAVEezzjCxnldxIgTi0yWTAP2b%2FhIaPnbwbAQE1CMG6Y%2FRYDa0%2B3ZsFY0y1s%2FQAz5P23GrtqiHNPfE5S%2BCEQlhH%2BEhTAAec5xovIFpyO0%2FpDYzhopki4KOM28qSUWHpDChwDG7jEK%2FsIjmfyWRGS7s%2BbNXC%2Fhhyfe0rC6n22ReMZJtDrc6rmgFk%2FXieew0WETJtM1y556g1PRlqdVZb0jKM3sOJfeyEq2PLN24Rei6iW5nZdrC4dVq7dOtsDFX78wWDhmOf%2BN64cLRiGXIf6zKS7KUo%2F9Nsc6Lhde2zPB6dsuI62gUy3DITiF7y5eytzwO4forBVVoasS6KPDqkF0TtyulDhxQH6kUJMthEwnWNAVgAnRcwqW0EQ%2BQCR3ATiKaeZRDs%2Be79aWxOATVOtchNJRu3vblTGSuXk%2Bq7ndfB75YFHwQlCXQHX4IccaQwcVRWUXMNHdPE99cyJEpsgkc44XJSxSdkP%2FGiAQD5tew6KEcbGL%2Fen6RMNXe58gGOqUBA%2FFpLeX6Cq9cxCZ2PxvyJFDpXVSCaGxCAPskS5G1qLTqBrj3Uco87pabQkhaGgLV5jV8TpQYcLNTes3s%2Bsi5p129n%2Fc02hpMnmQHhTKInUg9Iv9G%2Fg4L%2BBOzyb3hzfdr4zOY8eIAlHVWDCZkwyV0pmziDjVgoWBUHUGsnwDzKwqVSt%2BPm8IF00zejb%2FCb8Ul6zdEWhY8J2HwSFPodSHoIreZ7Ydf&X-Amz-Signature=05fc42db0b3ec8d4780380cdd08d8241a4f46ee6cd90d645142bdd67ec2fdc4c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

