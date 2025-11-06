---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STO5A26N%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T030051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDreU6rZDcYwh6fO4EbjCALdv6opjr2rB7DRieNItuOHwIhALbYt5pU334QSxWzD%2BUN8D%2FTYHpx0YJ0lbLSQl%2FgKYM5KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwurSWkoDqcvV5PI98q3ANlaL1PlUvWttnGz6GynX4BmlIOyaG%2FnX0IxMkPsYvfW6w3ht56W7XX4O68FAutcWvB98K1G91hTCGtcCJ8sRxxAqOYtpnVfBDjH9hPwHXspmOA4tjD0fr9Fn8NEKYTy6NDB18aWfsePHQrj91zcM8y3J4Qeoyo45L4Phc0LQg%2FjUf%2FnI4ByE4sqOD9hft8DN9QchNXxQiA4Tty1TEFN4Paa32Q4zwaT43VoYTOJtzxZRNXE7aerd7uyfCJnKImbknGEy%2FQOFfWSsnRSGA06eBqlMs6R8HoRlC%2FA%2BJg%2Bd54VRvmMH0quSyuDlIpwsQuflSfeK%2FnpySSIcGWaMcRYswWLSKXq84W2cXky6rr%2BZDOhgt00PHSzS2b7%2FAWNRkQhwi1S9%2BGUA8p3VK4%2B%2FSNX7rfWYSKEwW8gyu5nQZT6E2fm2GR1x9e83CZiFesawVC1zEfpb7USY442Y%2BhLOUiClU8mCu%2B0V09ynirqC71u6Rzb49oMzzHl3GyNiQEmvTnfP1eEaCqGValttsS80VquEKlqCd72BQ0kqzZASBUne1OBSP2KW1OOytN1VRu%2B1qp4RMRcsOcfGEqt%2B6k%2BzzbJoxXvRaqIWOUZJILdK2LSjwb8cPvJgtAzzKyNPjhtTDN%2F6%2FIBjqkAdIH8eg3fgEZDY%2Fus0HhhdFYX7giRXPuMlouZqyzdkAeHKQh3pfBrm5WfxIFyPkqjcV1VnrH%2B8KCd26Om49QYGkBbgCvT9xW%2BNGVjwVIEo2G2dS%2BI9P%2F3hdxNXEjTudqvi6RucbONHEaLQAx37bksHnjyUeyn56bqzLy0HoXktijMihwR8z6oqaTlnivPQnaJ%2B1QRLIiZzVcNYhDSVREFIUKB5iL&X-Amz-Signature=408fa2448c88c47ec262e65bab63ae335575ce7c3d0f9ea79fbf50b82b990945&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

