---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY2ECVE2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T170058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDAsENdkW1uJIuGoqLdw8w%2Bypt7CSCroYFqhCmpDe5taAIhAJ%2FE5GrCRGvMrCh2ZVADa0zSpldjCo8mfSNb6wcdo6N1KogECID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2BZi2FOtzfae8Bhl0q3AMcHGS%2BsXyKmxEvns3Lk6f%2BEJDAPbWZ1QRgMJ%2BOTd1fkejGF4K6O7c4syOV%2B8QJCw9cjKis6d7b5PzcJUrkK3mDVr05xYPZmYKehUKPqetzrw7cITAl9MIpyPBJshRK5qdWf6r71RWZH1LSO41K1rHWJtZcu5yCQkAHdjeb%2B%2B3xZfefGeQYoNl%2Bb2aHM59bBiFjdcteFsqioDQtpqdqoSLw3Z3BI8dl8HlgHuoGERgAthwMNpdqnx0pBmZnUldefzuwJly05nJ%2FOVQwzCwCV6Xlu3mPL1Gqvw2922eWB%2Fa8wUAteYE6pzeVtZOnDiSlH7DgPI98JMUmhMKn%2FLDzjZhba75aXhxjq0yFE819lQOia1Uk2NITM9%2BaO4lei%2Bx96sM1V6yLFoXBVYfPWgubbisAF0HSKBfldcInO1%2Fsggw6FLgQQ8Y56%2BgbUwdBDQANm6A1Mn%2FYeYuanPZA%2F2nPRbIlt%2FgiJSS4aedSfh5Vabt81jf%2BYn3Uqy5%2F%2BqRuL%2BIput3QUr2Or8ZvBs3VU4eV1CJ%2FTKoPS47W1dsoYmMJ5%2FXZb2Hnmr%2FYsGRh5k1nKGno75pLpTZky2xlggQX5ep40Hr4iVxtK19RkSPZLxdoiNCQ351Nrq8uFw9ggQEC4DDvn%2BLIBjqkAR47FanKyIIsCD9xXclv6Vp%2FzLECifw3VlGI9xMQyyjtx8SsLj2VrKg6tdJ3wEz2a3zF%2FNVuafoTR0f12x%2BtvU1IDJ8TbZkCsmeH18yOwHinzefgcC6um7bmTgmXKKQvtcp1N1ZUXz6CPhF5yyiBg6pt22PCDIalZHzufC9HZlxHH3%2BEidmAjz5eXoTctgwKawrq1QImAUtxkZOLxZ25SIxogSds&X-Amz-Signature=bd226e62c58fda34e516becedacb890a6ac1365a37340710fce1592eb0108c74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

