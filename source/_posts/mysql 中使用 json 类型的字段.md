---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSHJ64KK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T120058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIQD%2F6RRxC4Sv2sMFkkcWhFw0527APCJsb6eDIqyeIu2YFQIgLEUBeGHeqVqTO5BbABiFtfJjRXnnJ%2F0%2BwPd8yvIww2Qq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDL%2B7rNz8wD83e%2FHyWSrcA3drm3IrYGW%2BrkEcHh1IBLjn3q%2FU1CCOVSEVaiQAiFusIqiNqJknDRem%2FH4XRPcz41JuI%2BqHsJwOa6HHT%2BG7XtZYau2udOlRec4PTYY%2BQndXwci6Hj7sl4HzC8ETpy1b3RArP5TK0cHHrkTHST5%2F9PVXVwSSWgl5crfCOsuSGmW1qf0ktmt052htMCJE1wDkBPVsQWIH0U4hJ9BFXRcPBnS2V5BieB6USPP1jM0vxsL6KAxp%2B1Mfdqw1GRkt8Rdt4yxzI11n7KKgYC7pTm9izo3JRYtroRkUbzbnbX5buD6Yu42VtKWVeYn1o7FlVht2SwRVmOlPsZoNmNwBJNBHeE%2F%2BPRR1MFRXmGCQTc6TwdwDORa0w181YMQkklxizgLej%2BuTaIhp9aq5yQqudiJ69GFfhF8fP9%2FG9f2ZBTAI%2BEmXLDqcgan8ZRA2%2BtWeAP7qE6DNwPwZ1bHUywlI0vI3hovk5Di639FGTGFtXG9Bgu29xHSJAan%2BTZGSjvfbqSbkY220T%2FNkoaR0%2BiwsZzsQwAfJlOG0CrYi5AAuld0fdU0Um74Pti0igBO73vzz8MJbfoIT8u6IOSyDxRxpXFSNlhvjotSOG0WwZFgJEFzL19Uhwyli1C1FViRkDBtrMIrQ0cgGOqUBZDW0OkQMIImILVwroL3uSw2xSqxuR%2FDyxj5SdsIm%2BUPtbVlUGT4B7T4kxPKIQx8vaZ7JLPKJyYAFSk5twYig2fwBgfmdDeNf9O%2ByeWv0mBm5rDiBiPd9omrnhSgkiWsF8cP1hjC6zMuLQPAMgCIi5ged3mIei3OVq5o6XM%2BVYzjia7a8syQ02GGJZHkRiOEbAP7s11BeecacUX3FgKbsND17H0J7&X-Amz-Signature=a641bd55358cac88762c6ef6d2bd730c1c571a3d98d0331414ae763157d1e945&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

