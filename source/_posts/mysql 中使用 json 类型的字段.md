---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7VDMU7B%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqooXaI8hmtb85cX5I2N%2B0BxaXQNAPitHvnFRgPLDuvAIgHdqk8ZYBV5xgTDiSxQjQlqBSv0mWI3fIc4QwC9tguyoqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDORE%2Fz7YyQwTBpm3mSrcA%2FOx3QGfbG%2Fq1dV1Nm8%2FXYEu%2FDVeqbES2aZLKjKTOwS4xyiCH4bku%2FPKtdPThP%2BvHAKNv7b32nrSooh0ERl4ceSHgXksDqjNxTnFAKu8LKSVm8Gy%2FpfBByr8AR%2FrVeRsAMX7uwZhfXWCzj9Wb0NdCD6IqarE1R1kdiMBiq7lNFxsa0TymDjMGfE3MBIu%2FtlQ6y9iBW6LjKDa35vz8VpPRWh6HXRFTnXrGu26Dx2Ty5yRObr2SNI64iAmR1xg5p2YngJGXcpAR%2BAntu4Ewx%2Bkx9TtEQqBo6cpSvQRjtvw6JZPiMn4QmcbjTjKsWwDTreTVZ%2F8%2FP%2BYPLIzb%2Fi9fvPT71DNepsje27n4%2FT7pPyrnFnmmcE54CtjyKIo%2F1tTXebEQuFqx4r7YdrL536CzH%2F8Cf%2BlgmUgA06RTMc98mAjvNl8tsBewBl4X1dNHQXscxFUYtzIp5zJgux10mU41giJn9PoG9xVDKTVja7tKtAtqV88xeoojmFehrsZlOxaOavh0eVc2MXcPsP%2BnLe4ulFfs2J22grQZQBwi0e67I%2Fb5gfnrY%2BWrXhaCbq1i3MM%2BUJn609K%2F7vMLZ8EKI8iXqjCZ2WYuuxBnlIvOmzgqPNamD1ycx%2FeuWd2k8w1hmUqMJrP5MgGOqUBNv5hcho%2BI8G%2BaH3LI8rsvT7r7eJWZZdzS1rGZ9Ml75NwgfymsQ5Rdg7DvjgyOTauBqEWgawd%2BCVdD0ViGLBL7LNWlU91NVVtlCwricb48ee%2FUDvKhKCsQygbYZu7Vdt%2BUacc8p2ymYrBcEzrBlqHDv6pvlMRPPhYLOKEWRHIRk4iRXV5xHvcvhF3tLbdFsa2KrYdc%2Fl8Su4T%2FofZOFZMfvUmdmdA&X-Amz-Signature=427c78a381ada3fef16500f7067923e29e207494be1687dc1c1470c2e40ff395&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

