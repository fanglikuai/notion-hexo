---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667P7VTRME%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDs%2BTVOZ0Gl6HG5IZYZptjBYkcs1a20q7E4xA3iBViS6QIgLfpOlM3pWFVXMXVHFuCBH85KWyhRBp9a3QMQ58QlKfAq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDEBP4OCnt6qYYY0MgyrcAznzrthM3UHfk3khL6qrdJODhvMvXDGMgkw1BAATWDm2huxauDo6%2BnlpZXiAq174MDeNap6s2Pmf6Eg1ziMwH5FIUH1sdH3hjkGzUXbgnhPxFWMIvB50uSI7Gtu1stLJShalDyKfgllxi3shOIp7KTLTwbX42TaJNGy6gKjJzkeNwK80k29JmdVfzj%2F1d5echw1gdtxNum7FsvQlT3PVHvtPKHfHvIpZbFGkIiA2ogskRjUENrhz8FXApshtOG%2BeOps8szwRaMFTh%2FEN5lcGCwEniQXasiEri1%2FsTw09tT8ihf8qe%2FDyxt8MzFy376LN8cMKPay68YI9TAzZxR%2FVGkNcCTQfK3zCJhbAgTmUunvSeDhlYkLOsZ1l39Hd63Bv%2BLPaS%2Barl08HODBdQAgqyVj0M0wBLswlCDNhiQ2dC%2FQ6H7DqY5yfn1A3p8fvy5jOjxFV0lY2R2WBacvtmdz6%2FrTDY08IP%2FOpeDcWLwcPB%2FgkTv3XBev7qr7KCJaCM5Wz4x41cD4C5qDdjetrIYiOIXE7WcDAK19m87fU5a%2Bj1sg8xLRW5mqP5TCpowbx5%2Bbs86295Ue1s8ynhqR%2FVGWhRgX4hXvZQ2812cBT2odXWfs2RvwSxQTERpqgY48uMJjY1sYGOqUBcCNJF6VSHix3SXFAHZnPaWd6Z8xvA%2B%2BmD37SStXea3SkUdem9QPvqsTDqSh%2Bz9nKWUmbMRMT%2FWVEp3qOafHUOfJ2znYagnf4jxmX0eYw27AKh8eINg12GZxaMSzLIa4g1tr2gRxirQCRo7YqEUMNGn2uUS1ZXOeyBRzgwmgswh5HTvAAJH0OeW%2FjaSXpIk4AHjWAXm%2FCNtcMjdg3H3in6KdEBqzK&X-Amz-Signature=c16ded9b547e7288a953b3e4749ce3fd851ace75e8d432b1a1f60314e69ac719&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

