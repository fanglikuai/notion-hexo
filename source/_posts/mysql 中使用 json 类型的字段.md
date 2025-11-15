---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VGZU5ZT%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T120102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaSoKrDHp%2FKJW6cQVLPwrvdh%2FwtbgVUooqyH2r95j%2FegIgYGo%2FQkFVTIU%2Fb7WfwzasSNcdMYGmBKo%2Fb53hzcQs1gwq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDOdVqn7KsbyUj2txoircAw32S%2BoUBQwdZ8SJ3EbRbUrnsFdtpHXuHp%2FX6iakrsK%2B8VVZkdmoiB%2FLAAjEVPldHgCMOQC8bvEeURGUphRRAoHF%2FQwkPU1uVXs7vCV8DXOnLcf1ZSUKzJXZ3ZPl18RO%2FwuzwSqHlrbeM2vvSGPdwBelimRuNr3A91C%2FIVKY9LFF0jokqVdjQMxLo%2FHueoUJ3zrsp5bb7UfK%2BeGs5k9dtE2G2%2BFDOqxLtseNM3L6r11b%2FAeReKQ4DCKx5gYyHPNBwz8Ht6Ss76JFspr2er7UC1eH32q3X7iIycAGKvsT6Cp4uw6nkyESjHwSF1Gs5E%2Fq%2FvWmDAVyHvgNhTkIikWUqDYMm8tjdtpcubSqpRQgOMHSw2CDLgAUbcl%2F8imDDTBMMgIkDBa0Q%2B2L%2FFhepdC9CTSYUYy64ELEZ3D5nFXzIzsquTAtJxfnj%2BBfcWzqybnrDN1Fjb8Hqs%2BmkkvwIAzXBNFYf2gVwh5YU4f6e6dwunn%2Fuo6%2BBYvNlYfNM2L8sRHJIgObZEbhyy9%2FWRNIRsLpGTuszrKDx9EYMpfJaJOYAxSr3kYd8tVFR7zGRwarw2E78zabl27NKz7g32UZOaH%2FdalYQunuE2Bo2T1jpyhwzuozSt6oUGCgDL8VZVUXMPKD4cgGOqUBo07YkXd13fkxf8oB%2FGXqi3lPO64S%2F7Ul1UbmOYnFNJFUj7u%2F40Qq9aNHo5kiTeA8oDA9qLsLYYQJ1wnKnXoQWALOkcA7D6gbOs7gXSe2SaHRk70r0Ze3fwz61GRDI5y12kz543eqcVQLaqKR0QuKv%2Bb0O6DzUVoLQBUTBcDwfmpguHpVLhsR94gtrt9Avkp7GnYlsCGlsKpraLm8IDxKr%2BcfHXyd&X-Amz-Signature=e8052aefecc053cc78b6ca8052942e5f205bee293ebddb98b23b6179de37c34d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

